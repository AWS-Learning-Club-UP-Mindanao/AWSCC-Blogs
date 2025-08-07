---
title: Security with AWS Cognito - Building Your First User Pool
description: This walkthrough will introduce you to Amazon Cognito's user pools and hosted UI service, which is just the beginning of your journey through the Amazon cloud. While this tutorial focused on manual configuration in the console, like with many AWS services, Cognito also covers SDKs and integrations with other frameworks, such as React and Flutter.
date: 2024-11-04
author: precious
categories: [Security]
tags: [Amazon Cognito, Web Development]
pin: false
image: 
  path: /assets/img/security-with-aws-cognito/amazon-cognito-banner.jpg
---

One of the most important features of any application is **user authentication**. It's also almost always the first feature you create when building an application. Yes, you could go about building your authentication and authorization for your web app, but building a secure authentication service is complex and time-consuming and exposes your app and your users to unnecessary risks.

**This is where 3rd-party authentication services come in.** It provides secure authentication, scalability, and a positive user experience. So many services are on the market right now, but we will focus on Amazon Cognito and see what it has to offer.

## But before all that, why choose Amazon Cognito?

[Amazon Cognito](https://aws.amazon.com/cognito/) is an excellent and secure way into user authentication for web and mobile applications, especially when you're already heavy in the AWS ecosystem, since it interacts well not only with API Gateway but also with IAM. Additionally, Cognito is an AWS managed service, so you do not have to worry about your infrastructure or security maintenance, and it scales automatically to handle your growing user base. In terms of pricing, on the other end, for levels of low to medium users, Cognito offers a free tier. If you are just getting started, that would probably be perfect for you.

## What is Amazon Cognito?

Amazon Cognito is the identity platform for web and mobile apps. It's a user directory, an authentication server, and an authorization service for OAuth 2.0 access tokens and AWS credentials. It represents a secure, scalable, and customizable authentication service with easy integration of other AWS services. Let Cognito do the heavy lifting for authentication and authorization, freeing you up to build the important parts of your application.

Amazon Cognito is composed of building blocks such as _User Pools_ and _Identity Pools_.

An **Amazon Cognito user pool** is a completely managed user directory through which authentication and authorization of users to either web or mobile applications is made quite easy. It stores user information in a secured manner, manages user signup and sign-in, and resets passwords. In addition, it supports MFA and social identity provider integration, which enhances security and improves user experience. Conversely, with Cognito User Pools, one would be offloading some complexity in user management and focusing on core features of the application.

## Creating your first user pool

_Note: This tutorial is intended for beginners, as such we’ll be utilizing the AWS Console instead of the CLI. Additionally, I am expecting that you already have your AWS Account ready for use._

Of course, Cognito also allows for use with other frameworks and technologies such as [React](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-test-application-react.html) and [Flutter](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-test-application-flutter.html). But aside from the these, Amazon Cognito also boasts a Hosted UI Service. This hosted UI service is a pre-built user interface provided by Amazon Cognito to handle user authentication and authorization tasks. We will use this feature to test out the Cognito service for the first time.

1\. Go to your [AWS Console](https://console.aws.amazon.com/), and look for [Cognito](https://console.aws.amazon.com/cognito/home).
2\. Click on Create User Pool.
![Desktop View](/assets/img/security-with-aws-cognito/cognito-1.jpg)

- On Step 1, ensure that for authentication providers Cognito User Pool is selected. Also, click on Username and email for the sign-in options.

![Desktop View](/assets/img/security-with-aws-cognito/cognito-2.jpg)

- In Step 2, ensure that Cognito defaults are chosen for the password policy. For MFA, choose Optional MFA, and allow for Authenticator apps and SMS. Also enable self-service account recovery, with email only chosen.

![Desktop View](/assets/img/security-with-aws-cognito/cognito-3.jpg)

3\. Keep everything as default.
Enable self-registration, and allow Cognito to automatically send messages to verify and confirm. For attributes to verify, choose Send email message, and verify email address. For verifying attribute changes, ensure that Keep original attribute value active when an update is pending is checked. For required attributes, ensure that email is chosen by default.

4\. Pick Send email with Cognito.
This is appropriate since we are just testing out the features, but Amazon SES is recommended for use for enterprise-level workloads.

![Desktop View](/assets/img/security-with-aws-cognito/cognito-4.jpg)

For SMS messages, Create a new IAM Role, and name it CognitoSMS or whatever you prefer.

5\. Configure a pool name, you can name it `MyFirstUserPool` or whatever you prefer. Afterwards, click on `Use Cognito Hosted UI`. Then, configure a Cognito domain that you would like to use.

![Desktop View](/assets/img/security-with-aws-cognito/cognito-5.jpg)

For the app client, ensure that the Public client is chosen. For the client name, you can choose the same name for your pool name, or something else. Click on Do not generate client secret. For your callback URL, use `https://[your domain name].auth.us-west-2.amazoncognito.com/auth-callback` (This won't work since we do not have an application, and the information goes nowhere. We're just using this as a placeholder so Cognito allows us to create the user pool.

6\. Lastly, review your choices and click on _Create user pool_.

Once your User pool has been created, you can click on it and navigate to _App integration_. Once you're there, scroll down to your _App clients_ and click on your App client. Navigate to the _Hosted UI_ segment and click on _View Hosted UI_.

![Desktop View](/assets/img/security-with-aws-cognito/cognito-6.jpg)

## Populating your user pool

1\.  Once in your Hosted UI, click Sign Up and fill out your email, username, and password.

![Desktop View](/assets/img/security-with-aws-cognito/cognito-7.jpg)

2\. Cognito will send you a verification code in your email, look for it and enter it on the verification page.

![Desktop View](/assets/img/security-with-aws-cognito/cognito-8.jpg)

3\. Once verified, it will redirect you to the callback URL (which, as mentioned earlier won't do anything).

4\. Go back to your [Cognito console](https://console.aws.amazon.com/cognito/home), and click on your user pool.

5\. Check the user's tab, and look at the user that you signed up earlier.

![Desktop View](/assets/img/security-with-aws-cognito/cognito-9.jpg)

_Note: Signing in will not give any response, as we didn't handle our callbacks properly._

Congrats! You just made your first Amazon Cognito user pool using it's Hosted UI service.

___

This walkthrough introduced you to Amazon Cognito's user pools and hosted UI service, which is just the beginning of your journey through the Amazon cloud. While this tutorial focused on manual configuration in the console, like with many AWS services, Cognito also covers SDKs and integrations with other frameworks, such as [React](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-test-application-react.html) and [Flutter](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-test-application-flutter.html). For more information, you can refer to the official documentation [here](https://docs.aws.amazon.com/cognito/).
