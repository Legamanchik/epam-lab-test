# epam-lab-test
Step 1: Sign in to AWS Management Console
Go to the AWS Management Console.
Sign in with your AWS account credentials.

Step 2: Create an S3 Bucket
In the AWS Management Console, navigate to S3 by searching for "S3" in the services search bar.

Click on Create bucket.
Enter a unique name for your bucket. (Bucket names must be globally unique across all AWS accounts.)
Select the region where you want to create the bucket.
Uncheck Block all public access. (This is necessary for hosting a public static website.)
Acknowledge the warning that appears about public access.

Click on Create bucket.

Step 3: Configure the Bucket for Static Website Hosting

Click on the name of the bucket you just created.

Go to the Properties tab.

Scroll down to the Static website hosting section.

Click on Edit.

Select Enable.

Enter the name of your index document (e.g., index.html).
Optionally, you can specify an error document (e.g., error.html).

Click Save changes.

Step 4: Set Bucket Policy for Public Access
Go to the Permissions tab of your bucket.

Click on Bucket Policy.

Add the following policy to allow public read access to your bucket. Replace YOUR_BUCKET_NAME with the name of your bucket:

json

    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
        }
    ]


Click Save.

Create a GitHub Repository

Log in to GitHub:

Go to GitHub and log in to your account.

Create a New Repository:

Click on the New button or navigate to the + icon in the upper right corner and select New repository.

Name your repository (e.g., my-personal-website).

Optionally, add a description.

Select Public if you want it to be publicly accessible.

Click on Create repository.

Push Your Code to GitHub:

Create a GitHub Actions Workflow:

In your GitHub repository, create a directory named .github/workflows and inside it, create a file named deploy.yml.
The directory structure should look like this:

my-personal-website
└── .github
    └── workflows
        └── deploy.yml
Add Workflow Configuration:

Add the following content to deploy.yml:
name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: YOUR_AWS_REGION

    - name: Sync files to S3
      run: aws s3 sync . s3://YOUR_BUCKET_NAME --exclude ".git/*" --exclude ".github/*"
      
Replace YOUR_AWS_REGION with your desired AWS region (e.g., us-east-1).

Replace YOUR_BUCKET_NAME with the name of your S3 bucket.

Store AWS Credentials as Secrets:

In your GitHub repository, go to Settings > Secrets and variables > Actions > New repository secret.
Create the following secrets:

AWS_ACCESS_KEY_ID: Your AWS access key ID.

AWS_SECRET_ACCESS_KEY: Your AWS secret access key.

Push all changes and return to S3 copy url and enjoy your beautifull website
