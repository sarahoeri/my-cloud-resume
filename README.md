# Cloud Resume Challenge 
This challenge aims to host a Static Website, which is a Cloud Resume, on AWS using Amazon S3, AWS CloudFront, AWS Certificate Manager, and Amazon Route 53. An Amazon S3 bucket will host the static website. Amazon Route53 will route traffic to the website domain name through a CloudFront distribution. The AWS Certificate Manager will deploy an SSL/TLS certificate for the website so that we can access it securely through HTTPS. 

## Step-by-Step Guide

### 1. Write HTML and CSS for your resume using your favourite IDE
In my case, I used a template from CloudmyTribe Community. You can write your own HTML and CSS code to customize your resume to your liking. I used Gitpod, a Cloud Development Environment with the Visual Studio Code IDE, where I edited the template to add my resume details. Gitpod can be integrated with Github through the Gitpod extension for your browser. This enables you to commit and push your code to your GitHub repository.
### 2. Upload your code to an Amazon S3 bucket
After creating your bucket, upload your code file to an Amazon S3. Since the bucket is for static website hosting, unselect 'Block all public access' under your bucket permissions. Also, enable the static website hosting property for your bucket. You need to provide a bucket policy to enable access to the files.

### 3. Create a hosted zone in Amazon Route 53
A hosted zone is a container for records containing information about how you want to route traffic for a specific domain. It tells Route 53 how to respond to DNS queries. You need a registered domain name. I purchased an affordable option with a .buzz extension at Host Africa. Proceed to create a public hosted zone using the purchased domain name. Navigate to your domain registrar and locate Nameservers under DNS Management. Edit the nameservers to enter your custom nameservers from the AWS Route 53 hosted zone you created and save.
### 4. Generate a certificate using AWS Certificate Manager
In ACM, request a public certificate that will provide SSL to enable secure access to your website via HTTPS instead of HTTP, which is not secure. To request a public certificate, enter your fully qualified domain name and leave other options as defaults. The certificate created is in pending validation status, as it requires more configurations and creating a record set to be validated.
### 5. Create a Record Set in Amazon Route53
Navigate to the Route 53 dashboard, click on the hosted zone created in the previous steps, and then click Create record. In this step, select the record type as the CNAME record. The purpose of the CNAME record is to map a domain to another domain name. In this case, it will help map the purchased domain name to the Amazon S3 bucket so that a user can access the S3 bucket through the domain name instead of the long bucket website endpoint URL. The record name for this record needs to be a subdomain. Navigate to AWS Certificate Manager to the certificate previously created. Under Domains, copy the CNAME name and CNAME value to a text editor. Copy the first part of the CNAME name up to the first period and paste it into the Record name field. Copy the whole CNAME value and paste it into the Value field. Leave other fields as default and click Create record. After this configuration, the pending validation status for the certificate created in AWS Certificate Manager should be validated and issued after a few seconds.
### 6. Create a CloudFront Distribution
Navigate to CloudFront Dashboard and click Create a CloudFront Distribution. In the Origin Domain field, paste the bucket website endpoint URL and delete the 'HTTP://' part of the URL. Under Viewer Protocol Policy, select Redirect HTTP to HTTPS since we now have the SSL certificate that can be used for security. Under Alternate Domain name, click Add Item and enter your domain name and select your SSL certificate under Custom SSL certificate. Leave the other options as default and click Create Distribution. This configuration will enable the website content to be served over HTTPS using SSL/TLS encryption to secure data in transit.
#### Create an Alias record in Amazon Route 53 to Route traffic to the CloudFront Distribution
In Route 53 dashboard, create a record in the hosted zone created in the previous steps. Select the record type to A, which maps a domain to an IP Address. Under Route traffic to, select Alias to CloudFront distribution and select the distribution created earlier. Toggle the Alias button to activate it and create the record. 
## Validation
This steps of configuration should now enable the static website to be securely accessed through your domain name. Notice the padlock icon on my Cloud Resume static website https://sarahcloudresume.buzz/
## Challenges Faced
- This project was pretty straightforward. Although when creating a CloudFront distribution, it may fail to be created because of account validation required which prevents the distribution from being created. From the error message, you are required to contact AWS Support for your account to be validated in order to create a CloudFront distribution. From some research, this issue can be solved without contacting support. Create a t2 micro EC2 instance and terminate it after 20 min. This solution solves the problem. It could be an issue related to quotas, this solution increases zeroed quotas and clears the account verification message. 
## Key Takeaways
- This project solidified my understanding of Route 53 (hosted zones, record sets), AWS Certificate Manager (generating and validating SSL certificates), and Amazon CloudFront (distributions), which I had not previously interacted with hands-on to deliver a solution.
# References
- How to host a static website on Amazon S3 https://kevinkiruri.medium.com/hosting-a-static-website-on-aws-s3-35f49dd1c5e6
- Amazon Route 53 Documentation
- AWS Cerificate Manager Documentation
- Amazon CloudFront Documentation
- How to Host a Static Website using S3, Route 53 and CloudFront https://youtube.com/playlist?list=PLqoUmUbJ_zDGTajZjNpYI3QAJhu6sLgaB&si=Yvb0CN2q7I__TDW9

