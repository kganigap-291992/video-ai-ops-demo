# Day 2 – Uploading Video to S3

## What I did
- Created a new S3 bucket for the project
- Uploaded a 30 MB MP4 video
- Configured bucket policy for public read access
- Tested access using the S3 object URL

## What I observed
- Opening the S3 object URL downloads the file instead of streaming
- This is expected behavior for raw S3 URLs

## What I learned
- S3 can act as a basic video origin
- Browser streaming requires a video player or CDN
- Bucket policies are required for public access

## Next step
- Add CloudFront CDN in front of S3
