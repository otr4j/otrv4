# Integration

OTR is a protocol that is designed to function on top of existing infrastructure. This document describes some considerations. This is motivated in part because of criticism on earlier versions of the protocol, in the hopes of relieving some of the confusion and frustration.

## Integrating OTRv4

When integration OTRv4 support in any chat application, there are a few things to realize.

1.  __OTR, as a protocol, cannot offer perfectly finely-tuned integration in every circumstance.__  
    OTRv4, as well as older versions, are designed to work _on top of_ a large range of possible (chat) transports. Therefore, it is designed with that thought in mind. Given that there are many chat protocols and each works slightly different, using OTR may leave some rough edges. The developer that integrates OTR into the chat app (e.g. as a plug-in) might need to fine-tune some rough edges. There are a few cases, discussed below, that highlight some of these circumstances.

1.  __The chat application must expose a unique identifier for contacts that both the local and the remote side of the conversation will know.__  
    OTRv4 uses ring-signatures to verify (bi-directionally) that both clients know which other party they are interacting with. Therefore, the local and remote client's account ID is included as part of this process. If there are multiple IDs, short-hands, volatile components, et cetera available to describe a contact with, care must be taken that only a obvious (reliable) "universally unique" contact ID is used, such that for any chat protocol, any two parties will choose the same unique contact ID.  
    For example in case of IRC, a full identity string containts nick, identity, hostname. Only the `nick` is reliable for identifying a contact. Arguably, that's a very bad choice as the nick can change at any moment, and you are right. However, here we are limited to design of the protocol. Presumably, regulars will have registered and protected their nick, so that they're guaranteed to have the nick whenever they connect.

1.  `FIXME OTRv4 includes public keys, making messages quite a bit larger. This is noticable on IRC in that fragmentation must be used to make it work.`

1.  `FIXME limitations due to chosen goal: embedding, independent of protocol`

## Drawbacks of OTR version 2, 3

- `FIXME extra symmetric key not further defined, so not useful`
- `FIXME different protocols, different mime-types, different assumptions`
- `OTR version 3 tackled situation where multiple clients are active on one account`
- 

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