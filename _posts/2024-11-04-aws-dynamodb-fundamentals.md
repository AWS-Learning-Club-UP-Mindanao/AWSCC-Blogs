---
title: Get Started with NoSQL - AWS DynamoDB Fundamentals
description: In this short article, we will explore the fundamentals of AWS DynamoDB, specifically on creating a table, inserting and updating data, all the while using the command-line interface to perform such actions. DynamoDB is a good choice for developers trying to produce quick, dependable, and serverless apps. It is because of its high availability, scalability, and seamless connection with other AWS services.
date: 2024-11-04
author: justin
categories: [Databases]
tags: [NoSQL, Amazon DynamoDB]
pin: false
image: 
  path: /assets/img/aws-dynamodb-fundamentals/banner.jpg
---
Behind the scenes of most modern applications is a vital component that brings websites and apps to life, the database. **Databases** are in charge of storing all the different data and information that a web application uses. It is relied upon to store, retrieve, update, and query data for various purposes.

In the programming world, one of the most important types of database is a non-relational database, often known as **NoSQL databases**. These are typically used to store flexible, dynamic data that needs quick access. It is a very useful tool for web applications that require fast and flexible data exchange between the web server and the database.

But here's the problem: dealing with databases is frequently regarded as **one of the most difficult** aspects of development. In fact, it is an established joke in the programming community that the _backend_ side of development, where database maintenance occurs, is so much more difficult than the, often more visually satisfying, _frontend_.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k8w2pwmeqdofd2ntecqv.jpg)

---

So then, this is where **Amazon Web Services (AWS)** comes in. Allowing us to worry no more, introducing their serverless, NoSQL, fully managed database: **AWS DynamoDB**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b2nw3dqd3qnuofnv5rpa.png)
---

## What is AWS DynamoDB

DynamoDB is a scalable NoSQL database service that is completely managed and programmed for fast and consistent performance, **with response times measured in single-digit milliseconds**. Since its creation in 2012, it has been an excellent match for modern serverless apps, catering to businesses of all sizes. DynamoDB effectively handles **large workloads**, handling over a billion queries per hour and accommodating table sizes more than 200 TB, all while providing **continuous availability and resiliency**.

---

## Why use AWS DynamoDB?

- **High Availability and Resilience with Global Tables**: DynamoDB offers global tables that enable multi-active data replication across selected AWS Regions, ensuring 99.999% availability. This feature allows applications to access data locally, maintaining high performance in data delivery. This also avoids the probability of server downfalls due to its multiple tables around regions.

- **Service Integrations**: DynamoDB seamlessly integrates with various AWS services, facilitating the development of serverless applications. It integrates with AWS Lambda to automatically respond to events, Amazon S3 for simple data import and export, and Amazon Redshift for easy analytics without needing to move the data around. Having this feature provides your application easy access to other AWS technologies to assist you in development.

---

## When to use AWS DynamoDB?

- Need fast and reliable access to data for real-time applications, like finance or gaming.
- Want to reduce management tasks with automatic scaling and easy setup.
- Require low-latency access to data from different locations around the world.
- Need to handle changing amounts of traffic smoothly, like during busy gaming sessions or streaming events.
- Want strong data safety with features that ensure everything stays consistent and reliable.
- Need a system that grows easily as your data and user demands increase

---

## Intro to AWS DynamoDB (via CLI)

### Prerequisites

