---
layout: post
title:  "Setup Automatic Rotation for RDS using Secret Manager in AWS"
author: Mayuresh
categories: [ Ruby, Rails ]
image: assets/images/rds_rotation.png
---

## **Introduction**

Automatic password rotation is a crucial security practice that helps protect your Amazon RDS (Relational Database Service) instances from potential threats. AWS Secrets Manager provides a convenient way to automate the rotation process for RDS passwords. In this guide, we'll walk you through the steps to set up automatic password rotation using AWS Secrets Manager, ensuring that your RDS instances remain secure.

To begin, you'll need to create a secret in AWS Secrets Manager. If you're unfamiliar with this process, you can refer to the **[Codexponent blog](https://codexponent.com/aws-secret-manager/)** for detailed instructions on creating an AWS secret.

## **Step-by-Step Instructions**

1. Go to the AWS Secrets Manager console and locate the secret you created in the previous step. Choose the secret and click on "Edit rotation."
2. Enable automatic rotation by toggling the switch to the "On" position.
3. Specify the rotation schedule that suits your requirements. You can choose the frequency at which the secrets should be rotated, such as every month, every three months, etc.
4. Next, you have two options for the rotation function. You can either use the AWS rotation functions provided by default or create a custom rotation function. For simplicity, we'll focus on using the built-in AWS rotation functions.
5. Select the option "Use a rotation function from your account" and choose "Create a rotation function." Enter the name of the appropriate template based on your RDS engine and setup. You can find a list of available rotation templates in the **[AWS Secrets Manager documentation](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_available-rotation-templates.html)**.
6. If you have separate credentials for rotating this secret, choose "Yes" and provide the necessary details. Otherwise, choose "No."

Following these steps will create a Lambda function based on the selected template. This function will automatically rotate the passwords for your Amazon RDS PostgreSQL instance according to the defined schedule.

## **Conclusion**

Automatic password rotation using AWS Secrets Manager simplifies the process of securing your Amazon RDS instances by regularly rotating the passwords. By leveraging the power of AWS Secrets Manager and Lambda functions, you can ensure that your RDS passwords are updated on a scheduled basis, minimizing the risk of unauthorized access.

Remember to test the rotation process thoroughly and verify that your applications can still access the RDS instance after the password has been rotated. With this setup, you can have peace of mind knowing that your RDS passwords are regularly changed and your data remains secure.

For more details and additional options, you can explore the **[GitHub repository](https://github.com/aws-samples/aws-secrets-manager-rotation-lambdas)** provided by AWS for automatic rotation configurations.

Now you're ready to implement automatic password rotation for your RDS instances using AWS Secrets Manager. Stay proactive and safeguard your AWS resources with regular password updates!
