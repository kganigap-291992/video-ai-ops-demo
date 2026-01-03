# Day 5 — Baseline QoE Anomaly Detection (v1)

## Problem Statement
Detect playback quality anomalies using browser telemetry before applying ML-based models.

## Telemetry Signals (v1)
We distinguish between **user behavior events** and **QoE signals**.

**Events collected from browser:**
- play
- pause
- buffer_start
- buffer_end
- error

Each event should include:
- sessionId
- ts (timestamp)
- currentTime (when applicable)
- duration (when applicable)

## QoE Metric Definitions

### Buffering Duration (per session)
Sum all buffering segments for the session:
buffer_duration = Σ(buffer_end_ts - buffer_start_ts)


### Playback Duration (per session)
Use the last reported playback position:
play_duration = last_reported_currentTime


### Rebuffer Ratio
rebuffer_ratio = buffer_duration / play_duration


## Aggregation Strategy
Compute metrics over **5-minute windows**.

For each window:
- total_sessions
- avg_rebuffer_ratio
- error_rate (future)

## Baseline Anomaly Detection (v1)
Compute a rolling baseline using recent history:

mean = rolling_mean(avg_rebuffer_ratio)
std = rolling_std(avg_rebuffer_ratio)
z_score = (current_value - mean) / std



Flag anomaly when:
- z_score >= 3
- total_sessions >= 20
- avg_rebuffer_ratio >= 0.05 (5%)

Ref: https://medium.com/@akashsri306/detecting-anomalies-with-z-scores-a-practical-approach-2f9a0f27458d

## Why This Approach
- Works with small datasets
- Explainable and debuggable
- Provides a baseline to compare ML-based detection later

## Next Steps
- Instrument browser to emit buffer_start / buffer_end events with sessionId
- Implement 5-min aggregation job (EventBridge -> Lambda)
- Store aggregated metrics for dashboards and detection

## Validation with Real Browser Data (Safari)

- Deployed updated player to S3 and invalidated CloudFront cache
- Verified session-scoped telemetry using `sessionId`
- Captured real buffering signals in Safari using `stalled` and `waiting` events
- Observed buffering behavior under constrained network conditions
- Measured buffering duration using `performance.now()`

### Sample Buffering Event Observed
- `buffer_start` triggered due to stalled playback
- `buffer_end` captured after playback resumed
- Buffer duration observed: ~67 seconds
- Playback time at stall: ~3.4 seconds

### QoE Calculation from Real Session
buffer_duration_s = 66.97
play_duration_s = 3.4
rebuffer_ratio = 19.69


This represents severe QoE degradation and validates the anomaly detection approach.

### Key Learnings
- Safari often emits `stalled` instead of `waiting` during buffering
- Buffering may not resolve until playback resumes (`playing` event)
- Even a single bad session can significantly impact aggregated QoE metrics
- Real telemetry behaves differently than synthetic assumptions
