# earthmuffin-web

## Development

`yarn run blendid`

## Deployment to AWS

### Create s3 website bucket

- Create bucket with and name it `earth-muffin.org`
- Go to bucket permissions and set:
1. `Block public access` for all items to `off`
2. `Bucket policy` to public

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AddPerm",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::earth-muffin.org/*"
        }
    ]
}
```

- Go to bucket properties and enable `Static website hosting`

### Create s3 website redirect bucket

- Create bucket with and name it `www.earth-muffin.org`
- Go to bucket permissions and set:
1. `Block public access` for all items to `off`
2. `Access Control List` to grant access to list objects in `Public Access` section

- Go to bucket properties and enable `Static website hosting` and set https redirect to `earth-muffin.org` bucket

### Deployment user & configuration

- Go to IAM console and create `s3-deployment` group with following permissions:
1. AmazonS3FullAccess
2. CloudFrontFullAccess
- Create user e.g. `earthmuffin-org-travis` and attach policies from `s3-deployment` group
- Store the `credentials.cvs` to configure travis build

### CloudFront distribution

- Create new distribution with origin matching endpoint without protocol from s3 `Static website hosting` section
- Create and link SSL certificate with ACM (N. Virginia)

In case you are not able to create CloudFront distribution because of account
verification or create certificate in ACM contact AWS support.

### Route 53

- Create `CNAME` record set pointing to endpoint without protocol from s3 `Static website hosting` section
- Create `A` record set pointing to CloudFront distribution domain name

Check if your NS records match with registered domain by using e.g. `whois` tool.

### Troubleshooting

- https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html
- https://aws.amazon.com/premiumsupport/knowledge-center/s3-rest-api-cloudfront-error-403/
- https://aws.amazon.com/premiumsupport/knowledge-center/s3-website-cloudfront-error-403/