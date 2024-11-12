# Deploy Static HTML to AWS S3 with GitHub Actions

This guide provides steps to deploy a static HTML page to an AWS S3 bucket using GitHub Actions. The S3 bucket is configured to host a static website, and the workflow is triggered from the GitHub repository.

---

## Prerequisites

1. AWS account with permissions to create S3 buckets and IAM users.
2. GitHub repository with access to manage secrets and workflows.

---

## Steps

### 1. Configure S3 Bucket

#### a. Create an S3 Bucket

1. Go to the **S3** service in the AWS Management Console.
2. Click on **Create bucket**.
3. Enter a unique **Bucket name** (e.g., `deploy-static-html-bucket`).
4. Uncheck **Block all public access**.
5. Check the **I acknowledge** box.
6. Click **Create bucket**.

#### b. Enable Website Hosting

1. Go to the **Properties** tab of your S3 bucket.
2. Click **Edit** under **Static website hosting**.
3. Select **Enable**.
4. Set the **Index document** to `index.html`.
5. Click **Save**.
6. Note the **Static website hosting URL** provided after enabling.

#### c. Add Bucket Policy

1. Go to the **Permissions** tab of your S3 bucket.
2. Click **Edit** under **Bucket policy**.
3. Add the following policy, replacing `<BUCKET_NAME>` with your bucket name:
   
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::deploy-static-html-bucket/*"
           }
       ]
   }

4. Click **Save changes**.


### 2. Configure IAM User for Access Keys
1. Go to the **IAM** service in the AWS Management Console.
2. Click on **Users** and then **Add user**.
3. After creating, you will see **Access Key ID** and **Secret Access Key**. Copy these values for later use.
4. Click **Add permissions** and attach the **AmazonS3FullAccess** policy.

### 3. Store IAM Access Keys in GitHub
1. In your GitHub repository, go to **Settings** > **Secrets and variables** > **Actions**.
2. Click on **New repository secret**.
3. Add secrets with the following names and paste the respective values:
   - `AWS_ACCESS_KEY_ID` - Your IAM Access Key ID
   - `AWS_SECRET_ACCESS_KEY` - Your IAM Secret Access Key

### 4. Configure GitHub Workflow
1. Upload your static HTML file to the repository (e.g., `index.html`).
2. Go to **Actions** in your GitHub repository.
3. Select **Set up a workflow yourself** or create a new `.yml` file in `.github/workflows`.
4. Define your workflow to upload files to S3 on each push.
5. Commit the workflow.
6. Go to the S3 Static website hosting URL to verify the deployment.

    

> Note: Replace `deploy-static-html-bucket` with your actual bucket name.
