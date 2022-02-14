---
draft: false
title: "[beta publish] Hosting My First Hugo Blog with Cloudfront+S3"
date: 2022-02-08T23:17:36+08:00
description: "Hosting Hugo site with S3 and Cloudfront distribution. Using Terraform to manage infrastructure, and Github Actions for CI/CD pipeline."
tags: ["hugo", "aws", "tech", "terraform", "github-actions"]
---
## Architecture
First thing first, this is what it looks like from infra structure point of view.

![AWS Cloudfront and S3](./images/architecture-diagram-1-cloudfront-s3.png#center)

We will zoom in to each component when we work on it.

## Objective
The main objective of this post is a walk-through on "what" we need and "how" to host a Hugo website in AWS. There will be an illustration of an overview of the stacks with an architect diagram.

I want to make sure this post an easy to follow along, so you can simply just follow the steps to make your site.

## Getting Ready
### First things first
1. AWS account
2. Domain name

If you don't have one yet, **creating one with AWS** is my recommendation - mainly because it has the free SSL certificate and automatically create "Hosted Zone" for us. 

The use of **"SSL"** cert and **"Hosted Zone"** will be elaborated in "CloudFront" section. 

3. Github account

## Creating a simple Hugo site

Follow the official [Quick Start Guide](https://gohugo.io/getting-started/quick-start/).

Now local [webserver](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server) is required to run the site locally. Hugo provides `hugo server` command which is used to spin a webserver locally for development purposes. Read more about this command in Hugo [documentation page](https://gohugo.io/commands/hugo_server/).
```shell
# in ~/Desktop/blogs
hugo server -D
```
```
.....
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at //localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
Visit [http://localhost:1313/](http://localhost:1313/)

### Building
To deploy a "static" version of this site, we need to build the site.

Run `hugo` command to build the build into a `./public` directory
```shell
# in ~/Desktop/blogs
hugo
```

If interested, we can also validate the content of `./public` directory.
This is not mandatory, but it's kinda fun to do what we can to validate our understanding. We can run command `npx http-server`.
`npx` package comes with node, so make sure you have [nodeJs installed](https://nodejs.org/en/download/) first.
```shell
cd ~/Desktop/blogs/public/
npx http-server
```
```shell
Starting up http-server, serving ./
http-server version: 14.1.0
.....
Available on:
  http://127.0.0.1:8080
  .....
Hit CTRL-C to stop the server

```

Now that we have a functional Hugo blog ready for deployment. Let's work on building the infrastructure which we will deploy the site upon.

## S3

## Cloud Front with Lambda@Edge
### Let's take a look at Domain and SSL First

## Deployment Pipeline with Github Actions

## Terraform for Infrastructure as Code
I'm choosing Terraform to provision aws infrastructure, in fact it could have been any Infrastructure as Code, because it's easier to manage(create, update, destroy etc.) and maintain the engineering contexts.

## Why Hugo?
Hugo site is a static website, and it doesn't even need a database. Not only it's feature rich and easy to use, but the built site is also easy to be hosted. So why not!

