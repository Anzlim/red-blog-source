---
title: "Getting started with AppSync"
summary: "Learn Basics of aws AppSync which is an GraphQL Engine"
date: 2021-11-01T19:17:56+05:30
draft: false
tags: ["aws","AppSync"]
---

## Preface

This guide is written for people who want to get started with appsync. Don’t want to get into details of documentation of this service. And wanted to have a look around appsync without actually understanding it completely.

This Guide has no intention of making the reader capable of working on this service asap.

But to Help get understand the Applications of this service. Getting Familiar with the service. And Exploring various possible ways of using the AppSync Service.


## Getting the fundamentals clear

Appsync is one of the best services provided by AWS for building small to medium size backend. It covers almost all the parts to build an end to end backend.

So what really is **Appsync**? We can have more than one definition based on our requirements. If we wanted to use it for filling the need of an api then it’s a perfect alternative to API gateway. Or if we wanted some logic, then it provides computation using resolvers, more details on resolvers later.

Or if you wanted to use **Graph QL** api then this is the best managed graph QL engine.

For me I had chosen appsync for realtime architecture needs.
Yes it supports real time data transfer with basic filtering. You can connect to other services of AWS as well as any service in the world using http resolver.

In a nutshell appsync is a managed graph QL engine with many features like authentication, realtime data (wss - websocket) support & a little bit of computation built-in using VTL (Velocity template language) language in resolver.


### Who should Consider Appsync

- If you want to build a real time application for example a chat application.
- If you are good at graph QL & wanted to use Graph QL api in your project
- If your project is not too complex or big
- If you wanted to use AWS service only
- If you wanted to build something is less time

### Who should not care about appsync

- If your project is too complex
- If you don't need real time capability in your application
- If rest api fulfills your requirements
  
  
## Graph QL

Before understanding graph ql let's take a look at how clients used to communicate with the server earlier.
For a client to communicate with the server an API is required, the most popular is the rest api. Which is widely used in big projects. Before rest there was **COBRA** & **Soap**. 

We don't need to understand there working nor there history but if you wanted to have a read on this do check the Article - [History of rest api](https://blog.readme.com/the-history-of-rest-apis/)

**Graph QL** was publicly released in 2015. It is the newest API to work with. 

