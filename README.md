# earthmuffin-web

## Development

`yarn run blendid`

## Deployment

### Create s3 bucket

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

### Deployment user & configuration

- Go to IAM console and create `s3-deployment` group with following permissions:
1. AmazonS3FullAccess
2. CloudFrontFullAccess
- Create user e.g. `earthmuffin-org-travis` and attach policies from `s3-deployment` group
- Store the `credentials.cvs` to configure travis build

### Troubleshooting

- https://aws.amazon.com/premiumsupport/knowledge-center/s3-rest-api-cloudfront-error-403/
- https://aws.amazon.com/premiumsupport/knowledge-center/s3-website-cloudfront-error-403/