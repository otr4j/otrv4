# Integration

## Integrating OTRv4

- local and remote client IDs must be the _unique identifier known/used by both clients_ (part of ring signatures, needed for security)
- `FIXME limitations due to chosen goal: embedding, independent of protocol`

## Drawbacks of OTR version 2, 3

- `FIXME extra symmetric key not further defined, so not useful`
- `FIXME different protocols, different mime-types, different assumptions`

## Known problems

There are known problems with earlier versions of Off-the-Record (OTR). Most of these issues are rooted in deviations of implementations and initial bugs in implementations. These are known issues/critiques and their reasons.

```FIXME
- "compatibility issues"
- "embedded HTML parser / stripping HTML tags"
- encoding of the content
- aligning use of _extra symmetric key_
- "unreliable" (incorrectly displaying current state, such as "showing as insecure while secure")
```

## Solutions

```FIXME
- Capability Negotiation (CAPNEG)
  - supporting clients can negotiate/align handling resources
    - mime-type for content payload
```

## TODO OTRv4 drawbacks

- the use of two public keys during messaging (required for out-of-order messaging) makes messages significantly longer. Protocols like IRC are impacted as these have limited message sizes, therefore requires fragmentation to work.