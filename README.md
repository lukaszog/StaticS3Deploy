# S3 Static Website Automated Deployments

Automate deployments to an S3 Static Website whenever code is committed to a particular branch in your CodeCommit repo. 

There are simpler ways in which this could be achieved, e.g. http://stackoverflow.com/questions/32530352/best-strategy-to-deploy-static-site-to-s3-on-github-push. However this approach gives you a central deployment pipeline which could easily be extended to include code reviews, builds or tests.

## Info

index.js is a Lambda function to be used as a custom action in CodePipeline for deploying from CodeCommit to an S3 Bucket:

 CodeCommit -> CodePipeline -> Lambda -> S3 Static Website

The CloudFormation template 'codepipeline.json' can be used to provision a pipeline to automate deployments from CodeCommit to an S3 static website.

## Instructions

1. Clone this repo

  `git clone https://github.com/alexgibs/StaticS3Deploy.git`

2. cd to the cloned repo:

  `cd StaticS3Deploy.git`

2. Run npm install to install the dependencies ('mkdirp' and 'yaunzl') for the Lambda function. 

  `npm install`

3. zip up the required files for the Lambda function:

  `zip -r StaticS3SiteDeploy.zip index.js node_modules/`

4. Upload the Lambda function zip archive to an S3 bucket in the 'us-east-1' region (CodeCommit is only available here at this stage).

  `aws s3 cp StaticS3SiteDeploy.zip s3://<your-bucket> --region us-east-1`

5. Deploy the CloudFormation stack in 'us-east-1', passing in the required parameters.

The CloudFormation stack will create a pipeline (CodePipeline) and Lambda function as a custom action.


## Caveats

The output artifact from CodeCommit is a zip archive which is extracted to /tmp in the Lambda function. Lambda limits us to 512 MB in /tmp, so this function will not work if your codebase is larger than this. The function could easily be modified to extract individual files, upload, then delete, rather than extracting all in one go.
It would be ideal if CodePipeline gave us the output artifact as a tarball, so that it could all be handled via a stream in memory.
