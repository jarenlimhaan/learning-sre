## Traces

![traces](../images/traces.png)

This slide is about traces and spans.

### Core concepts
- Trace: The complete story of one request. It has a unique trace ID that follows the request everywhere it goes.
- Span: A single operation within a trace. Each service handling the request creates one or more spans.

### Anatomy of a span

#### Span elements

Information provided:
- Name (for example, "GET /api/users")
- The complete path the request took
- Start and end timestamps
- How long each operation took
- Attributes (key-value pairs, context)
- Parallel vs. sequential operations
- Parent span ID (relationships)
- Where errors occurred