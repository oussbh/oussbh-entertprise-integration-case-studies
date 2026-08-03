# Large File Processing Investigation

## Context

An enterprise integration platform processed transaction files received from external systems.

Historically, file sizes remained below 1 GB.

The client introduced files exceeding 15 GB.

## Initial Symptoms

- Process freezing
- OutOfMemory-like behavior
- No explicit exception generated
- Inconsistent processing results

## Investigation

The first assumption focused on JVM memory allocation.

Heap sizing and JVM configuration were reviewed.

Further investigation revealed a charset parameter configured on the source endpoint.

The endpoint decoded incoming file streams before processing.

## Root Cause

The charset configuration caused excessive memory consumption when processing very large files.

The process stalled before normal exception handling could occur.

## Resolution

- Removed unnecessary charset decoding
- Relied on file header encoding information
- Validated in development environment
- Validated in test environment
- Prepared deployment for UAT

## Lessons Learned

Configuration settings that are harmless for small volumes can become critical bottlenecks when data size increases significantly.
