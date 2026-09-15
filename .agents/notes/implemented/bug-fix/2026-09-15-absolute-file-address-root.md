# Agent Note: Preserve the absolute POSIX root in file addresses

Status: implemented

English | [中文](2026-09-15-absolute-file-address-root.zh.md)

## Problem

`absoluteFileAddress('/')` produced `dsh-resource://file/absolute/`, but `parseFileAddress` rejected that address as missing a path. The helper therefore could not round-trip the valid absolute POSIX root.

## Decision

The parser treats the empty path after the `absolute/` scope as the POSIX root. The existing `absolute//` spelling remains reserved for a UNC path with an empty host and continues to be rejected.

## Alternatives considered

**Encode the root as a non-empty synthetic segment.** This would preserve the old rejection of `absolute/`, but would add a special wire spelling for a path whose existing serialization is already unambiguous for POSIX callers.

## Consequences

Absolute POSIX root addresses now round-trip, and consumers can distinguish them from malformed UNC addresses. The Session-less address still does not authorize a file read; consumers retain their existing workspace/session policy.
