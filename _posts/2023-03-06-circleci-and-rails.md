---
layout: post
title:  "Continuous integration and deployment with CircleCI and Ruby on Rails"
author: Mayuresh
categories: [ CircleCI, Rails ]
image: assets/images/circleciror.png
---

This blog post will explore how to implement Continuous Integration/Continuous Deployment (CI/CD) using CircleCI and Ruby on Rails.

CircleCI also has great integration with Ruby on Rails, with pre-built Docker images for popular versions of Ruby and Rails, as well as integrations with popular testing frameworks such as RSpec and Capybara.

## Setting up CircleCI

- Go to the CircleCI website and sign up for an account.
- Connect your GitHub account to CircleCI.
- Once your GitHub account is connected, CircleCI will automatically detect your repositories. Click on the repository you want to set up for continuous integration and deployment.
- CircleCI will create a default configuration file for you in the '.circleci' directory of your repository. You can customize this file according to your requirements.

Here is an example of a configuration file of CircleCi

```yaml
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/ruby:version
    working_directory: ~/repo
    steps:
      - checkout
      - run: gem install bundler
      - run: bundle install
      - run: bundle exec rake db:create db:migrate
      - run: bundle exec rspec
  deploy:
    docker:
      - image: google/cloud-sdk:alpine
    steps:
      - run:
          name: deploy
					command: deployment.config
    requires:
      - build
    filters:
      branches:
        only:
          - main
```

The **`build`  j**ob is responsible for building and testing the application. It uses the **circleci/ruby: version** Docker image and installs the necessary dependencies using **bundle install** It then runs database migrations and runs the RSpec tests.

The **`deploy`** job is responsible for deploying the application to any where like Kubernetes or google cloud for which you can use a particular configuration for deployment

- Note:- We had used rok8s scripts for deployment

The **`requires`** directive ensures that the **deploy** job only runs after the **build** job has been completed successfully. 

The **filters** directive ensures that the pipeline only runs for changes to the **main** branch.

This directive is intended to be used when there are two types of deployments: `staging` and `production`.

- Note that this is just one example configuration and you may need to customize it depending on your specific requirements and setup.

CircleCI allows you to run jobs in parallel, which can significantly reduce the time it takes to run your pipeline. Parallelism can be added to your CircleCI configuration file using the **`parallelism`**
 keyword, which specifies the number of parallel containers to use for a job. For example:

```yaml
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/ruby:2.7.4
    parallelism: 3
    steps:
      - checkout
      - run: gem install bundler
      - run: bundle install
      - run: bundle exec rake db:create db:migrate
      - run: bundle exec rspec
```

In this example, the **`build`**
 job is configured to run with a parallelism of 3, which means that three parallel containers will be used to run the job. This can greatly speed up the time it takes to run tests or other tasks that can be run in parallel.

In summary, implementing CI/CD with CircleCI and Ruby on Rails can greatly improve the efficiency and reliability of your software development process. Automated testing and deployment ensure that your application is working as expected, allowing for quick delivery of updates to your users. By following the steps outlined in this document, you can easily set up your own CI/CD pipeline and take advantage of its many benefits.

**References:**

- [CircleCI documentation](https://circleci.com/docs/)
- [Ruby on Rails documentation](https://rubyonrails.org/)
