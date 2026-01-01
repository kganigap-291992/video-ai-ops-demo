
# Day 4 – Video Telemetry Ingestion Pipeline

## Overview
In this step, I built an end-to-end video playback telemetry pipeline.  
A video player served via CloudFront emits playback events which are sent to AWS, processed server-side, and stored for analysis.

---

## Architecture Flow
Browser (HTML5 Video Player)
→ CloudFront (CDN)
→ API Gateway (HTTP API)
→ Lambda (Python)
→ S3 (Telemetry Storage)

---

## What I Built

### Client-side (Browser)
- HTML5 video player served through CloudFront
- JavaScript listeners for playback events:
  - play, pause, seeked
  - buffer_start, buffer_end
  - error
- Each event is captured with:
  - timestamp
  - current playback time
  - video duration
  - buffering duration (where applicable)

### Backend (AWS)
- API Gateway HTTP API exposing `POST /events`
- Lambda function that:
  - parses incoming JSON events
  - enriches events with server timestamp and request ID
  - writes each event as a JSON object to S3
- S3 bucket used as durable telemetry storage

## Security & Hardening

- Added required `x-demo-token` validation in the telemetry ingestion Lambda.
- Lambda returns `401 Unauthorized` when the token is missing or invalid.
- Token value is stored securely as a Lambda environment variable.
- Enabled API Gateway throttling on the `$default` stage using rate and burst limits.
- Verified protection by running burst tests that returned non-200 responses (503/429) under load.

---

## Example Telemetry Event

```json
{
  "event": "seeked",
  "ts": "2026-01-01T00:19:33.525Z",
  "currentTime": 18.82,
  "duration": 21.1,
  "paused": false,
  "_server_ts": 1767226773.6954284,
  "_request_id": "6a8be010-d708-4534-a081-51734a566f9a"
}
