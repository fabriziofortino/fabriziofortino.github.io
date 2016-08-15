---
layout: post
title: "Multi-language Fixer.io clients with Swagger"
description: "Generate Fixer.io API clients for multiple languages with Swagger"
category: articles
tags: [api, rest, swagger, fixer, client]
comments: true
share: true
ads: true
image:
  feature: currencies.jpg
---

<a href="http://fixer.io/" target="_blank">Fixer.io</a> is a free API that provides current
and historical foreign exchange rates published by the <a href="https://www.ecb.europa.eu/home/html/index.en.html"
 target="_blank">European Central Bank</a> (rates are updated every day around 4PM CET).

Although the API is very easy to use, if you want to integrate it in your application you need to
write a bit of code. Googling around I have found clients for <a href="https://pypi.python.org/pypi/pixer/0.0.2"
target="_blank">Python</a> and <a href="http://www.phpclasses.org/package/9799-PHP-Fetch-currency-exchange-rates-from-fixer-io.html"
target="_blank">PHP</a>.

### Schema Definition
Since I need to use it in different applications (mainly Java and Go) I have decided to map the API calls
and response models in a <a href="http://swagger.io/specification/" target="_blank">Swagger Schema</a>
using the YAML format.

{% gist fabriziofortino/5d0482c72a7b6b5bbe2c65d978f9f2f7 fixer.yml %}

The **definitions** section contains the ```Rates``` model: an object with 2 properties (base & date)
and a dictionary of exchanges where the key (string) is the currency and the value (double) the rate.

**paths** is where the API endpoints are mapped: the former gets the latest rates, the latter the historical ones.

I have published the schema as a public API on Swagger hub <a href="https://swaggerhub.com/api/fabriziofortino/fixer-io/1.0" target="_blank">here</a>. The right hand side
of the page shows the API in a nicely formatted style. Unfortunately the **Try this operation** button does not
to work due to a javascript bug in the website. As a workaround copy-paste the schema definition in
<a href="http://editor.swagger.io/" target="_blank">http://editor.swagger.io/</a> where everything seems to work.

### Code Generation
One of the most interesting feature of Swagger is the ability to generate code for the most common
languages through the <a href="https://github.com/swagger-api/swagger-codegen" target="_blank">Swagger Codegen project</a>.

The code generation feature is also integrated in Swagger hub. To create a client: click on the
download icon on the top-right menu, then Client and finally select a language from the list. In a matter of
seconds a zip file with the generated client will be downloaded. It is possible to customize the generated code
from the menu **Edit Codegen Options**.

### How to use the clients
Every client implementation is language dependent though they present similarities in term of naming and structure.
The following code snippet uses the Java Feign client to access the latest AUD exhange rate for USD.

{% gist fabriziofortino/5d0482c72a7b6b5bbe2c65d978f9f2f7 Rates.java %}

The Go client provides the functions ```NewRatesApi``` and ```GetLatest``` to respectively get an handler to the API
and invoke the GET request to retrieve the latest rates.

{% gist fabriziofortino/5d0482c72a7b6b5bbe2c65d978f9f2f7 main.go %}
