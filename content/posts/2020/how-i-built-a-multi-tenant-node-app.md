---
title: "How I Built a Multi-Tenant Node App"
date: 2020-04-08T00:00:00+05:30
draft: false
author: "Daniccan"
categories:
  - My Experiences
tags:
  - Multitenant
  - NodeJS
  - MySQL
  - Sequelize
  - JWT
---

Due to the [COVID-19](https://en.wikipedia.org/wiki/Coronavirus_disease_2019) pandemic, countries across the globe started enforcing lockdowns, where people stayed at home, to stay safe and [flatten the curve](https://en.wikipedia.org/wiki/Flatten_the_curve). People were using this time to laze around, binge watch tv and play games, spend time with their families, learn and try new things and so on. I did most of that too. As a software developer, I also tried out a [Proof of Concept](https://en.wikipedia.org/wiki/Proof_of_concept) by building a [multi-tenant NodeJS app](https://blog.lftechnology.com/designing-a-secure-and-scalable-multi-tenant-application-on-node-js-15ae13dda778).

This is not a step-by-step technical post on how to build the app, but more about my experience in building the app.

## What did I want to build?

The one line requirement was to build a [multi-tenant](https://en.wikipedia.org/wiki/Multitenancy) [RESTful web service](https://en.wikipedia.org/wiki/Representational_state_transfer) app on top of [NodeJS](https://nodejs.org/en/). In detail, the app had to saftisfy the following additional criterias.

### Basics
* A RESTful web service built using NodeJS
* Data to be persisted in a RDBMS database ([MySQL](https://www.mysql.com/) / [PostgreSQL](https://www.postgresql.org/) / [SQLite](https://sqlite.org/index.html))
* Basic CRUD (Create-Read-Update-Delete) operations on resources such as `Users`, `Organizations`

### Multi-tenancy
* A `Signup API` used to create a new `Account`
* Every new `Account` creation in turn creates a new Tenant DB / Schema
* All API requests to perform operations only in its respective Tenant DB

### Security
* A `Login API` to authenticate a `User`
* A token-based authentication layer on top of all CRUD API requests

## How did I build it?

This was a new experience for me. Before this, I haven't worked on multi-tenancy or multiple databases support in NodeJS. So, the concept of the whole app was initally overwhelming for me. But, the first step to completing any big task is to break it up into smaller manageable tasks and that is exactly what I did. 

### Setting up the Development Environment

I setup [NodeJS using NVM](https://nodejs.org/en/download/package-manager/#nvm) and [MySQL](https://dev.mysql.com/doc/refman/8.0/en/installing.html) in my laptop for development. To code, I used the [Visual Studio Code](https://code.visualstudio.com/) editor, as it supports multiple themes and has a lot of plugins for different programming and scripting languages.

### Starting with the Boilerplate

Setting up the boilerplate code was the easiest, as I just had to copy-paste the base code of a RESTful web service app. It included, 

* A HTTP server using [ExpressJS](https://www.npmjs.com/package/express) 
* A [Body Parser](https://www.npmjs.com/package/body-parser) to parse the incoming HTTP API requests
* A Logger to write application logs to file using [Winston](https://www.npmjs.com/package/winston) and [Morgan](https://www.npmjs.com/package/morgan)
* A monitoring tool to automatically restart the app when new changes were detected using [Nodemon](https://www.npmjs.com/package/nodemon)
* Routes and empty methods for the basic CRUD operations to be implemented

### Defining Schema via Models & Migrations

The next step was to define and implement the data model in such a way that it supports any RDBMS mentioned in the requirements. As this was just a POC, I went with a set of basic fields for the tables and created a [one-to-many](https://sequelize.org/master/manual/assocs.html#one-to-many-relationships) relationship between `Organizations` and `Users`.

However, I needed a way to implement this data model in the database. Also, I needed this to be automated, as these tables will be re-created for every new tenant. So, instead of creating the tables manually, I went with [Sequelize](https://www.npmjs.com/package/sequelize) and [Sequelize CLI](https://www.npmjs.com/package/sequelize-cli), an ORM framework. This helped me in three ways.

* To create and update databases, tables, and relationships using [Migrations](https://sequelize.org/master/manual/migrations.html). Execution of the `Migrations` can be automated programmatically too.
* To map the DB Tables with Objects in the app using [Models](https://sequelize.org/master/manual/model-basics.html). This way, I did not have to write any SQL queries to perform operations on the DB. I could just call the methods such as `create`, `findAll`, `update`, `destroy` etc., on the `Models` and its corresponding SQL queries would be executed on the DB.
* To support any of the RDBMS databases easily by changing the [dialect](https://sequelize.org/master/manual/dialect-specific-things.html) configuration.

### CRUD with REST APIs

This was my comfortable part of the app where I had to build APIs using `GET`, `POST`, `PUT` and `DELETE` HTTP methods to perform the CRUD operations on `Users` and `Organizations`.

All APIs were configured using [routing](https://expressjs.com/en/guide/routing.html) in the app and the interactions with the database were peformed using [Sequelize](https://sequelize.org/v5/manual/querying.html).

### Require Login for Authentication

Now, all of the APIs were open. Anyone could access the APIs directly. This was not secure. All API requests needed to be authenticated to ensure that the `User` is valid to perform any operations. The `Users` table had an `email` and `password` field which could be used for validation. I used [Passport](https://www.npmjs.com/package/passport) to validate the `email` and `password` sent in the `Login API` request.

However, if I am going to add an authentication layer on top of every CRUD API request, I cannot validate the `User` by checking the database everytime. This would be inefficient. This is where [JWT](https://jwt.io/), a token-based authentication helps. A encoded `JSON token` is returned on successful login using the `Login API`. This `JSON token` will then be sent in the `Authorization` header of all other API requests, where it is decoded and validated. A [Passport JWT](https://www.npmjs.com/package/passport-jwt) strategy is available for this integration.

### Signup to create a new Tenant

It was easy to create a `Signup API` which would accept basic information about the `Account` and the `User` who signs up. The `Account` information would be persisted in the database in the `Accounts` table. 

The next part was tricky. The `User` information should not be persisted in the main database. It had to be written to the tenant database. To do that, I would need to create a tenant database along with all the tables that it needs. This was done using [Sequelize CLI and Migrations](https://sequelize.org/master/manual/migrations.html#running-migrations). Programmatically, I created a tenant database based on the `ID` of the signup entry in the `Accounts` table and then ran the migrations on top of the tenant database. The `User` information can now be written to the tenant database. But, I still had to get the DB connection for the tenant database to do that.

### Pool of Tenant Connections

I created a simple key-value pair object to create, store and fetch DB connections. The tenant name would be the key and the value would be the DB connection. This would act as a [connection pool](https://en.wikipedia.org/wiki/Connection_pool). It is similar to how [caching](https://en.wikipedia.org/wiki/Cache_(computing)) works. When a new tenant database is created, a connection for that database is created and stored in the connection pool. The connection can then be fetched from the pool by providing the tenant name.

### Multi-tenancy in the API Requests

What about the connections for the tenant databases that are already present? Every time a new API request comes in, a check for the connection of the requested tenant is made in the connection pool. If it is available, the connection is fetched and the operation is performed on the tenant database. If it is not available, a new connection for the tenant is created and stored in the connection pool and is then used for the database operation. Since, the connection would now be present in the connection pool, all subsequent requests of the tenant can fetch the connection directly from the pool.

But, wait. How does the app know which tenant is requested by the API? The most common method is to use [subdomains](https://en.wikipedia.org/wiki/Subdomain), like `tenant1.example.com`, `tenant2.example.com`, etc. However, this is just a POC and also I am running the application on `localhost`. So, I went with the easier approach of sending a custom header called `X-TENANT-ID` in the API request, which will be used to identify the tenant.

## What else could I have done?

Some of the additional requirements that could have been there are:

* Implement `Roles` and `Policies` to setup [RBAC](https://en.wikipedia.org/wiki/Role-based_access_control)
* Use sub-domains instead of the custom header to identify the tenant
* Create a multi-tenant front-end application to interact with this REST app
* Containerize the app using `Docker`
* and so on...

Like the above, there is a lot more that I could have added to this POC. Maybe, someday I will.

## What did I learn from this?

[Learning by doing](https://en.wikipedia.org/wiki/Learning-by-doing) is the best approach to learn anything. By building this app, I learnt a lot of things conceptually and technically. I learnt about different approaches to the multi-tenancy architecture (I might even write a blog post on that one day). I learnt to use some common NPM libraries like `Sequelize`, `Passport`, etc. Most of all, I learnt that if you know how to break a huge task into smaller pieces, you will surely be able to complete it. 

You can find the code for the app in the [daniccan/multi-tenant-node-app](https://github.com/daniccan/multi-tenant-node-app) GitHub repository.
