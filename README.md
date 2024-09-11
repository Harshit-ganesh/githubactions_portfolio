# GitHub Actions Portfolio

This repository showcases various GitHub Actions workflows and configurations. It serves as a portfolio to demonstrate the capabilities and use cases of GitHub Actions in automating tasks, CI/CD pipelines, and more.

## Table of Contents

- Introduction
- Features
- Usage
- Workflows


## Introduction

In this project, we simply created CI/CD pipeline with help github actions that helps us to host a static portfolio website in s3 of AWS and automates the whole process, making it highly manageable.

## Features

- **Automated CI/CD Pipelines**: Streamline your development process with continuous integration and deployment.
- **Custom Workflows**: Create and manage custom workflows for different use cases.
- **Integration with Third-Party Services**: Connect with various third-party services such as s3 of AWS.

## Apply

In order to get this in your device, there are few steps we need to follow:

1. **Push your static website code in your repository**:
    ```bash
    git push -u orgin
    ```


2. **Create a yml file**: Navigate to the `.github/workflows` directory and create main.yml and add the below code.

## Workflows

   ```yaml
   name: Portfolio Deployment

    on:
      push:
        branches:
        - main
    
    jobs:
      build-and-deploy:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout
          uses: actions/checkout@v1

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1

    - name: Deploy static site to S3 bucket
      run: aws s3 sync . s3://<bucket-name> --delete
   ```

4. **Customize the workflows**: Then go to settings of your repository then Security/Secrets and variables/Actions. Now create two New repository secret with name and secret as

| Name              | Secret  |
| ----------------- | --------|
| AWS_ACCESS_KEY_ID | ******* |
| AWS_SECRET_KEY_ID | ******* |

The secret are the credentials of the IAM user with permissions to create bucket in s3.

## AWS Part 

1. **Login into the IAM user and create a bucket and provide it with public acess**
  
2. **Now go to its properties and enable static website hosting and save it. It creates a link that provide us with an acess to our website.**

3. **Also go to Permisssions and add this policy**

   ```bash
    {     
    "Version": "2012-10-17",
    "Statement": [         
       {             
      	"Sid": "PublicReadGetObject",             
      	"Effect": "Allow",             
      	"Principal": "*",             
      	"Action": "s3:GetObject",            
      	"Resource": "arn:aws:s3:::<bucket-name>/*"         
    	  }    
      ] 
    }

Replace <bucket-name> in the above policy and save it. Then replace the same in yml file and commit, the website would be hosted in the link.
