---
draft: true
title: "My First Blog with Hugo hosting in Cloudfront+S3"
date: 2022-02-08T23:17:36+08:00
description: "Hosting Hugo site with S3 and Cloudfront distribution. Using Terraform to manage infrastructure, and Github Actions for CI/CD pipeline."
tags: ["hugo", "aws", "tech", "terraform", "github-actions"]
---
## Architecture
First thing first, this is what it looks like from infra structure point of view.

![AWS Cloudfront and S3](./images/architecture-diagram-1-cloudfront-s3.png#center)

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
#### Install Hugo
```shell
brew install hugo # for osx
hugo version      # verify installation
```

#### create a new site
```shell
mkdir ~/Desktop/blogs
cd ~/Desktop/blogs

# in ~/Desktop/blogs
hugo new site .
```

#### Initialize git
```shell
# in ~/Desktop/blogs
git init
```

#### Add a theme
Using PaperMod as an example
```shell
# in ~/Desktop/blogs
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod

# make sure themes/PaperMod being created by the above command
```


## S3
## Cloud Front with Lambda@Edge
#### Let's take a look at Domain and SSL First

## Deployment Pipeline with Github Actions

## Terraform for Infrastructure as Code
I'm choosing Terraform to provision aws infrastructure, in fact it could have been any Infrastructure as Code, because it's easier to manage(create, update, destroy etc.) and maintain the engineering contexts.

## Why Hugo?
Hugo site is a static website, and it doesn't even need a database. Not only it's feature rich and easy to use, but the built site is also easy to be hosted. So why not!