Do watch this Graph QL - [Documentary](https://www.youtube.com/watch?v=783ccP__No8)


That's it with history. Let's understand the technology.

#### Let's take an example :

There are a bunch of attributes and a structure that you wanted to call. Let's say user data, & You also wanted a list of hobbies of that user.

So if a rest api is implemented then you need to call two times one for getting the user and another for getting it's hobbies
(Depends upon how the api is implemented)

The rest api is simple. Performs just one task. Though this may come at an extra api call.

On the other hand in Graph QL you could do just one call to get both the data, that is the user & it's hobbies.
In Graph QL you get what you want there is no under-fetching or over-fetching

**under-fetching** - when you do an API call but the data you got is not enough for your need then it's under-fetching.\
**over-fetching** - when you do an API call and you get more data than is required then it's over fetching.


To get these advantages it all depends on how you make your Schema design.
Don't worry if you have not understood. It takes a good amount of time to learn to make a good schema.

**Well what is a schema?**

It's one of the most important parts of graph QL. It defines the data type of the object. The shape of the object that could be passed. Also it lets us define mutation query & subscriptions.

**Mutations** - It can be considered as an alternative to **put**, **post** & **delete** request of rest api. Basically this is used to Manipulate The  data on the server.\
**Query** - this is an alternative to **get** requests of rest api. If you want to get a particular data from a server use a Query request.\
**Subscriptions** - This is an awesome feature which lets you do some real time stuff. Example if you wanted to build a chat system with getting new messages in real time.

Let's understand the Syntax of the schema\
The schema is what decides the shape of the data which is to be passed between the client & the server.

If you know DTO's (Data transfer Object) then it is a bit easy to get started with schema design.

**Example** :
Creating a user data, and getting a user data
```graphql
type user {
id : String
name : String
age: Int
}
```

As we discussed the different operations available in Graph QL. Let's take a look at the Query first.
```graphql
type Query {
getUsers : [user]
}
```

So we are creating a query getUsers and defining the return type which is an array of already defined type user.
Simple right.
Now let's make a mutation for adding a user
```graphql
type Mutations {
addUser(id:String,name: String,age:Int): user
}
```

Here we are passing arguments to the mutation addUser. It's like calling a function with some parameters & getting a return value.
We will discuss Subscription later.

This was really a short description of Schema. Because this guide is not really for Graph QL but for AWS Appsync service. But don't worry as we will progress we will cover schema design again.\
So let's hold tight and move forward.

Guide on GraphQl -  [Graph QL Docs](https://graphql.org/learn/)

---

## Elements of Appsync

- ### Data Source
- ### Schema
- ### Resolver
- ### Authentication
- ### Functions (optional)


### Data Source

The data Source decides the data Source. The name itself is Descriptive.\
Say we want to store data in DynamoDB we can use DynamoDB as our data source and connect through a mutation in the schema.

### Schema

We create Schema to shape as well as define operations.\
An addUser mutation can be used to add a user or getUser to get a user. A **Type** User which defines that it contains attributes like Id, name, age with type String, String & Int respectively.

### Resolver

The Resolver is where all the Business logic is done.\
The appsync provides VTL ( Velocity template language ) as an option to write our custom logic.\
We need to attach a data Source to a Resolver.\
The data source can be DynamoDB or Lambda Function and many other options are available. We define addUser mutation then connect it with a Resolver which then could perform the required operations.

### Authentication

The Appsync provides many authentication types like api key, AWS cognito, IAM etc.\
We will take a look at authentication methods provided, understanding their pros & cons.

### Functions

Function in appsync is like a unit of computation which can be used multiple times. Like we use in any other programming language.

These are the main parts. Let's look at them one at a time and how they are connected to each other.


## What is a Resolver

Resolver is one of the most important part in Appsync.\
and even more important if you are thinking of adding most of the business logic in the resolver. Let's get Started.

First we need to create a data Source. We will create a DynamoDB data source. We need to use VTL ( Velocity template language ) in which we add our business logic.

**What is VTL ?**\
Apache Velocity Template Language (VTL) is a Java-based template engine.\
VTL is a logical template language that gives you the power to manipulate both the request and the response in the standard request/response flow of a web application.

It is very similar to any other language.
It has variables, conditional statements, loops and many data types supported.

I will guide with basic stuff only. But for getting a deeper understanding please refer to the AWS guide.


#### Variables 
```velocity
#set(variable_name = "Value")
```
#### Arrays
```velocity
#set(Arr = [])
```
To add element
```velocity
Arr.add("first element")
```
#### Conditional checks
```velocity
#if(condition)
##Logic to be performed
#else
##other logic to be performed
#end
```
***Notice the #end it's compulsory***

#### Loops
```velocity
#foreach(var in Arr)
##desired operations
#end
```
Don't worry if you haven't understood. Because it's not meant to be.\
This is to get an overview and nothing more than that.

VTL is good for achieving easy - medium business logic not at all recommended for hard & complex logic ( My personal experience)

### Concept

Here we go, A resolver connects the schema with the Back-end.
We can write CRUD operations in resolver.\
Suppose I want to write add user Operation in DynamoDB
The request mapping template is
```velocity
{
"version" : "2018-05-29",
"operation" : "PutItem",
"key": {
"name" : $util.dynamodb.toDynamoDBJson($ctx.args.email),
"age" : $util.dynamodb.toDynamoDBJson($ctx.args.age)
},
"attributeValues" : {
"address" : $util.dynamodb.toDynamoDBJson($ctx.args.address) }
}
```
If you don't know DynamoDB - It's a superfast Nosql database from AWS. Where you create a table and store objects in it.\
It has concept of primary key & sort key
Both these are attributes.\
Like to store user data the table would be like
- ***email - Primary key attribute***
- ***name - Sort key attribute***
- ***age - regular attribute***
- ***address - regular attribute***


The condition is that in every item it should have a unique combination of primary key & Sort key in a particular table.\
In this case no two items can have same email as well as name (This is just an example as
email will always be unique anyway)

An item should compulsorily have a primary key and can have any other attributes. (Upto 100 attributes)
Now if you check resolver above
It has some fields

**version** - The version for template definition\
**Operation** - the DynamoDB operations (In our case a PutItem to add new item) 
**Key** - which allow us to add the the primary & sort key value\
**attributeValues** : These are the extra attributes which could be added

Now the Response mapping template could be
```velocity
$utils.toJson($context.result)
```
Just passing the result back to the client as is.

If you didn't understand the technicality, no worry because it was not meant to be.\
All you should get by now is how Appsync works in basic terms. The Resolver is a very important & very big part and we will dive deeper in some other guides.

## Basic Authentication

The Appsync provides many authentication methods
Mainly IAM, AWS Cognito, API_KEY and others.

### API_KEY 

The aws console allows us to create a temp api key which can have a validity of upto a year.\
So this could be where you use your graph QL api as a third party api elsewhere. It's not recommended for regular projects though. Can be used for testing purpose

### IAM
If you know about AWS IAM and it's concepts like temporary roles, and you are good at it.\
Then you should consider this authentication type otherwise staying away at the beginning is good.

### AWS Cognito
This is the recommended authentication type to use in Appsync. You need to connect a user pool to appsync.\
The biggest benefit is that you get the identity object which have user attributes which
can be used for fine grained Authorization.


You can connect with me for any query or discussion on [LinkedIn](https://www.linkedin.com/in/mohd-anzar-a29b13184/) or [Twitter](https://twitter.com/RedThemer)