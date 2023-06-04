# Review

## 1. DAKE confusion

Invariants derived from the processes: ratcheting (rotating public keys), rotating chainkeys, sending/receiving messages.

1.  Perform ratchet `<==>` "first"
    - Ratchet with sender keys `<==>` send first message after receiving
    - Ratchet with receiver keys `<==>` receive first message after sending
1.  _For each participant_: Ratchet `i <==> (` _ECDH public key_`,` _DH public key_ `)`,  
    with: _DH public key_ is present `<==> i % 3 == 0`.
1.  DH ratchet every third brace key rotation. (`i % 3 == 0`)
1.  `i` of Alice and `i` of Bob are mutually dependent due to send/receive events triggering the ratchet. (increment in lock-step)
1.  `max_remote_i_seen` value can be derived from knowledge of local `i` (ratcheting state), given dependency of ratcheting to sending/receiving.
1.  Double Ratchet has period of `6` ratchets: 2 consecutive periods of 3, each with one DH ratchet ("every third brace-key"). Each period provides a single DH ratchet for each participant.
1.  `FIXME this needs checking, IIRC i-1 should be used.` When sending: `i` is sender ratchet ID, `i-1` is receiver ratchet ID. When receiving vice-versa.

Criticisms:

1.  It only adds complexity to describe two separate `i` values, one for each participant. As mentioned above, these are mutually dependent.
1.  No data in a data message is reliable or trustworthy before authentication (including `i`, _public keys_). There is a short period where this data needs to be handled without making lasting state changes.
1.  Resetting `i` to `0` seems to have no relevance in terms of functionality or security, as we already use `i % 3` where appropriate. The data-type is sufficiently large.
1.  `i`, _ECDH public keys_, _DH public keys_ are all equally untrustworthy before messages are authenticated and equally trustworthy afterwards (assuming no protocol violations). Therefore, no reason not to use incrementing `i` as indicator for ratchet and message keys, as key in `skipped_MKenc` store.
    - Even if `i` keeps incrementing, it is not an indicator of number of messages sent/received, any ratio between sent/received, or frequency.
1.  Resetting `i` necessitates use of _ECDH public key_ as identifier where necessary. However, the key does not offer any benefits.
1.  Incorrect assumption: if any (delayed) message with pair `(`_ECDH public key_, `j)` was received and does not have a key in the `skipped_MKenc` store, it must be duplicate.  
    This is not correct, as you cannot yet trust that the _ECDH public key_ is reliable, trustworthy. Therefore, it either is duplicate or malicious or corrupted. However, this is not a big problem as we can authenticate and then we either discover that _public key_ is correct and it is delayed/duplicate, or the message is corrupted/malicious and must be discarded. However, _ECDH public key_ is at best useful, at worst redundant information.

Proposal:

1.  Drop `max_remote_i_seen` variable. (No benefit, derivable, superfluous.)
1.  Drop resets of `i`. (No benefit, derivable when necessary with `i % 3`.)
1.  Have single notion of `i` for Alice and Bob, with understanding that semantics are swapped: any `i` that is Alice's sender ratchet, is Bob's receiver ratchet.
    - __Is there a risk in ever-incrementing `i` value?__
1.  Cannot conclude duplicate messages: for any duplicate message, the Message Keys (if even previously stored) would now be gone. Without ability to authenticate, message could be: duplicate, corrupted or malicious.
