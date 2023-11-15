# Data-Engineering-on-AWS


# Introduction & Goals
- Hello my name is Dennis and welcome to my Data Engineering journey. For this project, I will be handling a complete data engineering project for a client on cloud platform. For this project, I will be using AWS Cloud service so let’s get the job done.
-I will build a platform, build pipelines and this will be showing the life cycle of a data engineering Job. I will start with an introduction to data engineering, if you're new to data engineering, you will get to understand the whole concept and how it works. Then we're going to go over the data set we will use for the project, it's super nice to work with and you're going to like it. Then what we're going to build a platform on top of it using some tools. 
-Next we're going to build a Pipelines, the first pipeline we're going to build is a data injection pipeline with API, Lambda and kinesis. Then we're going to build streaming pipelines to stream raw data to raw storage S3. We're going to build storage to Dynamo DB, visualization for DynamoDB with an API.  We're going to also do some streaming to redshift and a batch processing pipeline that is for bulk imports 
-Will start with the basis of Aws from scratch, then things to keep in mind, and lastly build these pipelines and after that finish with a conclusion and what I have learned. Let’s have fun

# Definition of Data Engineering
What is data engineering, what's the purpose of data engineering Where's the difference to software engineer and a data scientist? 

## Purpose 
 - Data engineering is simply making data usable for the business and accessible for data scientists for business intelligence for analysts 
•-The idea here is in contrast to a data scientist who is doing the analytics of the data, the data engineer is actually working the data and making sure that people who need it can access it and work with it.  

## What a data engineer is Doing:
- Building platform’s 

-	Data modeling for storage and for processing because data is going to be accessed differently, in different ways for these other people to use
-	Selecting and setting up tools
-	Build Pipeline
  
## The importance of the cloud: 
Data Engineering is a very versatile field and a lot of that is happening in the cloud. Of course people are still working on premise but when you when you look at the trends and what people are doing or where they moving and where it's going, the cloud is getting more and more important 
- Sheer number of tools. There are so many options and platforms it's everything is software as a service so you don't even need to set up the tools so you don't need to install them and configure them, it's all running and ready you just need to integrated with them. We're also going to use some of these tools like Kinesis and, API gateway and lambda functions in this project
-	Scaling, when you think about hardware, how do you scale hardware? you buy more hardware on premise but when you're in the cloud, you’re in a virtual environment and scaling is very easy. You just need to make an addition and add resources to it and if you're on a software as a service tool then it's even simpler you don't you don't really need to provision hardware virtual hardware anymore it's just scales very easy and very quick now 
-	Set up time is super low 
-	Pricing is a plus for beginner’s or start up type situations where your workload is not that high and you are trying to make something work with. On-premise its high 

# Data Science Platform
Data Science platform is one of the things that are important for data engineers. For data science in general you need a good platform to work with and how does how does such a platform look? Below is print out of it. We have a few stages but without actually naming the tools we're coming to that later

![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/013bd465-f623-42e7-b378-b37a5cacf96c)


# Contents

