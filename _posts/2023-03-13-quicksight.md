---
layout: post
title:  "How to Integrate QuickSight into a Rails React Application"
author: Mayuresh
categories: [ AWS, Rails ]
image: assets/images/aws_quicksight.png
---

QuickSight, a business intelligence service by Amazon Web Services (AWS), provides a cloud-based data visualization and analytics platform.

To integrate QuickSight into a Rails React application, you can follow the below steps:

- First of all, you need to create a dashboard on quicksight
- create a cognito pool and associate it with the quicksight
- add `gem 'aws-sdk-quicksight’` and `gem 'aws-sdk-cognitoidentity’` to your gemfile

   To associate cognito with quicksight  go to ‘Security & permissions’

   Click on the "Add or Remove" button next to "Amazon Cognito user pools".

- Now add quicksight embedder service which will fetch the dashboard URL

```ruby
require 'aws-sdk-quicksight'
require 'aws-sdk-cognitoidentity'
class QuicksightEmbedder
  attr_reader :user
  def initialize
    @user = User.current
  end

  def get_dashboard_url
    assume_role_resp = assume_cognito_role
    qs_client_configs = set_quicksight_client_configs(assume_role_resp)
    qs_client = Aws::QuickSight::Client.new(qs_client_configs)
    register_user_in_quicksight(qs_client)
    result = get_embed_url(qs_client)
    render json: { embed_url: result.embed_url, status: result.status }
  end

  private

  def get_embed_url(qs_client)
    dashboard_params = {
      aws_account_id: ENV['AWS_ACCOUNT_ID'],
      dashboard_id: *dashboard_id,
      identity_type: 'IAM',
      session_lifetime_in_minutes: 300
    }
    qs_client.get_dashboard_embed_url(dashboard_params)
  end

  def assume_cognito_role
    client = Aws::CognitoIdentity::Client.new({ region: *AWS_REGION })
    # Note that the client.get_id call here is what creates a new Cognito user.
    get_id_response = client.get_id({ identity_pool_id: *cognito_identity_pool_id })
    token_resp = client.get_open_id_token({
                                            identity_id: get_id_response.identity_id
                                          })
    configs = {
      credentials: Aws::Credentials.new(
        ENV['AWS_ACCESS_KEY_ID'],
        ENV['AWS_SECRET_ACCESS_KEY']
      ),
      region: 'us-east-1'
    }
    sts = Aws::STS::Client.new(configs)
    assume_role_params = {
      role_arn: cognito_identity_role_arn,
      role_session_name: @user.email,
      web_identity_token: token_resp.token
    }
    # Now we assume the identity pool role:
    sts.assume_role_with_web_identity(assume_role_params)
  end

  def cognito_identity_role_arn
    *you can find this arn in cognito pool
  end

  def set_quicksight_client_configs(assume_role_resp)
    Aws.config.update({
                        access_key_id: assume_role_resp.credentials.access_key_id,
                        secret_access_key: assume_role_resp.credentials.secret_access_key,
                        session_token: assume_role_resp.credentials.session_token,
                        region: *region
                      })
    {
      credentials: assume_role_resp,
      region: *region
    }
  end

  def register_user_in_quicksight(qs_client)
    qs_client.register_user({
      identity_type: 'IAM',
      email: user.email,
      user_role: 'READER',
      iam_arn: cognito_identity_role_arn,
      session_name: user.email,
      aws_account_id: ENV['AWS_ACCOUNT_ID'],
      namespace: 'default'
    })
  end
end
```

This method will give you dashboard ID from which you can fetch the quicksight dashboard

- Then create a react component and call this `get_dashboard_url` method
- Component is as follows:-

```ruby
import React, { useEffect, useState } from 'react';

const QuicksightDashboard = () => {
  useSetBreadCrumb([{ name: 'Quicksight Dashboard' }]);
  const [EmbedURL, setEmbedURL] = useState('');
  useEffect(() => {
    getRequest({
      url: '/quicksight_dashboard/get_dashboard_url',
    }).then(data => {
      setEmbedURL(data.embed_url);
    });
  }, []);

  return (
    <div>
      <div id="embeddingContainer" />
      <iframe
        title="quicksightdashboard"
        style={{ width: '100vw', height: '100vh' }}
        src={EmbedURL}
      />
    </div>
  );
};

export default QuicksightDashboard;
```

this component will hit a request and get the dashboard URL and then it will show it on the page using an iframe 

Integrating QuickSight into a Rails React application can be a great way to add powerful data visualization capabilities to your web application. With the use of the AWS SDKs and the QuickSight Embedding API, you can easily fetch the dashboard URL and display the dashboard using an iframe in your application.

References:-

1. [QuickSight Developer Guide](https://docs.aws.amazon.com/quicksight/latest/APIReference/Welcome.html):
2. [AWS SDK for Ruby - QuickSight](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/QuickSight.html)
3. [AWS SDK for Ruby - CognitoIdentity](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/CognitoIdentity.html)
4. [QuickSight Embedding API](https://docs.aws.amazon.com/quicksight/latest/user/embedded-dashboards.html)
