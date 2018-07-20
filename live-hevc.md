# Live HEVC streaming with Azure Media Services (for SD, HD or UHD streaming)

In order to stream live HEVC content with Azure Media Services, you need the following :
- a compatible live encoder
- an Azure Media Services Account, with a live channel using fMP4 (Smooth) as ingest
- an HEVC compatible device 

## HEVC Live Encoders
To send a live stream to Azure Media Services, you must use an hardware HEVC live encoder which supports Smooth Streaming ingest protcol and the [Smooth Streaming protocol amendment for HEVC](https://docs.microsoft.com/en-us/azure/media-services/media-services-specifications-ms-sstr-amendment-hevc) 
The following encoder(s) have been tested successfuly:
- MediaExcel Hero 4K HEVC LIVE ENCODER

You can use a multi bitrate/resolution preset. All video bitrates must be GOP aligned and use the HEVC codec.

## Example of preset
The following preset has been successfuly tested with MediaExcel Hero encoder and Media Services :

| Resolution        | Bitrate           | Frame rate  |
| ------------- |:-------------:| -----:|
| UHD 2160p      | 16 mbps | 50 fps |
| HD 1080p      |  6 mbps      |   50 fps |
| HD 720p | 2.5 mbps      |    50 fps |
| 360p | 600 kbps      |    50 fps |

## Dynamic Range
Standard Dynamic Range (SDR) and High Dynamic Range (HDR) can be used. However, our testing shows that HDR is problematic for many devices. We recommend starting testing 4K/HEVC streaming with SDR.

## Media Services account and live channel
- Create a Media Services account,
- Create and a live channel using the fragmented MP4 (Smooth) protocol for the ingest,
- Create a program (called 'event' in the Portal) and publish it, using a streaming locator.

## HEVC Playback
You can test the program output URL using Windows 10, a recent iOS device or a Smart TV supporting DASH-HEVC.

### Dynamic Packaging and protocols
Dynamic packaging which is happening on the streaming endpoint will output HEVC for the following prococols:
- MPEG-DASH. Use (format=mpd-time-csf) in the URL
- CMAF. Use (format=m3u8-cmaf) in the URL

Urls will look like:

https://{AMS account name}-{region}.streaming.media.azure.net/{locator GUID}/{manifest name}.ism/manifest(format=mpd-time-csf)

https://{AMS account name}-{region}.streaming.media.azure.net/{locator GUID}/{manifest name}.ism/manifest(format=m3u8-cmaf)

If you use Azure media Player, you should pass the general streaming URL
https://{AMS account name}-{region}.streaming.media.azure.net/{locator GUID}/{manifest name}.ism/manifest
and force AMP to use DASH

Azure Media Player test page : https://ampdemo.azureedge.net/azuremediaplayer.html

You can use the CMAF url natively in the Safari browser on iOS, or in Microsoft Edge on Windows 10.

### Digital Rights Management (DRM)
You can configure Dynamic Encryption to deliver the HEVC stream with DRM.

The following protocols and encryption can be used with HEVC:
- DASH with Common Encryption (PlayReady and Widevine)
  - The Azure Media Services streaming URL should containe the syntax (format=mpd-time-csf,encryption=cenc)
  - You can play the stream with Windows 10 Edge and Azure Media Player, some SmartTVs, some Android TV boxes
- HLS using CMAF with FairPlay
  - The Azure Media Services streaming URL should containe the syntax (format=m3u8-cmaf,encryption=cbcs-aapl)
  - You can play the stream with iPhone 7 / iOS 11 and later, Apple TV and MacOS

Recommandation is to setup DRM using v2 or V3 Media Services API. You should setup:
- Common Encryption for DASH protocol
- FairPlay for DASH and HLS protocols

### Windows 10, Xbox One
You must install the HEVC Video Extensions from this [link](https://www.microsoft.com/en-us/store/p/hevc-video-extension/9n4wgh0z6vhq). Please note that your GPU must support HEVC decoding.

### iOS,  macOS, Apple TV
iOS 11 is required for HEVC decode. More details [here](http://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Apple-Embraces-HEVC-What-Does-it-Mean-for-Encoding-118735.aspx).

### Smart TV
Some smart TV supports HEVC-DASH streaming. Please contact the TV manufacturer for more information.