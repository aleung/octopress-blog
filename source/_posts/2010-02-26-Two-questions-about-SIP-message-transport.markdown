---
layout: post
title: "Two questions about SIP message transport"
comments: true
date: 2010-02-26 12:30
tags:
- SoftwareDev
- SIP
---
###  SIP messages are sent in mix of UDP and TCP

In our test we found that our client first sent SIP messages in UDP but later on it changed to TCP. Is it an issue?

In RFC 3261 (SIP) chapter 18.1.1 and 18.2.1, it says:

> If a request is within 200 bytes of the path MTU, or if it is larger than 1300 bytes and the path MTU is unknown, the request MUST be sent using an RFC 2914 congestion controlled transport protocol, such as TCP. ... This prevents fragmentation of messages over UDP and provides congestion control for larger messages.
> 
> For any port and interface that a server listens on for UDP, it MUST listen on that same port and interface for TCP. This is because a message may need to be sent using TCP, rather than UDP, if it is too large.

So, whether send a SIP message in UDP or TCP is decided bytransport layer. When the initial SIP message of a transaction is sent in UDP it's possible that the consequent messages are sent in TCP.

###  SIP MESSAGE is resent in 0.5 second

In another test case we found that the client resends the SIP MESSAGE request in 0.5 second if it doesn't receive the response. We know that SIP message should be resent in a specific time if no response is received. But why the interval is 0.5 second? Is it configurable in Sailfin?

Also go to RFC 3261, find the answer in chapter 17.1.1.2 and 17.1.2.2:

> If an unreliable transport is in use, the client transaction MUST set timer E to fire in T1 seconds. If timer E fires while still in this state, the timer is reset, but this time with a value of MIN(2*T1, T2). When the timer fires again, it is reset to a MIN(4*T1, T2). ... For the default values of T1 and T2, this results in intervals of 500 ms, 1 s, 2 s, 4 s, 4 s, 4 s, etc.
> 
> The default value for T1 is 500 ms. T1 is an estimate of the RTT between the client and server transactions. ... T1 MAY be chosen larger, and this is RECOMMENDED if it is known in advance (such as on high latency access links) that the RTT is larger.

In SailFin(GlassFish) the value of T1 and T2 can be configured in admin console under: Configuration - SIP Service - SIP Protocol.
