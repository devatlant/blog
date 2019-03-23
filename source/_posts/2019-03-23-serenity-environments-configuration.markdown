---
layout: post
title: "Serenity environments configuration"
date: 2019-03-23 12:32:08 +0200
comments: true
categories: serenity test configuration environments maven java english
---


{% img flotte /images/SerenityMultiEnvironments.jpg 100 100 %}

Real, production-grade and automated **acceptance tests** on top of **Selenium** are running on different environments. 


If you are using **Serenity** you will find here a useful tip - how to set up a **multi environment** configuration.

<!-- more -->

{% img /images/Serenity_multi_env_config_example.png Serenity environments configuration %}

## Context
Recently I discovered the powerful **Java** framework for writing acceptance tests on top of **Selenium**, which is called [Serenity](https://www.thucydides.info).

One of the basic requirements we have is  to run your tests on **different environments**: 

* Developer will run test on the local machine
* Jenkins will run your acceptance tests on a remote machine
* Sometimes you will need to run your tests on production (as a fastest way to guarantee the non-regression after release)


# Multi environment configuration
So you need to tell the serenity framework where to find those config.
We have started to using [Spring profiles](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html) 
to manage this configuration. Hopefully Serenity supports Spring. But we faced the problem of handling 2 different configs:
1. Serenity.properties 
2. Spring-based config with **localhost.properties, staging.properties, production.properties**

##### Config with Spring was hard, non intuitive and error-prone.

## Solution - serenity.conf file

The solution is to use the special config file natively understandable by Serenity framework - **serenity.conf**

```yaml
environments {
    localhost {
        webdriver.base.url = "http://localhost:8080/"
    }
    staging {
        webdriver.base.url = "http://staging.devatlant.com"
    }
    prod {
        webdriver.base.url = "http://my.super.production.com"
    }
}
```

Unfortunately this very useful feature is not yet documented on the official project page, I have found it [here](https://johnfergusonsmart.com/environment-specific-configuration-in-serenity-bdd)



### Serenity version is 2.0.30 or higher
This feature is available for you from **2.0.30**. Guys from Serenity project releases often (_369 releases! at date of the 22 March 2019_ ), so keep in mind to regularly 
updating your maven dependency from [official GitHub](https://github.com/serenity-bdd/serenity-maven-plugin/releases) release page

### Run maven with environment name

```bash
mvn run -Denvironment=staging
```


