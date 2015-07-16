---
layout: post
title: "Hibernate JSON UserType: simple handling of JSON objects with Jackson"
description: "Persist and read JSON objects with Hibernate"
category: articles
tags: [java, json, orm, hibernate, jackson]
comments: true
share: true
ads: true
---

<a href="http://hibernate.org" target="_blank">Hibernate</a> provides a wide range of mappings to convert SQL data to Java objects and the other way around. In some circumstances, developers may need to define custom types to handle the mapping of a column to a specific type.
In the last years, along with the exploding growth of JavaScript, JSON has become the most popular data­-interchange format with lots of supporting libraries that make it easy to read and write models in JSON.
<a href="http://jackson.codehaus.org/" target="_blank">Jackson</a> is a widely used high performance Java JSON data processing library.
Here I will show you how to create a custom Hibernate UserType that uses Jackson to map generic POJOs to a database column in a transparent fashion.

### Add Jackson as dependency to your project
{% gist fabriziofortino/d33e6cf0e06545fcba18 pom.xml %}

### Include the Jackson UserType implementation
Here is the code in charge of handling the marshall/unmarshall of JSON to Java and vice versa. It implements *ParameterizedType* to allow parameterization of the mapping file. This implementation supports LONGVARCHAR, CLOB and BLOB.

{% gist fabriziofortino/d33e6cf0e06545fcba18 JSONUserType.java %}

### Define the hbm file to instruct Hibernate how to map the defined class to the database table
Below is an example of mapping file for the Person java class. As you can see it contains the *status* property of type EnumType (provided by Hibernate) and our JSONUserType *config* property. Parameter *classType* is the mapped POJO and *type* is the CLOB column type constant (see class <a href="http://docs.oracle.com/javase/6/docs/api/java/sql/Types.html" target="_blank">java.sql.Types</a> for more details on constant field values).
{% gist fabriziofortino/d33e6cf0e06545fcba18 mapping.xml %}
