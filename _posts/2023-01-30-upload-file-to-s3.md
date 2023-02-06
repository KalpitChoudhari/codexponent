---
layout: post
title:  "Direct upload files to S3 - Ruby Code"
author: Kalpit
categories: [ Ruby, AWS ]
image: assets/images/aws_s3.png
absolute_url: ruby-s3-file-upload
---
In this blog, we will see how one can directly upload file to S3 bucket.

Firstly, you’ll need an AWS account and remember that S3 (Simple Storage Service) is not free service. You need to pay as per your usage.

Following are the charges dated on 28/01/2023:

![AWS s3 Rates](/assets/images/s3_rates.png)

Now let’s start step by step:

1. First, you need to sign in to your AWS account and search for S3 and click on create bucket.
2. After clicking on create bucket, you need to enter bucket name as per your use. Here i’ll be entering `demofordirectupload` as bucket name as follows:

![Create s3 bucket](/assets/images/create_bucket.png)

> Note that bucket name should be globally unique and must not contain spaces or uppercase letters.    

Click on create bucket as we are not changing any default configuration.

1. Now that we have successfully created S3 bucket, let’s move to ruby code which will directly upload file to created bucket.
2. In order to upload file from ruby code, we’ll need following info
    1. AWS_SECRET_ACCESS_KEY and AWS_ACCESS_KEY_ID
    2. bucket name
    3. AWS region
3. In order to generate AWS secret keys, I’ve created new user and generated access keys inside IAM Security Credentials.
    
    Important note. You need to give permission to upload s3 file from permissions in IAM dashboard. You can create new policy and attach that policy to user in order to grant permission.
    

Code:

```ruby
require 'aws-sdk-s3'

AWS_ACCESS_KEY_ID = "SOMERANDOMGENERATEDSTRING"
AWS_SECRET_ACCESS_KEY = "MORELENGTHYRAMDOMEGENERATEDSTRING"
REGION = "us-east-2"
BUCKET = "bucketname"

def get_s3_client
  Aws::S3::Client.new(
    access_key_id: AWS_ACCESS_KEY_ID,
    secret_access_key: AWS_SECRET_ACCESS_KEY,
    region: REGION,
  )
end

def direct_upload
  s3 = get_s3_client

  path = "FILE_PATH"

  File.open(path, 'rb') do |file|
    p 'Started uploading file to S3'
    response = s3.put_object(
      bucket: BUCKET,
      key: path,
      body: file,
      acl: 'bucket-owner-full-control'
    )
    p response
  end
  puts "File should be available at https://#{BUCKET}.s3.#{REGION}.amazonaws.com/#{path}"
end

direct_upload
```

In above code, please replace `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` with your keys and same goes for bucket.

After you’ve replaced these things, you’ll see output as:

![s3 Console Output](/assets/images/s3_console_output.png)

![s3 Bucket Output](/assets/images/s3_bucket_output.png)

We can see uploaded file as above 👆🏼. Cheers 🥳
