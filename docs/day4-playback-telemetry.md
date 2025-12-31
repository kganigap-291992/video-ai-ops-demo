# Day 4 – Playback Telemetry (Client-side)

## What I did
- Added JavaScript event listeners to the HTML5 `<video>` player
- Logged playback events: play, pause, seeking/seeked, ended, error
- Added buffering telemetry by measuring duration between `waiting` → `playing`
- Rendered events as JSON on the page and in the browser console

## Key events emitted
- page_loaded
- play / pause
- seeking / seeked
- buffer_start / buffer_end (bufferMs)
- ended
- error

## What I learned
- The browser emits native playback lifecycle events that can be used as telemetry
- Buffering duration is a key QoE metric for video delivery (AIOps signal)
- These events are the foundation for backend ingestion, metrics, and anomaly detection

## Next step
- Send telemetry events to AWS via API Gateway + Lambda
