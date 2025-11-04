In this guide, I’ll walk you through the two most popular methods of static website hosting, using screenshots from a real deployment I did recently.

The “Classic” Method: Manually combining S3 for storage and CloudFront (a CDN) for secure, high-speed delivery. This gives you maximum control.
The “Managed” Method: Using AWS Amplify, a modern, all-in-one service that handles everything from code to deployment with a built-in CI/CD pipeline.
Let’s break down both.

🛠️ Method 1: The “Classic” Build (S3 + CloudFront)
This is the foundational method. You manually provision the storage (S3) and the delivery network (CloudFront). This is perfect for understanding exactly how the services fit together.

Step 1: Create and Prep Your S3 Bucket

First, we need a place to store our index.html, style.css, and other files.

Create the Bucket: In the AWS Console, go to S3 and create a new bucket. Give it a unique name. In our example, we’re using portwebgh in the us-east-1 region.

<img width="720" height="403" alt="image" src="https://github.com/user-attachments/assets/87782867-6f22-48b2-952c-f45cfff2f7c8" />


2.Upload Your Files: Click into your new bucket and upload your static website files from the Objects tab. At a minimum, you’ll need an index.html file.

<img width="720" height="405" alt="image" src="https://github.com/user-attachments/assets/9bdbba68-208e-47be-a34e-8d061faceed0" />


Step 2: Configure Bucket Permissions
This is the most critical step. We need to allow public read access to the objects in the bucket.

Turn Off “Block Public Access”: By default, S3 blocks all public access. In your bucket’s Permissions tab, Edit the “Block public access (bucket settings)” and uncheck the “Block all public access” box.

<img width="720" height="410" alt="image" src="https://github.com/user-attachments/assets/04aa8395-19f9-4faa-98ee-3cdf60419dc6" />


2. Apply a Public Read Bucket Policy: Still on the Permissions tab, scroll to Bucket policy and paste in a policy that allows public s3:GetObject actions. Remember to replace portwebgh with your bucket's name!

{     "Version": "2012-10-17",     
"Statement": [{"Sid": "PublicReadGetObject",
"Effect": "Allow",
"Principal": "*",
"Action": "s3:GetObject",
"Resource": "arn:aws:s3:::portwebgh/*"
}] }

Step 3: Enable Static Website Hosting
Now, we tell S3 to treat this bucket as a website.

Go to the Properties tab in your bucket.
Scroll down to Static website hosting and click Edit.
Enable it and set your “Index document” to index.html.
After saving, S3 will give you a Bucket website endpoint. Copy this URL.
<img width="720" height="409" alt="image" src="https://github.com/user-attachments/assets/b17513c5-fc86-438c-95db-79f8f0d72ac0" />


Step 4: Create a CloudFront Distribution
We could stop here, but the S3 endpoint is slow and not secure (HTTP). We’ll use CloudFront (AWS’s CDN) to fix this.

Navigate to the CloudFront service.
Click Create distribution.
Origin domain: Paste the S3 bucket website endpoint you copied in the last step.
Viewer protocol policy: Set this to Redirect HTTP to HTTPS.
Default root object: Type index.html.
Click Create distribution.
It will take a few minutes to deploy. You’ll see its status as “Deploying” or “Enabled” on the dashboard. Once enabled, your site is live and securely served at the Distribution domain name (e.g., d5gp...cloudfront.net).
<img width="720" height="405" alt="image" src="https://github.com/user-attachments/assets/336898b6-adaa-4b5f-858c-93fd9ffafc2e" />


🚀 Method 2: The “Managed” Alternative (AWS Amplify)
If the steps above seem too manual, or if you’re a developer who loves a good Git-based workflow, AWS Amplify is for you. It’s an all-in-one service that handles the build, deployment, and hosting for you, automatically setting up S3 and CloudFront under the hood.

Step 1: Create Your Amplify App

In the AWS Amplify console, you start by creating a “new app” and connecting it to your Git provider (like GitHub, GitLab, Bitbucket, or AWS CodeCommit).

You can see our portweb app is already created and shows as "Deployed."
<img width="720" height="408" alt="image" src="https://github.com/user-attachments/assets/a5a14246-9819-4540-af2d-6bebdcd7243e" />


Step 2: Connect & Deploy Your Branch
Amplify’s magic is its CI/CD pipeline. When you push code to a specific branch (e.g., main or staging), Amplify automatically:

Detects the push.
Builds your app (if it’s a framework like React or Vue).
Deploys the static files.
Here, you can see our staging branch is deployed. Amplify automatically provides a secure ...amplifyapp.com domain for every branch, which is perfect for testing.

<img width="720" height="411" alt="image" src="https://github.com/user-attachments/assets/e16139e8-6b38-409e-b390-87ef35b985aa" />


Step 3: Your Site is Live!
And that’s it! You can now visit your Amplify domain, and your website is live. In the background, Amplify has automatically created an S3 bucket, configured a CloudFront distribution, and applied an HTTPS certificate. You get all the benefits of Method 1, but with a fraction of the manual setup and a built-in CI/CD pipeline.

<img width="720" height="405" alt="image" src="https://github.com/user-attachments/assets/c405926b-334d-4135-ad27-f45e56bf162a" />



Lastly, let’s take a look at the bucket policy after the hosting off s3 for both the CloudFront and s3 and amplify with s3 as the source or origin.

<img width="640" height="365" alt="image" src="https://github.com/user-attachments/assets/4cc527a2-c10e-422f-914f-8a102eb1621c" />
<img width="640" height="368" alt="image" src="https://github.com/user-attachments/assets/1464e751-3fa5-421f-95c6-690ce10a7dd2" />

🤔 Which Method Should You Choose?
Both methods are excellent, but they serve different needs:

Choose S3 + CloudFront (Method 1) if:
You need granular control over every aspect of the S3 bucket policy or CloudFront cache behavior.
You are deploying files manually and not using a Git-based workflow.
You are learning the AWS fundamentals and want to build the infrastructure yourself.
Choose AWS Amplify (Method 2) if:
Your code lives in a Git repository.
You want a fast, automated CI/CD pipeline for builds and deployments.
You want features like atomic deployments, branch-based previews, and easy custom domain management.
You prefer a managed, “serverless” experience and don’t want to configure S3 or CloudFront manually.
Jump right into my YouTube channel for video tutorial for better understanding.
https://youtu.be/b8ihCyObsyU
https://youtu.be/plGC2cK0tvM


Happy building!
