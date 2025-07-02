A single-file HTML application that allows a user to:

1. Enter their AWS credentials:
   - Access Key ID
   - Secret Access Key
   - AWS Region
   - Bucket name
   - Folder prefix (e.g., `csm-uploads/`)

2. See a list of files already in the S3 folder

3. Upload a new file to that same folder

Requirements:

- Use the AWS SDK for JavaScript v2 (served via CDN) that runs entirely in-browser
- Implement UI with only HTML, inline JavaScript, and inline CSS (no external files or frameworks)
- Use `s3.listObjectsV2` to get the file list under the given prefix
- Use `s3.upload` or `putObject` to upload selected files
- Validate inputs (bucket name, keys, prefix)
- Display:
   - A list of existing files (name + size)
   - Upload progress/status
   - Errors if credentials are invalid

Keep the design simple and clean so non-technical users can operate it. Do not use external dependencies beyond the AWS SDK.


Requires AWS IAM Policy:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PutObjectToFolder",
      "Effect": "Allow",
      "Action": ["s3:PutObject"],
      "Resource": "arn:aws:s3:::my-saas-files/csm-uploads/*"
    },
    {
      "Sid": "ListFolderObjects",
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": "arn:aws:s3:::my-saas-files",
      "Condition": {
        "StringLike": {
          "s3:prefix": "csm-uploads/*"
        }
      }
    }
  ]
}



Requires S3 Cors:
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": ["ETag"],
    "MaxAgeSeconds": 3000
  }
]