
# Video AI Ops Demo

This repository documents my learning journey building an end-to-end
video streaming and AI Ops pipeline on AWS.

## Why this project
I work in the video delivery domain and want to transition into
MLOps / AIOps roles by building a real-world system from scratch.

## Planned Architecture
- Video stored in S3
- Video delivered via CloudFront (CDN)
- Playback metrics captured from browser
- Metrics ingested via API Gateway + Lambda
- ML used for anomaly detection on video QoE

## Current Status
- [x] GitHub repository created
- [x] Video uploaded to S3
- [x] CloudFront distribution created
- [x] Video playback working
- [x] Metrics ingestion
- [ ] AI anomaly detection
