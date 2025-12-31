# Day 3 – CloudFront CDN Setup

## What I did
- Created a CloudFront distribution with S3 as the origin
- Enabled Origin Access Control (OAC) to secure S3
- Updated S3 bucket policy to allow access only from CloudFront
- Fixed AccessDenied issues by correcting OAC permissions
- Updated object Content-Type to `video/quicktime`
- Invalidated CloudFront cache to refresh headers

## What I observed
- Direct S3 access is blocked after enabling OAC
- CloudFront successfully serves the video
- Safari plays `.MOV` inline
- Chrome downloads `.MOV` due to browser behavior

## What I learned
- CloudFront + OAC is the correct secure pattern for S3 origins
- Object-level metadata (Content-Type) affects browser playback
- CDN caches headers and requires invalidation after metadata changes
- Browser behavior differs for `.MOV` vs `.MP4`

## Next step
- Add an HTML video player and capture playback events
