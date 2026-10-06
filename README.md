# Static Website on Amazon S3

A personal portfolio page hosted as a static website on Amazon S3 (region: `ap-southeast-2`, Sydney).

**Live site:** http://s3-demo-bucket-ugie.s3-website-ap-southeast-2.amazonaws.com/

## What this project demonstrates

- Creating and configuring an S3 bucket for static website hosting
- Setting the index document and managing public access with a bucket policy
- Troubleshooting S3 `403 AccessDenied` errors (Block Public Access, bucket policy, object ownership)
- Understanding the cost model of S3 hosting (storage, requests, data transfer)

## Architecture (current)

```
Browser  -->  S3 static website endpoint (HTTP)  -->  index.html
```

## How it was built

1. Created an S3 bucket in `ap-southeast-2`.
2. Enabled **Static website hosting** under bucket Properties, with `index.html` as the index document.
3. Turned off Block Public Access for the bucket and added a bucket policy allowing public `s3:GetObject` on the objects.
4. Uploaded `index.html` to the bucket root.

Bucket policy used:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicReadGetObject",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::s3-demo-bucket-ugie/*"
  }]
}
```

## Cost

For a small static site the cost is typically cents per month: storage is about US$0.023 per GB-month, GET requests about US$0.0004 per 1,000, and data transfer out is free for the first 100 GB per month. A billing alert is set up to avoid surprises.

## Limitations and roadmap

The S3 website endpoint only supports HTTP and requires a public bucket. Planned improvements:

- [ ] Put CloudFront in front of a **private** bucket using Origin Access Control, for HTTPS
- [ ] Define the infrastructure as code with Terraform
- [ ] Deploy automatically with GitHub Actions using OIDC (no stored AWS keys)
- [ ] Add security headers and CloudWatch alarms
- [ ] Custom domain with Route 53 and ACM

## Repository contents

| File | Purpose |
| --- | --- |
| `index.html` | The website (single self-contained page) |
| `README.md` | Project documentation |
