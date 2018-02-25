---
layout: post
title: "How to fix datetime-local input in Chrome"
date: 2018-02-25 19:38:01 +0200
comments: true
categories: java html5 english datetime-local input issue Chrome
---

 
## Problem description

You are facing the problem displaying the date in HTML5  input __datetime-local__ in Chrome. In FireFox the same web page, with the same HTML code, looks pretty good

{% img flotte /images/Html5_DateTimeLocal_Issue_Chrome.jpg datetime-local issue in chrome %}
 
<!-- more -->


## Problem analysis

In fact, the __datetime-local__ attribute is rather new in HTML5 specification and the implementation differs from one browser to another.

By reading the [HTML5 specs](https://www.w3.org/TR/2012/WD-html-markup-20120315/input.datetime-local.html) about this attribute, 
I noticed that __date and time part__
should be __separated__ by literal string __"T"__

FireFox is more permissive for this particular scenario and displays the date correctly. 
Chrome respects the specification without applying any magic and does not recognize the *"wrong"* date format.  

## Proposed Solution

The solution is to respect the w3c specification and to use the literal string __"T"__ between date and time parts.
Like this

{% codeblock lang:html%}
        <input type="datetime-local" value="2018-02-25T19:24:23"/>
{% endcodeblock %}

If your HTML code is generated programmatically (PHP, Java, etc), you should apply the special formatting for your objects containing dates.

For my Java project with [Spring](https://projects.spring.io/spring-framework/) and [Thymeleaf](http://www.thymeleaf.org), 
I applied the following annotation for my fields typed as __Date__. 

{% codeblock lang:java %}
        @DateTimeFormat(pattern = "yyyy-MM-dd'T'HH:mm:ss")
        private Date createdAt;
        
        @DateTimeFormat(pattern = "yyyy-MM-dd'T'HH:mm:ss")
        private Date updatedAt; 
{% endcodeblock %}

Please, note, that accordingly to [SimpleDateFormat pattern doc](https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html), literal __T__ must be enclosed between __single quotes__. 
 
  


