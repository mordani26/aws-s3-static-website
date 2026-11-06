# Static Website Hosting with Amazon S3

## Overview

This was my first hands-on AWS project. I followed a NextWork tutorial to deploy a static website using Amazon S3. The goal was to get familiar with the AWS Management Console, learn how S3 stores files, and understand how permissions affect access to a hosted website.

The HTML page and images were provided by the tutorial. My work focused on setting up the AWS resources, configuring access, and getting the site online.

**AWS service:** Amazon S3  
**Completed:** November 2026

## Project Setup

1. Created an S3 bucket in the AWS Management Console with a unique bucket name. I enabled ACLs and selected *Bucket owner preferred* for object ownership, as instructed in the lab.
2. Updated the Block Public Access settings to allow the website files to be publicly readable for this exercise.
3. Downloaded the provided `index.html` file and image assets, extracted the images, and uploaded the website files to the bucket.
4. Enabled static website hosting under the bucket's Properties tab and set `index.html` as the index document.
5. Opened the S3 website endpoint. The files were initially private, so I updated their access permissions using ACLs and confirmed the website loaded in my browser.

The setup used an S3 website endpoint to serve the HTML and image files directly. It did not require an EC2 instance or a database.

## S3 Bucket Policy

I also practiced working with a bucket policy written in JSON. I added an explicit deny rule for `s3:DeleteObject` on the `index.html` file. This prevented normal deletion of that object while the policy was in place, including deletion attempts by the bucket owner.

```json
{
  "Version": "2012-10-17",
  "Id": "MyBucketPolicy",
  "Statement": [
    {
      "Sid": "BucketPutDelete",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/index.html"
    }
  ]
}
```

`YOUR-BUCKET-NAME` is a placeholder for the bucket name used in my lab. The important parts of the policy are the `Deny` effect, the `s3:DeleteObject` action, and the resource ARN identifying the HTML file.

This helped me understand that bucket policies can restrict specific actions on specific objects, not just control who can view them. The deny policy needs to be removed before deleting the file during cleanup.

## What I Learned

- How to create an S3 bucket and upload objects.
- How to configure S3 static website hosting and use the website endpoint.
- The difference between enabling hosting and granting permission to access the website files.
- How ACLs and JSON bucket policies control access to S3 objects.
- Why checking resource permissions is important when troubleshooting an `AccessDenied` error.
- Why AWS resources should be reviewed and cleaned up after a lab to avoid unnecessary costs.

## Screenshots

Screenshots of the bucket, uploaded files, hosting configuration, and working website can be added to a `screenshots/` folder in this repository. They provide a record of the deployment even if the AWS resources are later deleted.

## Cost and Security Notes

The tutorial used public object access so the site could be viewed through the S3 website endpoint. This was suitable for a practice lab with non-sensitive files, but it is not how I would set up a production website. A future improvement would be to keep the bucket private and use Amazon CloudFront with HTTPS.

I also learned that S3 can charge for storage, requests, and data transfer. The tutorial recommends removing the uploaded objects and bucket when finished. The documentation and screenshots can remain on GitHub after the AWS resources are removed.

## Reference

This project followed the [NextWork "Host a Website on Amazon S3" tutorial](https://nextwork.ai/projects/5f1cbfc6-6346-4c1f-9164-81d5a35327e1) by Maya Menon. The starter website files came from NextWork.
