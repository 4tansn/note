# Fixing Missing Traces Due to `00` Flag from Dynatrace

- **Issue**: Dynatrace was sending trace context with the `00` flag (no sampling), causing **missing traces** in our system.
- **Cause**: The `00` flag in the `traceparent` header disabled sampling and prevented traces from being recorded.
- **Solution**: We configured `SdkTracerProvider` with `Sampler.alwaysOn()` to **override the `00` flag** and enable trace collection.
- **Result**: Now, all traces are captured, ensuring complete trace visibility and fixing the missing trace issue.
