# AWS CodePipleline 


## Basic Setup
- Need to create a repository in GitHub and push your code inside for testing
- Create a file ‘buildspec.yml’ for your build configurations and pre/post build commands.
- Create an EC2 Instance for your project per your need.
- Setup PHP and Apache into your EC2 instance by accessing via ssh manually.
- Install CodeDeploy agent via https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install in your EC2 instance.
- Go to AWS CodePipeline and create your new Project Pipeline.
- In order to create your new pipeline, you need to follow some steps of create pipeline.
- Inside Source, select GitHub or you may select any other available repository source where you have committed your code. After selection you need to give authorization to AWS for that repository.
- After syncing your repository you need to select your desired repository and select your desired branch.
- Inside Build, select AWS CodeBuild, or you may select any other available build provider per your need and availability.
- After selecting Build provider you may configure per your selected option, in our example we are going to setup for AWS CodeBuild, so you may enter a desired Project Name inside configuration section.
- Also you may select your build environment by selecting OS and Runtime environment. Similarly, you may set your environment variables here (if needed).
- This step also create a new role for build services, which you may see inside `AWS CodeBuild Service Role` section.
- Inside Deploy, select AWS CodeDeploy for EC2 Instances and create your desired application by clicking `Create a new one in AWS CodeDeploy` along with deployment group.
- You also require to add your EC2 instance which you have already created in above step for your deployments by adding a name tag and value.
- Inside Service Role, create a service role in IAM to give AWS CodePipeline permission to use resources in your account. If you already have a service role. configured for this purpose, you can choose it from the list instead of creating a role.
- Inside Review, you may review all your configurations and create your pipeline.
- After creating your pipeline successfully, your build will be deployed automatically or you may manually deploy your builds.
- By following all above steps, your builds will be deployed automatically on every commit (after specific time frame).
- After getting success on setting up AWS CodePipeline you need to create a file with your desired application configurations in your repository with a name of ‘appspec.yml’, otherwise your build deployment will be failed.

## Challenges
- Configuring S3 Bucket for Builds and Revisions
- Extracting Application Build and Setting up Configuration
- Permissions Issues

## Areas Covered
- AWS CodePipeline - This service will help you to model and automate your software release process.
- AWS CodeBuild – A build generator, which is a fully managed build service that compiles source code, runs tests, and produces software packages that are ready to deploy
- AWS CodeDeploy - This service efficiently deploys your released code to a “fleet” of EC2 instances while taking care to leave as much of the fleet online as possible. 
- GitHub instead of AWS CodeCommit -  A source control service that makes it easy for companies to host secure and highly scalable private Git repositories.
- Other Required Services:
	EC2 Instances
	S3 Bucket
	IAM
	AWS Service Role

## Recommendations
- We can utilize AWS CodePipeline for fully automated builds, that covers CI and CD for automated deployments.
- Can also explore to utilize with On Premises, Lamda and EB Instances
- Can utilize Blue Green Deployments with zero downtime
- Can utilize Load Balancers and Auto Scaling features for large scale applications
- We can also use Jenkins or Solano instead of AWS CodeBuild per requirements/client need
- We can also use S3 Bucket, AWS CodeCommit for code repositories instead of GitHub per requirements

## Additional Details
- We will have `buildspec.yml` file inside repository for AWS CodeBuild, where we can define all build specific required commands which we wants to execute during build generation time. Which includes version, environment variables, phases ( install, pre-build, build and post build) attributes to add your desired commands and settings for build generation.
- Similarly, we will have ‘appspec.yml’ file inside our repository for AWS CodeDeploy, where we can define all application specific required commands which we wanted to execute during deployment time. Which includes version, os, files location for extracted build deployment, and hooks (before install, after install, application start and validate service) attributes to add your desired bash file for your deployments.
- Inside AWS CodeBuild, it deploys your build in specified environment first to test, also you may configure your custom environments by using ‘advanced settings’. For more details you may see https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref.html#build-env-ref-env-vars 
- AWS CodePipeline for different environments, we can setup multiple environments with in a single AWS CodePipeline by adding different required actions inside your pipeline, also you may add manual approval processes for approval and hold deployments on any specific environment for testing.
- Manual Approval process can be added by the help of AWS SNS services.
- AWS CodePipeline, there are no upfront fees or commitments. You pay only for what you use. AWS CodePipeline costs $1 per active pipeline* per month. Where an active pipeline is a pipeline that has existed for more than 30 days and has at least one code change that runs through it during the month.

## EC2 Instance Requirements:
- Inbound 80 port should be allowed for HTTP
- S3 Storage access permission role should be assigned
- Apache2 should be installed
- PHP 7 should be installed
- Code Deploy Agent should be installed

## Install Apache2

> $ sudo apt-get update

> $ sudo apt-get install apache2
	
## Install PHP 7.0

> $ sudo apt-get update

> $ sudo apt-get install -y php7.0 libapache2-mod-php7.0 php7.0-cli

> $ sudo /etc/init.d/apache2 restart

You may verify by:

> $ php -v
	
# Install Code Deploy Agent

> $ sudo apt-get update

> $ sudo apt-get install ruby # Ubuntu 16
or
> $ sudo apt-get install ruby2.0 # Ubuntu 14 

> $ sudo apt-get install wget

> $ cd /home/ubuntu

> $ wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install

> $ sudo chmod +x ./install

> $ sudo ./install auto

> $ sudo service codedeploy-agent status

> $ sudo service codedeploy-agent start


## References
- https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ubuntu.html 
- https://aws.amazon.com/getting-started/tutorials/continuous-deployment-pipeline/
- https://aws.amazon.com/blogs/devops/continuous-delivery-for-a-php-application-using-aws-codepipeline-aws-elastic-beanstalk-and-solano-labs/ 
- https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-example.html 
- https://github.com/awslabs/aws-codedeploy-samples 
- https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref.html#build-env-ref-env-vars 

