---
layout : default
title : QUIC Discovery in chrome eng
parent: QUIC
grand_parent : OpenSource
---

[link](https://docs.google.com/document/d/1i4m7DbrWGgXafHxwl8SwIusY2ELUe8WX258xt2LFxPM/edit#)

QUIC Discovery
Ryan Hamilton <rch@chromium.org>
2014-10-28

First Connection

When Chrome makes a request to an origin it has never spoken to before, it does not know that the origin supports QUIC and so it will send the first request over TCP. The server’s response will indicate that it also supports QUIC via the Alt-Svc HTTP response header. (For example, the response. ”Alt-Svc: 443:quic” tells Chrome that the origin supports QUIC on port 443.). Now that Chrome knows the origin (scheme, host, port) speaks QUIC, it can attempt to use QUIC for the next request. When the request is made, Chrome will race a QUIC connect job with a TCP connect job. (These jobs establish a connection but do not issue the request.) Since the first request went out over TCP, the TCP job will win the race almost immediately and the second request will go out over TCP. At some subsequent point, the QUIC connection will succeed. Once this happens, all future requests will be sent over the QUIC connection.

Subsequent Connections

Chrome persists the knowledge that an origin supports QUIC, as well as the server config which enables QUIC’s 0-RTT handshake. When Chrome makes a request to an origin that it has previously spoken QUIC to, it will race a TCP job with a QUIC job. Since Chrome will be able to perform a 0-RTT handshake, the QUIC job will immediately win and the request will be issued on the QUIC connection. 


Broken Connections

In the event that a QUIC handshake fails (for example if UDP is blocked, or if the server does not speak a compatible version of QUIC) then Chrome will mark QUIC as broken for that host. Any requests which were in-flight will be re-issued over TCP. While QUIC is marked as broken for a host, no QUIC connections will be attempted. After 5 minutes, the broken entry will be expired QUIC will be marked as “recently broken” for that origin. When the next request is issued to the origin, Chrome will race a QUIC job with a TCP job. Because QUIC was “recently broken” the 0-RTT handshake will be disabled. If the handshake fails again, QUIC will be marked as broken for that origin again this time for 10 minutes, and the process repeats marking QUIC as broken for twice as long as the previous period. If the handshake succeeds, the request will be sent over QUIC and QUIC will no longer be marked as “recently broken”
