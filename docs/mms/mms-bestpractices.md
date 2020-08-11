---
title: Sinch MMS Best Practices
---

## Best Practices for Sending Static MMS Images

### Recommended File Size for Static MMS Images

No Transcoding: We recommend keeping static MMS images files under 740Kb.
Sinch Trancoding: We recommend keeping static MMS images files under 1Mb.

### Recommended Images Dimension for Static MMS Images

The optimal image size across all devices has an aspect ratio of 9:16.
We recommend __640px__ by __1138px__ or __1080px__ by __1920px__.

### Image Design Considerations for Static MMS Images

When an MMS image is delivered to a phone there are typically two ways the content is displayed. The content is displayed in the message conversation thread within a message bubble and then when the image bubble is tapped, the content is displayed in a full screen view. When when the image is displayed within a message bubble, there are portions of the image cut off from view. It is therefore important that any image be designed with the “content of interest” within a more restricted areas of the image so that it is readable from within the conversation view.

For Images __640px__ by __1138px__: Avoid adding content of interest on the top 129px and the bottom 89px.
For Images __1080px__ by __1920px__: Avoid adding content of interest on the top 219px and the bottom 150px

### Additional best practices for Static MMS Images

* Image files in a .jpg format tend to render faster and are supported on more devices than .png files.

* Do not use transparency in your image file. Transparent sections of an image may be rendered, white, black or in the message bubble color. There is no consistency behavior between device client versions.

## Best Practices for Sending an Animated Gif Images

An animated gif is a .gif image file composed of multiple images that play on a loop. Unlike videos, animated gifs files do not include sound and do not require the viewer to press play.

### Recommended File Size Animated Gifs

No Transcoding: We recommend keeping animated MMS image files under 600 KB.
Sinch Trancoding: We recommend keeping animated MMS images files under 740KB

*When transcoding an animated gif, we will reduce the dimensions of the animated gif by 25% and 50%. We will also apply compression to the animated gif. The results of this compression will vary based on the number of colors and compression you have already applied to your source animated gif.*

### Recommended File Dimensions Animated Gifs

__Recommended Dimensions__ (Portrait/Vertical)
__480px__ wide by __720px__ tall (a little animation)
__480px__ wide by __640px__ tall (a lot of animation)

__Recommended Dimensions__ (Square)
__640px__ wide by __640px__ tall (a little animation)
__480px__ wide by __480px__ tall (a lot of animation)

*Animated GIFs are displayed too large on iOS v13 – 13.4 unless the width is 320px or less. This issue is fixed in 13.5*

### Image Design Considerations for Static MMS Images

__480px__ by __720px__: Avoid placing content of interest in the 39px at the top, right, and bottom of the gif. Also avoid the adding such content in the 44px on the left side of the gif.

__480px__ by __640px__: Avoid placing content of interest in the 65px at the top, and the 58px at the bottom of the gif. Also avoid the 39px to each side of the gif.

__640px__ by __640px__: Avoid placing content of interest in the 52px at the top, right, and bottom of the gif. Also avoid the adding content in the 58px on the left side of the gif.

__480px__ by __480px__: Avoid placing content of interest in the 39px at the top, right, and bottom of the gif. Also avoid the adding such content in the 43x on the left side of the gif.

### Additional best practices for Animated MMS Images

* Keep the animation short and repeat it.
* Lower your Frames Per Second (FPS). A higher frame speed means that you are adding more content to each second which will increase the file size. While a video may have 24-30 FPS, we recommend keeping animated gifs in the 2-4 range depending on the length of the content.
* Limit the number of colors used in your file.
* Animated images should always be saved and uploaded as .gif files.
* Do not include any transparent aspects in your file. Transparency will be filled in with another color by the phone.
* We recommend that before sending this to your list, you send a test of your MMS to yourself to ensure high image quality.

## MMS Best Practices for Sending Videos

### Recommended File Size of MMS Videos

No Transcoding: We recommend keeping MMS video files under 740Kb.
Sinch Transcoding:  We recommend keeping MMS video files under 10MB. This will result in Video files from 300Kb to 1.5Mb.

### Recommended Video Dimensions for MMS Videos

__Landscape Video Format__ (Horizontal)
__1280px__ by __720px__

__Portrait Video Format__ (Vertical)
__640px__ by __1138px__

### Additional best practices for MMS Videos

__Duration__:
Maximum duration for HD quality video: 20 seconds. 
Maximum duration for high deliverability: 30 seconds.

__Frame Rate__:  30 FPS.

### Further Advice for Videos

You can improve the quality of your video by using the following techniques:

* Use spoken audio instead of music.
* Use slower movements in the video.
* Use solid colors.
* Video source files are best in .mp4 or .mov formats.
* Different carriers have different requirements when it comes to the file size of a video. Carriers may decrease the size of the video if it is too large. This could affect the quality that is provided.
