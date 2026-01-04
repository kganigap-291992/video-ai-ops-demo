# Day 6 – AIOps/QoE Metrics (Phase 1)

## Goal
Turn raw playback events into session-level QoE metrics that can detect “bad video sessions”.

## What is a session?
A session is one page load of the player. Events are grouped by `sessionId` (client-generated).

## QoE Metrics (Phase 1)

### 1) Total Buffering Time (total_buffer_ms)
- Source: `buffer_end` events include `bufferMs`
- Computation: sum(bufferMs) per session
- Why it matters: long buffering correlates strongly with poor experience

Initial thresholds (rule-based):
- Warning: >= 2000 ms
- Bad: >= 5000 ms

### 2) Startup Delay (startup_ms)
- Source: `page_loaded` timestamp and first `playing` timestamp
- Computation: first_playing_ts - page_loaded_ts
- Why it matters: users drop quickly if startup is slow

Initial thresholds (rule-based):
- Warning: >= 1500 ms
- Bad: >= 3000 ms

## Detection logic (rules first, ML later)
A session is “bad” if:
- total_buffer_ms >= 5000 OR
- startup_ms >= 3000

## Next steps
- Write a small script to read events from S3 and compute session metrics
- Generate a daily summary: p50/p95 startup and buffering
- After enough data, learn thresholds automatically (basic anomaly detection)
