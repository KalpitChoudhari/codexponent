---
layout: post
title:  "How to Securely Store RDS Instance Passwords in AWS Secret Manager and Access Them in a Ruby on Rails Application"
author: Mayuresh
categories: [ Ruby, Rails, AWS ]
image: assets/images/secret_manager.png
---

AWS Secret Manager is a service offered by Amazon Web Services (AWS) that enables you to manage secrets such as database passwords, API keys, and other sensitive data. This blog will discuss the steps to set up AWS Secret Manager.

## Step 1: Create a Secret

To create a secret in AWS Secret Manager, follow the steps below:

1. Sign in to the AWS Management Console
2. Go to the AWS Secret Manager dashboard
3. Click "Store a new secret"
4. the secret type will be `Credentials for Amazon RDS database`
5. Enter the secret value
6. Add a key-value pair to tag the secret (optional)
7. Click "Next"
8. Enter a name for the secret
9. Choose the encryption key
10. Click "Next"
11. Review the details and click "Store secret"

## Step 2: Access the Secret in ruby on rails application

To access the secret in a Ruby on Rails application, you can use the AWS SDK for Ruby. First, install the SDK by adding the `aws-sdk-secretsmanager` gem to your Gemfile and running `bundle install`. 

In your Rails app, create an initializer file to load the AWS credentials and region from environment variables, and then use the AWS SDK to retrieve the RDS instance password from Secret Manager.

```ruby
# config/initializers/aws.rb

Aws.config.update({
  region: ENV['AWS_REGION'],
  credentials: Aws::Credentials.new(
    ENV['AWS_ACCESS_KEY_ID'],
    ENV['AWS_SECRET_ACCESS_KEY']
  )
})

# Retrieve RDS password from Secret Manager
secrets_manager = Aws::SecretsManager::Client.new
secret_value = secrets_manager.get_secret_value({ secret_id: 'my-rds-password' })
RDS_PASSWORD = secret_value.secret_string
```

or you can fetch the passwords and save it in your environment variables

code is as follows :-

```ruby
#decalre region_name and secret_name statically or dynamically 
client = Aws::SecretsManager::Client.new(region: region_name)
get_secret_value_response = client.get_secret_value(secret_id: secret_name)
secret_json = get_secret_value_response.secret_string
secret_hash = JSON.parse(secret_json)
ENV['DATABASE_HOST'] = secret_hash['host']
ENV['DATABASE_USERNAME'] = secret_hash['username']
ENV['DATABASE_PASSWORD'] = secret_hash['password']
```

## Conclusion

AWS Secret Manager is a powerful tool for managing sensitive data in your applications. By following the steps outlined in this document, you can easily set up, access, and use secrets in your applications.
