# S3 Static Website Automated Deployments

Automate deployments to an S3 Static Website whenever code is committed to a particular branch in your CodeCommit repo. 

There are simpler ways in which this could be achieved, e.g. http://stackoverflow.com/questions/32530352/best-strategy-to-deploy-static-site-to-s3-on-github-push. However this approach gives you a central deployment pipeline which could easily be extended to include code reviews, builds or tests.

## Info

index.js is a Lambda function to be used as a custom action in CodePipeline for deploying from CodeCommit to an S3 Bucket:

 CodePipeline -> Lambda -> S3 Static Website

## Instructions

1. Run npm install to install the dependencies ('mkdirp' and 'yaunzl') for the Lambda function. 

  `npm install`

2. zip up the required files for the Lambda function:

  `zip -r StaticS3SiteDeploy.zip index.js node_modules/`

3. Upload the Lambda function zip archive to AWS Lambda Service using AWS Console.

