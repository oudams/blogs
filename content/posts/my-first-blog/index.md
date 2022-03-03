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
1. AWS account and CLI

If you haven't already, create an AWS account then [Create a CLI user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_cliwpsapi) and place the credenial in

```shell
# ~/.aws/credentials
[your_username]
aws_access_key_id = xxxxxx
aws_secret_access_key = xxxxxx
```

Next, make sure your machine has [aws cli installed.](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Finally, run below command to allow AWS CLI to use `your_username` profile

```shell
export AWS_PROFILE=your_username
```

2. Domain name

If you don't have one yet, **creating one with AWS** is my recommendation - mainly because it has the free SSL certificate and automatically create "Hosted Zone" for us. 

The use of **"SSL"** cert and **"Hosted Zone"** will be elaborated in "CloudFront" section. 

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
```shell||
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

## Prepare Development Environment

#### Install Terraform

Follow HashiCorp [Guide for Terraform installation](https://learn.hashicorp.com/tutorials/terraform/install-cli)

#### Working Directory

I Choose to create a separated director from the `blogs` directory.

So, in the terminal
```shell
mkdir ~/Desktop/blogs-infra
cd ~/Desktop/blogs-infra
git init

touch main.tf
```

Define aws as a provider in main.tf

```hcl
# main.tf
provider "aws" {
  region = "us-east-1"
}
```

## Creating Infrastructure

![AWS Cloudfront and S3](./images/architecture-diagram-1-cloudfront-s3.png#center)

### S3
S3 has 2 responsibilities when hosting a website. First, it stores all the content of the website. Second, it operates as a web server.

From security point of view, S3 is placed privately behind a cloudfront, meaning it cannot be accessed directly from public.

What we need for terraform? 
- (A)Unique Name for s3 bucket. It could simply be the name of the website.

### About CloudFront
Cloudfront will be a public layer, it can be accessed via a domain name, and then talk to S3 to query the website contents.

What do we need for terraform?
- (Not Required)

### Domain Name on Route53
After register a domain in AWS, a Hosted Zone is being created for this domain.

What do we need for terraform?
- (B)Domain Name, for example `oudam.es`

### Let's get started
To minimize code boilerplate, it's common to use a public terraform module.
In this case, we are going to use [oudams/cloudfront-s3-website/aws](https://registry.terraform.io/modules/oudams/cloudfront-s3-website/aws/latest)

Open your `~/Desktop/blogs-infra/main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

# Place your (B)Domain Name value in here 
locals { 
  domain = "oudam.es"
}

# Using  "oudams/cloudfront-s3-website/aws" to provision common infrastructure components

module "cloudfront_s3_website_oudam_es" {
  source                      = "oudams/cloudfront-s3-website/aws"
  version                     = "1.2.4"
  hosted_zone                 = local.domain
  domain_name                 = local.domain
  acm_certificate_domain      = local.domain
}
```
Let's 'make' apply these component on to AWS

```shell
# cd ~/Desktop/blogs-infra/
make init

make plan

make apply
```

We expect, that the content of the S3 bucket is empty.
The configuration of Cloudfront, and Route53 should be done correctly.

To test this out, let's upload our website content to S3 bucket. 
```shell
cd ~/Desktop/blogs/
hugo
# now the website artifacts should be generated in `blogs/public/`
```

So we just need to upload the content inside the `blogs/public/` directory.
Please follow this [guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/upload-objects.html)

Finally, in your browser, visit https://<Domain Name>/index.html
You should see your website home page.!

Nice Job! But there's one Gotcha when deploying Hugo behind a cloud front....

## Deployment Pipeline with Github Actions

## Terraform for Infrastructure as Code
I'm choosing Terraform to provision aws infrastructure, in fact it could have been any Infrastructure as Code, because it's easier to manage(create, update, destroy etc.) and maintain the engineering contexts.

## Why Hugo?
Hugo site is a static website, and it doesn't even need a database. Not only it's feature rich and easy to use, but the built site is also easy to be hosted. So why not!

