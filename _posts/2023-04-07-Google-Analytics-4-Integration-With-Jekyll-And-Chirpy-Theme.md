---
layout: post
title:  "Jekyll Google Analytics 4 (GA 4) Integration With Chirpy Theme 📊"
date: 2023-04-07 22:33:48
categories: [Jekyll,setup]
tags: [jekyll,GA4,chirpy]
toc: true
---

![Retro cassette player in garage](/assets/analytics-on-screen.jpeg)
_Photo Credit [campaign creators](https://unsplash.com/@campaign_creators)_

Google Analytics (GA) 4 is the latest iteration of the popular web analytics platform, offering powerful insights into user behavior and site performance. If you're using Chirpy theme on a jekyll powered website you know that the instruction in the docs does not support GA4, hence you won't be able to track page views (PV) anymore. 

Since I have struggled with this issue during the creation of this very blog, I have finally found a working solution to integrate GA4 with Jekyll and Chripy theme, and it's rather very simple and straightforward 😁 

In this blog post, I will detail the process of integrating GA 4 with your Jekyll site, so by the end of this tutorial, you'll be able to track user engagement and gather vital data to optimize your site's performance.

## Prerequisites 📝 

Before we begin we will need 2 things:

1. A `Jekyll` site with the `Chirpy` theme installed.
2. A Google Analytics 4 (`GA 4`) account with a valid property ID.

## Setup Google Analytics ⚙️

### Create a Google Analytics 4 account

In case you happen to already have a GA 4 account then you can skip directly to this [section](#configure-gtag-script-globally)!

1. Visit [https://analytics.google.com/](https://analytics.google.com/) and click "Start Measuring."
2. Input your preferred Account Name and select the appropriate checkboxes.
3. Provide a suitable Property Name, which will represent the tracker project displayed on your Google Analytics dashboard.
4. Fill in the necessary details about your business.
5. Click "Create" and accept any licensing pop-ups to establish your Google Analytics account and generate your property.

### Create a data stream for your blog

After creating your property, the next step is to set up a Data Stream to monitor your blog's traffic. If you're not automatically prompted to create your first Data Stream upon signing up, follow these steps:

1. Click `Admin` in the left-hand column.
2. Choose the appropriate property from the second column's drop-down menu.
3. Select `Data Streams`.
4. Click __Add a stream__ and choose `Web`.
5. Input your blog's URL.

> If you have already setup the `google_analytics` config tag in the `_config.yml` file then you should revert it. Otherwise the integration will not be successful!
{: .prompt-danger }

Your `_config.yml` should look something like this:

```yml
..
google_analytics:
  id: # fill in your Google Analytics ID
  # Google Analytics pageviews report settings
  pv:
    proxy_endpoint: # fill in the Google Analytics superProxy endpoint of Google App Engine
    cache_path: # the local PV cache data, friendly to visitors from GFW region
..
```
{: file="_config.yml" }

## Configure gtag script globally 🌐

> `gtag` is a JavaScript library used for tracking and measuring website traffic and user behavior. It simplifies the process of implementing Google's tracking and analytics tools, such as Google Analytics, Google Ads, and Google Conversion Tracking, by providing a unified tagging system.
{: .prompt-info }

If you created your Jekyll website using this [chirpy starter](https://github.com/cotes2020/chirpy-starter) project then there are couple file that we need to override from the main source chirpy project which can be found [here](https://github.com/cotes2020/jekyll-theme-chirpy)

### Case 1: Chripy starter project

1. Let's start by creating a new directory 📁 on the root level of your jekyll blog source and let's name it `_includes`.
2. inside `_includes` directory __create__ a new file `google_analytics.html` and past the following code:

    > Do not forget to swap the `G-XXXXXXXXXX` value with your data stream `Measurement ID`!
    {: .prompt-danger }

    ```html
    <!-- Google tag (gtag.js) -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
    <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());

    gtag('config', 'G-XXXXXXXXXX');
    </script>
    ```
    {: file="google_analytics.html" }

3. Then from the Chripy main source project [found here](https://github.com/cotes2020/jekyll-theme-chirpy) we will __copy__ the [head.htm](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/_includes/head.html) file which we will place inside the `_includes` directory we have created in step-1.

4. Using your code editor __open__ the `head.html` file and __remove__ this code

    ```liquid
    {% raw %}{% if jekyll.environment == 'production' and site.google_analytics.id != empty and site.google_analytics.id %}
        <link rel="preconnect" href="https://www.google-analytics.com" crossorigin="use-credentials">
        <link rel="dns-prefetch" href="https://www.google-analytics.com">
    
        <link rel="preconnect" href="https://www.googletagmanager.com" crossorigin="anonymous">
        <link rel="dns-prefetch" href="https://www.googletagmanager.com">
    
        {% if site.google_analytics.pv.proxy_endpoint %}
            {% assign proxy_url = site.google_analytics.pv.proxy_endpoint
            | replace: 'https://', ''
            | split: '/'
            | first
            | prepend: 'https://'
            %}
            <link rel="preconnect" href="{{ proxy_url }}" crossorigin="use-credentials">
            <link rel="dns-prefetch" href="{{ proxy_url }}">
        {% endif %}
    {% endif %}{% endraw %}
    ```
    {: file="head.html" }

5. inside the same file, insert the `google_analytics.html` file content using `liquid` syntax.

    ```liquid
    {% raw %}{% include google_analytics.html %}{% endraw %}
    ```
    {: file="head.html" }

6. Save, build and deploy 🚀

### Case 2: Jekyll template with chirpy theme applied

> In this case your blog should've been manually created by installing `Jekyll` and then applying `Chripy` theme.
{: .prompt-info }

1. locate the directory `_layouts`
2. edit the file `default.htm` by __adding__ this `gtag` script at the end of the `<head>` html tag.

    ```html
    <!-- Google tag (gtag.js) -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
    <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());

    gtag('config', 'G-XXXXXXXXXX');
    </script>
    ```

3. save, build and deploy 🚀

## Verify the traffic ✅

To verify whether or not the `GA 4` integration is successful:

1. Head back to Google [analytics](https://analytics.google.com/)
2. On the left pane, click on __Reports__
3. Click on __Realtime__ to view your traffic in realtime. At this stage I would advise using a different browser or invalidate the browser's cache.

You should see traffic comming in in the dashboard as you visit your site. Since we have configured the `gtag` script into the `<head>` section, it should now be a part of each an every page of your jekyll blog. This way Google analytics can track all the page views (pv).

😌 And as always happy coding and may the **Source** be with you! 