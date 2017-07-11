## 金山云FFmpeg扩展

金山云对FFmpeg RTMP/FLV部分做了扩展，用于支持H.265。

针对《video_file_format_spec_v10_1》 VIDEODATA部分扩展如下：

### VIDEODATA
The VideoTagHeader contains video-specific metadata.
#### VideoTagHeader
| Field |Type |	Comment|
| :---: | :---| :---|
|Frame Type|UB [4]|Frame Type Type of video frame. The following values are defined: <br>1 = key frame (for AVC and HEVC, a seekable frame)<br>2 = inter frame (for AVC and HEVC, a non-seekable frame)<br>3 = disposable inter frame (H.263 only)<br>4 = generated key frame (reserved for server use only)<br>5 = video info/command frame|
|CodecID|UB [4]|Codec Identifier. The following values are defined:<br>2 = Sorenson H.263<br>3 = Screen video<br>4 = On2 VP6<br>5 = On2 VP6 with alpha channel<br>6 = Screen video version 2<br>7 = AVC<br>12=HEVC|
|HVCPacketType|	IF CodecID == 12<br> UI8|	The following values are defined:<br>0 = HEVC sequence header<br>1 = HEVC NALU<br>2 = HEVC end of sequence (lower level NALU sequence ender is not required or supported|
|CompositionTime|	IF CodecID==7 OR CodecID == 12 <br>SI24	|IF AVCPacketType == 1 OR HVCPacketType == 1<br>Composition time offset<br>ELSE<br>0<br>See ISO 14496-12, 8.15.3 for an explanation of composition times. The offset in an FLV file is always in milliseconds.|
|VideoTagBody	|IF FrameType == 5<br>  UI8<br>ELSE (<br>IF CodecID == 2<br>H263VIDEOPACKET<br>IF CodecID == 3<br>SCREENVIDEOPACKET<br>IF CodecID == 4<br>VP6FLVVIDEOPACKET<br>IF CodecID == 5<br>VP6FLVALPHAVIDEOPACKET<br>IF CodecID == 6<br>SCREENV2VIDEOPACKET<br>IF CodecID == 7 <br>AVCVIDEOPACKET<br>IF CodecID == 12 <br>HVCVIDEOPACKET<br>)|Video frame payload or frame info<br>If FrameType == 5, instead of a video payload, the Video Data Body contains a UI8 with the following meaning:<br>0 = Start of client-side seeking video frame sequence<br>1 = End of client-side seeking video frame sequence<br>For all but AVCVIDEOPACKET or HVCVIDEOPACKET, see the SWF File<br>Format Specification for details|

## 使用说明
目录flv265-Kingsoft/FFmpeg下已经将patch达到了origin/release/3.2等branch上。

当前已经支持的release分支包括：
* 2.8
* 3.0
* 3.1
* 3.2
* 3.3

## patch说明
patch位于flv265patch_from_kingsoft.7z压缩包内。


patch涉及的改动包括：
* libavform/flv.h
* libavform/flvenc.c
* libavform/flvdec.c
