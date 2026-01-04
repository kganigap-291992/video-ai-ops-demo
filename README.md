
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
- [x] AI anomaly detection


## Future Work (Planned ML Extension)
- Generate session-level feature dataset (startup_ms, buffer_count, total_buffer_ms, max_buffer_ms, played_ratio)
- Train anomaly detection model (Isolation Forest or One-Class SVM) using mostly-normal sessions
- Batch score daily sessions and write anomaly scores to S3
- Trigger alerts when anomaly volume or QoE percentiles regress
- Add data quality checks + drift monitoring to keep scoring reliable over time


## Project Scope & Constraints

This project is intentionally scoped to remain **low-cost, low-volume, and safe to run in a personal AWS account**.

Key constraints and design decisions:

- Telemetry volume is intentionally small (single video, limited sessions)
- Data is stored as lightweight JSON events in S3
- Analysis is performed using Athena with very small scan sizes (KB-level per query)
- No always-on services (no long-running EC2, no streaming ingestion)
- ML components are **designed but not deployed** to avoid unnecessary training and inference costs
- Batch-based analysis is preferred over real-time pipelines

The goal of the project is to **demonstrate end-to-end Video AIOps concepts and architecture**, not to operate at production scale. 

## Project Status

This project is currently in a **paused state** to prevent unnecessary AWS costs and exposure.

- Telemetry ingestion is disabled via `x-demo-token` rotation
- API Gateway throttling is set to zero to block public ingestion
- Public infrastructure (CloudFront, S3, Lambda) remains intact
- Collected telemetry data is preserved for analysis and future use
- The system can be re-enabled quickly if needed

This pause reflects intentional cost, security, and lifecycle management rather than inactivity.