**Step 1**. Make sure to install AWS CLI
[Install AWS CLI](https://aws.amazon.com/cli/)

**Step 2**. Configure your IAM credentials in AWS CLI
[Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

---

### Creating a Table

The first action that you would do is to create a table. So, assuming we want to store data about video games. You would create a _Games_ table in DynamoDB with the following details:

- **Partition key** — Developer
- **Sort key** — GameTitle

(Windows CMD):

```bash
aws dynamodb create-table ^
    --table-name Games ^
    --attribute-definitions ^
        AttributeName=Developer,AttributeType=S ^
        AttributeName=GameTitle,AttributeType=S ^
    --key-schema ^
        AttributeName=Developer,KeyType=HASH ^
        AttributeName=GameTitle,KeyType=RANGE ^
    --provisioned-throughput ^
        ReadCapacityUnits=5,WriteCapacityUnits=5 ^
    --table-class STANDARD
```

Output:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/f0m33tnmrfvtboy5amzl.png)

---

### Writing Data

After creating a table, you can then also insert several items into the _Games_ table that you created.

(Windows CMD):

```bash
aws dynamodb put-item ^
    --table-name Games ^
    --item ^
        "{\"Developer\": {\"S\": \"Epic Games\"}, \"GameTitle\": {\"S\": \"Fortnite\"}, \"Genre\": {\"S\": \"Battle Royale\"}, \"Rating\": {\"N\": \"9\"}}"

aws dynamodb put-item ^
    --table-name Games ^
    --item ^
        "{\"Developer\": {\"S\": \"Rockstar Games\"}, \"GameTitle\": {\"S\": \"Grand Theft Auto V\"}, \"Genre\": {\"S\": \"Action-Adventure\"}, \"Rating\": {\"N\": \"10\"}}"

aws dynamodb put-item ^
    --table-name Games ^
    --item ^
        "{\"Developer\": {\"S\": \"Valve\"}, \"GameTitle\": {\"S\": \"Half-Life 2\"}, \"Genre\": {\"S\": \"First-Person Shooter\"}, \"Rating\": {\"N\": \"9\"}}"

aws dynamodb put-item ^
    --table-name Games ^
    --item ^
        "{\"Developer\": {\"S\": \"Naughty Dog\"}, \"GameTitle\": {\"S\": \"The Last of Us\"}, \"Genre\": {\"S\": \"Action-Adventure\"}, \"Rating\": {\"N\": \"10\"}}"
```

---

### Reading Data

If you'd like to retrieve data about the data you wrote, you can also read the items that you created in the table.

`--consistent-read` refers to the latest changes of your table

(Windows CMD):

```bash
aws dynamodb get-item --consistent-read ^
    --table-name Games ^
    --key "{\"Developer\": {\"S\": \"Valve\"}, \"GameTitle\": {\"S\": \"Half-Life 2\"}}"
```

Output:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8mu1gjokrk6po0zugpsn.png)

---

### Updating Data

And of course! If you can write and read, you can also update existing data!

(Windows CMD):

```bash
aws dynamodb update-item ^
    --table-name Games ^
    --key "{\"Developer\": {\"S\": \"Valve\"}, \"GameTitle\": {\"S\": \"Half-Life 2\"}}" ^
    --update-expression "SET Genre = :newval" ^
    --expression-attribute-values "{\":newval\":{\"S\":\"First-Person Shooter, Puzzle\"}}" ^
    --return-values ALL_NEW
```

Output:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h29ombr96765jvv0svdf.png)

---

### Query Data

With your existing table _Games_, you can query the data that you wrote  by specifying _Developer_. This will display all games that are associated with the partition key: _Developer_.

(Windows CMD):

``` bash
aws dynamodb query ^
    --table-name Games ^
    --key-condition-expression "Developer = :name" ^
    --expression-attribute-values "{\":name\":{\"S\":\"Naughty Dog\"}}"
```

Output:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g2puckmupmv8cm4ktmnw.png)

---

### Deleting a Table

Lastly, this step helps ensure that you aren't charged for resources that you aren't using by deleting the table you created.

(Windows CMD):

```bash
aws dynamodb delete-table --table-name Games
```

Output:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sy67qu6gsu5owy5h5kw5.png)

---

## Conclusion

In this short article, you have learned the fundamentals of AWS DynamoDB, specifically on creating a table, inserting and updating data, all the while using the command-line interface to perform such actions. DynamoDB is a good choice for developers trying to produce quick, dependable, and serverless apps. It is because of its high availability, scalability, and seamless connection with other AWS services. You are now ready to implement NoSQL on your applications. You may learn more about AWS DynamoDB in their official documentation below:

<https://docs.aws.amazon.com/dynamodb/?icmpid=docs_homepage_featuredsvcs>
