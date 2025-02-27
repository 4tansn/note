# Resolving `NullPointerException` with OpenTelemetry and Couchbase SDK in Vert.x

## Problem

In a Vert.x-based application, when using **OpenTelemetry** for distributed tracing, we encountered a `NullPointerException` in the `Span.makeCurrent()` method. This issue occurred because the Couchbase SDK operates in its own threads, which are not Vert.x event loop threads. 

In Vert.x, OpenTelemetry requires tracing contexts to be executed on Vert.x threads (specifically event loop or worker threads). If a span is created or accessed from a non-Vert.x thread, such as one from an external library like Couchbase SDK, the context propagation may fail, resulting in a `NullPointerException`.

### Key Points:
- **Root Cause**: The Couchbase SDK returned data on a separate thread not managed by Vert.x, causing the OpenTelemetry context to be missing or null.
- **Result**: This led to a `NullPointerException` when trying to make a span active with `span.makeCurrent()`.

### Stack Trace
```plaintext
java.lang.NullPointerException
  at io.opentelemetry.context.Context.current() 
  at io.opentelemetry.api.trace.Span.makeCurrent()
