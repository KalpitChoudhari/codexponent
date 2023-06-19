---
layout: post
title:  "Rails 7 excluding on ActiveRecord::Relation"
author: Mayuresh
categories: [ Redis ]
image: assets/images/redis.png
---

# **Migrating from an Unencrypted Redis Cluster to an Encrypted Redis Cluster**

AWS provides two types of encryption for Redis clusters: at-rest encryption and in-transit encryption. Let's explore what each of these means and how to migrate your Redis cluster to enable encryption.

### **What is At-Rest Encryption?**

At-rest encryption ensures that data is encrypted when it is stored on disk. In the context of Redis on AWS, it involves leveraging AWS Key Management Service (KMS) to manage the encryption keys. This provides an additional layer of security for your data.

### **What is In-Transit Encryption?**

In-transit encryption, on the other hand, focuses on securing data while it is being transmitted between clients and the Redis cluster. AWS offers different mechanisms, such as Transport Layer Security (TLS) and Secure Sockets Layer (SSL), to establish a secure connection between the client and the Redis cluster. This prevents unauthorized access or tampering of data during transit.

### **How to Perform the Migration**

To migrate from an unencrypted Redis cluster to an encrypted Redis cluster, follow these steps:

1. **Take a backup of the existing unencrypted Redis cluster**: It's important to have a backup of your Redis data before proceeding with any changes. This will ensure your data is safe and can be restored.
2. **Restore the backup to create a new Redis cluster**: In the AWS Management Console, navigate to the backup tab of your Redis cluster and restore the previously taken backup. This process will create a new Redis cluster with the same data.
3. **Enable encryption for the new Redis cluster**: While creating the new Redis cluster, make sure to enable both at-rest and in-transit encryption. You can do this by modifying the cluster configuration and specifying the appropriate encryption settings. This will ensure that the new Redis cluster is encrypted and provides enhanced security for your data.
4. **Update the Redis cluster URL in your application**: After creating the encrypted Redis cluster, you need to update the connection URL in your application. The URL should now include the necessary settings for in-transit encryption. Specifically, change the protocol from **`redis://`** to **`rediss://`**  it’s just like `**http**://` to `**https**://` or you can pass `**config.redis = { url: REDIS_URL, ssl: :true }**`
5. **Validate the migration**: Test the connection to your application's newly encrypted Redis cluster to ensure that the migration was successful. Verify that data is being securely transmitted and that your application can interact with the Redis cluster as expected.

### **Conclusion**

Migrating from an unencrypted Redis cluster to an encrypted Redis cluster on AWS involves enabling both at-rest and in-transit encryption. By following the outlined steps, you can enhance the security of your Redis data by encrypting it at rest and securing its transmission between clients and the Redis cluster. This helps protect your sensitive information and ensures compliance with data security best practices.
