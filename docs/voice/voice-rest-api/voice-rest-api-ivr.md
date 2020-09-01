---
title: Interactive Voice Response
excerpt: The Sinch platform supports playing audio recordings during a call.
next:
  pages:
    - voice-sip-trunking
---

## External media support

Access to external media is supported by providing a valid URL in a SVAML action/instruction. Currently we support HTTP and HTTPS schemes. Please consider that when a media file is downloaded for the first time, some delays may be experienced by the users, which is directly affected by the file size and the upload speed of the file provider.

### Downloaded content caching

The platform will store a local copy of the downloaded media file after it successfully downloads it. This is to prevent constant traffic between Sinch and partner backend, as well as to decrease the time it takes for the file to be played during the call.

The following rules apply to caching:

- if a Cache-Control header is included in the HTTP/HTTPS response, the platform will use the max-age directive’s value to calculate how long the file will be cached for. The minimum supported value is 24 hours, i.e. smaller max-age value will result in the file being cached for the minimum value of 24 hours. The value provided is absolute and will expire after exactly the time specified after it was downloaded.
- if the response message does not contain a Cache-Control header, the file is cached for 24 hours since it was last accessed. Note that this is different from the previous case: The expiration time will be constantly extended after each usage of the media resource.

### Limits

Consider the following limitations when providing an external media reference:

- the file size is by default limited to 1MB
- the number of download attempts is limited to 50 downloads per hour
- the size of the total downloaded content is limited to 50MB per hour

If you want to play bigger files or need to extend the total download content length per hour, please contact Sinch support.

### Supported Content-Type headers

When the platform is accessing an external media resource, it requires a Content-Type header in the response. The following values are supported:

<table>
    <tr>
        <th>Content-Type</th>
        <th>File extensions</th>
    </tr>
    <tr>
        <td>audio/wav<br />audio/x-wav<br />audio/wave<br />audio/x-pn-wav</br>audio/adpcm</td>
        <td>.wav .wave</td>
    </tr>
    <tr>
        <td>audio/mpeg<br />audio/x-mpeg<br />audio/mp3<br />audio/x-mp3<br />audio/x-mpeg3<br />audio/mp4</td>
        <td>.mp1, .mp2, .mp3, .mp4, .mpa</td>
    </tr>
    <tr>
        <td>audio/aac<br />audio/x-aac</td>
        <td>.m4a, .m4b, .m4p, .m4v, .m4r, .aac, .3gp</td>
    </tr>
    <tr>
        <td>audio/aiff<br />audio/x-aiff<br />audio/aifc<br />audio/x-aifc</td>
        <td>.aiff, .aif, .aifc</td>
    </tr>
    <tr>
        <td>audio/gsm<br />audio/x-gsm</td>
        <td>gsm</td>
    </tr>
    <tr>
        <td>audio/ogg</td>
        <td>.ogg</td>
    </tr>
    <tr>
        <td>audio/opus</td>
        <td>.opus</td>
    </tr>
</table>