- [The Data Set](#the-data-set)
- [Used Tools](#used-tools)
  - [Connect](#connect)
  - [Buffer](#buffer)
  - [Processing](#processing)
  - [Storage](#storage)
  - [Visualization](#visualization)
- [Pipelines](#pipelines)
  - [Stream Processing](#stream-processing)
    - [Storing Data Stream](#storing-data-stream)
    - [Processing Data Stream](#processing-data-stream)
  - [Batch Processing](#batch-processing)
  - [Visualizations](#visualizations)
- [Demo](#demo)
- [Conclusion](#conclusion)
- [Follow Me On](#follow-me-on)
- [Appendix](#appendix)

# The Data Set
## Data Types we are going to be working with
First we have a look at the data type a data engineer is going to handle on a day to day basis. For this project will be working with Jason files

 
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/230a62f3-a125-475c-8045-2d9a3aa51739)


##  What Is a Good Dataset 
Below are some of the characteristics of the dataset I will be working with on this project and the reasons why I chose to work with this dataset
 
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/b922e32d-04a6-4423-b2ee-6824d20a8144)


## The Dataset I am using:

The data set I am using id from kaggle below is the link: 
[https://www.kaggle.com/datasets/carrie1/ecommerce-data]

 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/154e9fd4-a573-410d-b8c1-1b4b0c643b2a)


## Defining The Purpose

The purpose of the project can be divided into two as shown below:

![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/69883495-b02e-49e1-bedd-2ba6d977d82a)

For this project, we will focus on the main goal part of the above section and that is for the user and business interest. An example can be giving customers access to their purchase history. So how can you model this? There are two approaches one can take.

a)	Relational Storage Possibilities

 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/ca9920b7-ffaf-4108-a8c2-17659194710a)


Depending on how the data looks, relational database is one approach where you create tables for different entities. eg create a customer table, then invoice table and stock table. It’s a one to end relationship. One customer can have many invoices but not the reverse, also one invoice can have multiple items, but an item cannot belong to multiple invoices. This an end to end relationship and thus create another helper table inv/stock table that has the invoice id and stock code id which helps to crate this end to end relationship. In total you will have 4 tables. This can work on small data set and might be difficult to handle large datasets. But there is another option that can handle large datasets



b) NoSQL Storage Possibilities

![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/35e2a48e-9be7-4a2c-b594-7d3d3a516937)

 
 This is wide column store where you can have millions of columns. There are two ways of looking at this. The customer purchase overview, where you have the customers invoice and what he/she has bought. The other table is the invoice table, which shows what the customer has bought on the invoice. This is what we are going to use this option and build two tables in DynamoDB


# Used Tools
## Selecting The Tools
 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/b3bf429e-832e-4d24-af7b-5c82d85ad987)


Above as the tools we are going to be working with. Also below is a detail of each tool that we are going to be working with

## Client
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/b96e5bb2-01db-4bc4-8ab7-65cb2e43fa21)

We will work with csv file representing the client and convert it into a json file for easy flow. Then the json and is then sent to our api
## Connect
 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/e50c2e46-8137-4741-9daa-7383d6b0485c)

The client file is sending file to api gateway that hosted on URL and using lambda code to send it to some system
## Buffer
 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/def1350d-8278-4087-98b6-2ee49b941a12)

In Kinesis a massage que or Kafka, we have the producer and the consumers. The producer sends the data into the massage que and the consumer takes data out of the massage que mass
## Processing
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/c84bf2cc-7a48-40ac-ba2c-704be7d9d76e)

We have two types of process, first is the streaming process. Here we have the source sending data to processing, then data is processed to destination. The source can be Kinesis, and the processing is a lambda function that is triggered by a new kinesis record, and puts it Ina destination
For batch processing, its starts with the scheduler, data is put in kinesis, or s3 and then a scheduler is going to start to activate the processing which connects to the data source and pull the data to the destination.
## Storage
 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/1c76dc47-c94d-46c1-984d-532c59659e20)

We a going to use S3 storage, DynamoDB and Redshift data warehouse
## Visualization
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/9123308d-dae4-4b54-aeb3-419e113b537f)

We can use APIS and Tableau to visualize and create dashboards.

# Pipelines
## Development Environment
## Data Ingestion Pipeline
 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/2c4bc623-2cb7-41b6-bc2b-c89a0a69c340)

                                              
### Create Lambda For API
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/4a6e3d33-6753-4ce8-9ec4-d6f83ab1cac0)

 ### Create API Gateway
 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/c1dad293-9157-4f2a-9181-dd68b79c62c3)

![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/090dbf35-b9dd-4a1f-9913-aec372b9ded6)

### Set up Kinesis
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/901d5db2-c713-4fa9-bea8-89e6ef6de625)

### Set up IAM for API
 
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/0a6f0635-a4b2-43de-b74d-35c956fe15bb)

![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/09e06a94-2532-4aed-b935-2d91f5dec138)

### Create Ingestion Pipeline (Code)

#### a)Convert csv file to json file
 
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/05d39732-e212-42d2-bb59-42cab7b86e50)

#### The output code:
 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/9cf15954-61bd-44b4-88ae-8bf121b2991a)

 ![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/c5b275c7-92a1-4eea-a90b-9030a7728014)

![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/20f1c802-99a9-45ae-9ea7-9e5d35e7988e)

#### Create Script to Send Data then we  Test The Pipeline
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/c01e80c5-e229-4cf7-b37c-21bfc47ca537)

#### Output:
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/94500fb4-c4fa-462e-8503-7f533541b5b2)

then we go to Cloudwatch in aws and check the logs and as shown below they have been found and written

![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/ae96d623-0c18-465a-addd-c454969e30cf)


![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/297e4208-21b2-4edf-aa9c-9607f2facf94)

and when we look into the kinesis data stream:


#Stream Processing
## Stream To Raw S3 Storage Pipeline
### create an s3 bucket

![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/3c23483d-3cf3-40a9-bdc7-3f9ac927c144)

### Configure IAM For S3
![image](https://github.com/DataMan7/Data-Engineering-on-AWS/assets/71377859/67d8c69d-af4c-46c3-98a4-2f9e88075be4)

###  Create Lambda For S3 Insert
Link to the code:









