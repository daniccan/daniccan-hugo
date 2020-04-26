---
title: "Big Data Architectures: An Introduction to Big Data"
date: 2020-04-22T00:00:00+05:30
draft: false
author: "Daniccan"
categories:
  - Software Architectures
  - Big Data
tags:
  - Data
  - Architectures
  - Patterns
---

## Posts in the Big Data Architectures Series

* [An Introduction to Big Data](.)

## What is Big Data?

[Big Data](https://en.wikipedia.org/wiki/Big_data#Definition) is defined by Wikipedia as, "Data that usually includes data sets with sizes beyond the ability of commonly used software tools to capture, curate, manage, and process data within a tolerable elapsed time." In simpler terms, huge amounts of data that cannot be handled using a traditional database system is called as big data. Data from Social Media, Traffic Sensors, IoT (Internet of Things) and Web Servers are few examples.

## The 5Vs of Big Data

Big Data is characterized not only by its size. But also by different parameters. Initially, big data was defined using the 3Vs - `Volume`, `Velocity` and `Variety`. Over the period of time, an additional 2Vs - `Veracity` and `Value` were added to the list. 

### Volume

Volume refers to the amount of data that is generated and stored. In today's digital world, every interaction with a computer system is an event that is aggregated to data. Taking social media as an example, every like, comment, share, click and so on, are all events that are aggreated to form big data that can easily be in the number of billions per day. The volume of big data is usually measured in volumes of terabytes (TB), petabytes (PB) or higher.

### Velocity

Velocity refers to the speed or rate at which the data is generated. According to [Internet Live Stats](https://www.internetlivestats.com/one-second/), in 1 second, an average of 9,000 tweets are sent in Twitter, 1,000 photos are uploaded on Instagram and 80,000 google searches are done. The list goes on. That is how big data gets generated in the count of thousands per second. This can easily increase in the future.

### Variety

Variety refers to the different types of data. With a traditional database system, we could handle only textual data that can be organized as rows and columns. Big data breaks these limitations. There are 3 types of big data.

#### Structured Data

Structured Data is similar to the traditional data that can be stored in a [relational database system](https://en.wikipedia.org/wiki/Relational_database#RDBMS). It is commonly textual data that can be stored in rows and columns. Student or Employee details are examples of structured data.

#### Unstructured Data

Unstructured Data is not organized in a defined manner and does not have a particular format. This data cannot be stored in a traditional database due to its varying formats. PDFs, images and videos are examples of unstructured data.

#### Semi-structured Data

Semi-structured Data is the median between its counterparts. It cannot be stored in a relational database like structured data. But, it has some sort of definition or structure, which differentiates it from unstructured data. XML, JSON are examples of semi-structured data.

### Veracity

Veracity refers to the quality of data. Is it reliable? Can information be extracted from it? Or is it just noise? Big data should be reliable and meaningful either in its raw form or after it is processed.

### Value

Value refers to the information retrieved from the data. It is similar to Veracity with one difference. Veracity is about quality, whereas value is about the usability of the data.

## Applications of Big Data

Big Data is generated, collected, stored and processed across multiple industries and domains. Few of the common applications of big data are:

* Social Media - Suggestions and recommendations, personalized user experience, targeted advertising
* Healthcare - Research and studies, computer-aided diagnosis
* IT Infrastructure - Monitoring and management of servers and network infrastructure, security
* Internet of Things (IoT) - Monitoring and management of devices using internet
* Insurance - Forecasting and insights
* Business Intelligence - Insights on sales and marketing, informed decision making
* Machine Learning - Predictions, artificial intelligence
