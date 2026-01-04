### Day 7 – Telemetry Analysis & Playback Anomaly Detection (Athena)

Converted raw client-side video playback telemetry into actionable Quality-of-Experience (QoE) insights using Amazon Athena.

- Queried JSON telemetry stored in S3 directly using Athena (no ETL required)
- Built session-level QoE metrics from low-level playback events:
  - Startup latency
  - Buffering frequency
  - Total and worst-case buffering duration
- Identified real playback anomalies by comparing degraded vs healthy sessions
- Validated that telemetry metrics accurately reflect observed playback behavior
- Applied a metrics- and rules-based AIOps approach (intentionally pre-ML) to prioritize correctness and explainability
- Reinforced operational safeguards with token-based request validation and API Gateway throttling to prevent abuse and unexpected costs

---

### Session-Level Metrics Derived

Each playback session was aggregated to compute:

- **startup_ms** – time from page load to first `playing` event  
- **buffer_count** – number of buffering events  
- **total_buffer_ms** – total buffering duration per session  
- **max_buffer_ms** – worst single buffering event  

These metrics transform noisy event logs into user-experience signals.

---

### Example Findings

| sessionId | startup_ms | buffer_count | total_buffer_ms | max_buffer_ms |
|----------|-----------:|-------------:|----------------:|--------------:|
| 9e21d601 | 20069 | 2 | 68071 | 66968 |
| b7d84ac7 | 2078 | 0 | 0 | 0 |

One session exhibited ~20s startup latency and ~68s buffering, while another showed sub-2s startup with no buffering.

---

### Interpretation

- High startup time and buffering clearly indicate degraded playback quality
- Healthy sessions demonstrate fast startup and stable playback
- Derived metrics align closely with observed user playback behavior
