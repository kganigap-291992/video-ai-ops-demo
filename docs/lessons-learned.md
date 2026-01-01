# Lessons Learned & Operational Notes

This document captures all the real issues encountered during development and the steps taken to diagnose and fix them.  
---

## 1. Blank White Screen When Loading index.html

### Symptom
- CloudFront URL returned HTTP 200
- Browser showed a blank white page
- No visible errors at first glance

### Root Cause
- Only a `<script>` block was uploaded to S3
- The deployed `index.html` did not include `<html>`, `<body>`, or `<video>` elements
- Browser successfully loaded the file but had nothing to render

### Diagnosis Steps
- Used `curl -I` to confirm CloudFront returned `200 OK`
- Inspected S3 object content directly
- Realized mismatch between local file and deployed object

### Fix
- Re-uploaded full HTML document (not partial script)
- Invalidated CloudFront cache for `/index.html`

### Lesson
> A successful HTTP response does not guarantee valid application behavior.  
> Always verify deployed artifacts match local expectations.

---

## 2. Public API Endpoint and Cost Protection

### Concern
- API Gateway endpoint is public and referenced in client-side JavaScript
- Risk of abuse leading to unexpected AWS costs

### Initial State
- No authentication
- No rate limiting
- Lambda writes directly to S3 per request

---

## 3. Adding Lightweight Authentication (x-demo-token)

### Solution
- Added a required `x-demo-token` HTTP header
- Token stored securely as a Lambda environment variable
- Lambda returns `401 Unauthorized` for missing or invalid tokens

### Validation
- Requests without token returned `401`
- Requests with correct token returned `200`
- Verified telemetry objects were written to S3 only for authorized requests

### Lesson
> Even simple token validation significantly reduces accidental or automated abuse.

---

## 4. API Gateway Throttling to Prevent Abuse

### Solution
- Enabled API Gateway default route throttling
- Configured rate and burst limits on the `$default` stage

### Validation
- Ran burst tests using concurrent curl requests
- Observed non-200 responses (503/429) under load
- Confirmed normal traffic still succeeds

### Lesson
> Ingestion endpoints should be public but **cost-bounded**.  
> Throttling is a simple, effective first line of defense.

---

## Overall Takeaways

- Debugging production issues requires isolating infrastructure vs application problems
- Public endpoints are common, but must be protected with layered controls
- IAM roles prevent credential exposure even when APIs are public
- Documenting failures improves system understanding and future design decisions
