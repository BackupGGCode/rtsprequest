# RTSPrequest
A simple utility for generating a series of [RTSP](http://en.wikipedia.org/wiki/RTSP) requests of a media server. The C++ application uses [cURL](http://curl.haxx.se/) to transmit the RTSP requests and receive the responses. It can serve as an RTSP client side example that works with Sony's Network Cameras.

The following sequence of RTSP requests are generated:
  1. Request OPTIONS supported by the media server.
  1. Request media server to DESCRIBE a particular media stream.
> The session description in the response is written to an SDP file.
  1. Request the media server to SETUP a particular media stream by specifying the transport protocol.
  1. Request the media server to PLAY the select media stream.
  1. Request TEARDOWN of the current session.

Example console output:
```

Usage:   rtsprequest.exe url [-t transport] [-r range]
           [-norange] [-noteardown] [-noninteractive] [-v]
         url of video server
         transport (optional) specifier for media stream protocol
           default transport: RTP/AVP;unicast;client_port=1234-1235
         range (optional) specifier for playing media stream
           default range: 0.000-
Example: rtsprequest.exe rtsp://192.168.0.2/media/video1


RTSP request V1.2
    Project web site: http://code.google.com/p/rtsprequest/
    Requires cURL V7.20 or greater

    cURL V7.24.0 loaded

RTSP: OPTIONS rtsp://192.168.0.2/media/video1
RTSP/1.0 200 OK
CSeq: 1
Date: Wed, Mar 18 2009 05:51:46 GMT
Public: OPTIONS, DESCRIBE, SETUP, TEARDOWN, PLAY, SET_PARAMETER, GET_PARAMETER


RTSP: DESCRIBE rtsp://192.168.0.2/media/video1
Writing SDP to 'video1.sdp'
RTSP/1.0 200 OK
CSeq: 2
Date: Wed, Mar 18 2009 05:51:46 GMT
Content-Base: rtsp://192.168.0.2/video1/
Content-Type: application/sdp
Content-Length: 463


RTSP: SETUP rtsp://192.168.0.2/media/video1/video
      TRANSPORT RTP/AVP;unicast;client_port=1234-1235
RTSP/1.0 200 OK
CSeq: 3
Date: Wed, Mar 18 2009 05:51:46 GMT
Transport: RTP/AVP;unicast;destination=192.168.0.1;source=192.168.0.2;client_port=1234-1235;server_port=6000-6001
Session: 13


RTSP: PLAY rtsp://192.168.0.2/media/video1/
RTSP/1.0 200 OK
CSeq: 4
Date: Wed, Mar 18 2009 05:51:46 GMT
Session: 13
Content-Length: 0

Playing video, press any key to stop ...

RTSP: TEARDOWN rtsp://192.168.0.2/media/video1/
RTSP/1.0 200 OK
CSeq: 5
Date: Wed, Mar 18 2009 05:51:49 GMT
```
