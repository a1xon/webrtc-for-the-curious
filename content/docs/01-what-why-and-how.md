---
title: What, Why and How
type: docs
weight: 2
---


## What is WebRTC.

WebRTC is both an API and Protocol. The WebRTC protocol is a set of rules for two agents to negotiate bi-directional secure communication. The WebRTC API was designed just for Javascript. This Javascript API then allows web developers to use the WebRTC protocol in the browser.

A similar relationship would be HTTP and the fetch API. WebRTC the protocol would be HTTP, and WebRTC the API would be the fetch API.

Many other APIs besides Javascript, servers and tools exist for WebRTC. All of these implementations can interact with each others.

## Why should I learn WebRTC?

These are the things that WebRTC will give you. This list is not exhaustive but is some of the things you may appreciate during your journey. Don't worry if you don't know some of these terms yet, this book will teach you them along tne way.

* Open Standard
* Multiple Implementations
* Available in Browsers
* Mandatory Encryption
* NAT Traversal
* Repurposed existing technology
* Congestion Control
* Sub-Second Latency

## How does WebRTC (the protocol) Work

This is a question that takes an entire book to explain. However, to start off we break it into four steps.

* Signaling
* Connecting
* Securing
* Communicating

These four steps happen sequentially. The prior step must be 100% successful for the subsequent one to even begin. At a high level this is what each one of these steps is accomplishing.

One pecuilar fact about WebRTC is that it actually made up of many other protocols! To make WebRTC we stitch together many existing technologies. In that sense WebRTC is more a combination and configuration of well understood tech that has been around since the early 2000s.

Each of these steps have dedicated chapters, but it is helpful to understand them at a high level first. Since they are dependant on each other it will help explain each steps purpose more.

### Signaling

When a WebRTC Agent starts it has no idea who it is going to communicate with and what they are going to communicate about. Signaling solves this issue! Signaling is used to bootstrap the call so that the two WebRTC agents can start communicating directly.

Signaling uses an existing protocol SDP. SDP is a plain text buffer made up off key/value pairs and contains a list of 'media sections'. The SDP that the two WebRTC Agents exchange contains details like.

* IPs and Ports that the agent is reachable on (candidates)
* How many audio and video tracks the agent wishes to send
* What audio and video codecs the agent supports
* Values used while connecting (uFrag/uPwd)
* Values used while securing (certificate fingerprint)

### Connecting

The two WebRTC Agents now know enough details to attempt to connect to each other. WebRTC again uses an existing technology called ICE.

ICE (Interactive Connectivity Establishment) is a protocol that pre-dates WebRTC. ICE allows the establishment of a connection between two Agents. These Agents could be in the same network, or on the other side of the world. ICE is the solution to establishing a direct connection without a central server.

The real magic here is 'NAT Traversal' and TURN Servers. These two concepts are all you need to communicate with an IP/Port in another subnet. This is where STUN and TURN come into play. We will explore these topics in depth later.

Once ICE successfully connects, WebRTC then moves on to establishing an encrypted transport. This transport is used for the audio, video and data.


### Securing

Now that we have bi-directional communication (via ICE) we need to establish secure communication. This is done through two protocols that pre-date WebRTC. The first protocol is DTLS (Datagram Transport Layer Security) which is just TLS over UDP. TLS is the technology that powers HTTPS. The second protocol is SRTP (Secure Real-time Transport Protocol).

First WebRTC connects by doing a DTLS handshake over the connection established by ICE. Unlike HTTPS WebRTC doesnt use a central authority for certificates. Instead WebRTC just asserts that the certificate exchanged via DTLS matches the the fingerprint shared via signaling. This DTLS connection is then used for DataChannel messages.

WebRTC then uses a different protocol for audio/video transmission called RTP. We secure our RTP packets using SRTP. We initialize our SRTP session by extracting the keys from the negotiated DTLS session. In a later chapter we discuss why media transmission has its own protocol.

We are done! You now have bi-directional and secure communication. If you have a stable connection between your WebRTC Agents this is all the complexity you may need. Unfortunately the real world has packet loss and bandwidth limits, and the next section is about how we deal with them.

### Communicating

We now have two WebRTC Agents with secure bi-directional communication. Lets start communicating! Again we use two pre-existing protocol RTP (Real-time Transport Protocol) and SCTP (Stream Control Transmission Protocol). 

RTP is used to carry media, and is encrypted using SRTP. The RTP protocol is quite minimal, but gives us what we need to implement real-time streaming. The important thing is that RTP gives flexibility to the developer so they can handle latency, loss and congestion as they please. We will discuss this further in the media chapter!

The final protocol in the stack is SCTP. SCTP is used to send DataChannel messages and those messages are encrypted with DTLS. SCTP allows many different delivery options for messages. You can optionally choose to have unreliable, out of order delivery so you can get the latency needed for real-time systems.


## WebRTC, a collection of protocols

**TODO a diagram of protocols working together**

WebRTC solves a lot of problems. At first this may even seem over-engineered. The genius of WebRTC is really the humility. It didn't assume it could solve everything better. Instead it embraced many existing single purpose technologies and bundled them together.

This allows us to examine and learn each part individually without being overwhelmed.

## How does WebRTC (the API) work


This section shows how the Javascript API maps to the protcol. If you aren't familiar with either that is ok. This is a fun section to return to as you learn more!


### new PeerConnection
### addTrack
### createDataChannel
### createOffer
### setLocalDescription
### setRemoteDescription
### ontrack
### oniceconnectionstatechange
### onstatechange

