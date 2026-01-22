IAM Role: LambdaS3AccessRole
Purpose:
Provides AWS Lambda functions with secure, least‑privilege access to the project’s S3 bucket for reading and writing anonymized text data, and to CloudWatch for logging execution details.

Attached Policies
- Custom S3 Access Policy (`LambdaS3AccessPolicy`)
`{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::cognify-s3-requests/*"
    }
  ]
}`


- Scope: Restricted to a single S3 bucket (your-bucket-name).
- Actions Allowed:
- GetObject → Read files from the bucket.
- PutObject → Write processed outputs back to the bucket.
- Rationale: Ensures Lambda cannot access other buckets or perform unnecessary actions.

- CloudWatch Logging Policy (`AWSLambdaBasicExecutionRole`)
- Managed Policy provided by AWS.
- Grants permissions to:
- logs:CreateLogGroup
- logs:CreateLogStream
- logs:PutLogEvents
- Purpose: Enables Lambda to write execution logs to CloudWatch for monitoring and debugging.

Trust Relationship
`{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "lambda.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}`


- Trusted Entity: AWS Lambda service.
- Purpose: Allows Lambda functions to assume this role at runtime.

Usage Notes
- Attach this role to all Lambda functions that interact with the project’s S3 bucket.
- Verify CloudWatch logs after each Lambda execution to confirm proper logging.
- Rotate access keys if using CLI/SDK for developer roles.
- Document updates to policies in this README for transparency and governance.
