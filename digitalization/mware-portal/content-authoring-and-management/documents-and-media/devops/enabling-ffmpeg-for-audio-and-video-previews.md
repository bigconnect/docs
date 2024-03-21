# Enabling FFmpeg for Audio and Video Previews

Documents and Media provides integration with the FFmpeg multimedia framework for generating audio and video file previews. To use this integration, you must first install FFmpeg on your server. If you’re working in a clustered environment, FFmpeg must be installed on each node.

NOTE

Previously, the portal used Xuggler to generate video and audio previews. However, the Xuggler library is currently unmaintained. As of version 7.4, users are advised to use FFmpeg for generating previews.

Once installed, follow these steps to configure Documents and Media to use FFmpeg:

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)), and go to _Control Panel_ → _System Settings_ → _Documents and Media_.
2. Go to _FFMPEG Audio Converter_, check _Enable_, and click _Update_.
3.  Go to _FFMPEG Video Converter_, check _Enable_, and click _Update_.

    ![Enable both FFMPEG Audio Converter and FFMPEG Video Converter.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/enabling-ffmpeg-for-audio-and-video-previews/images/01.png)

    If FFmpeg is installed correctly on your system, the portal displays a success message after clicking Update. Otherwise, the portal displays an error message.

    ![MWARE Portal displays an error message if FFmpeg is not installed.](https://resources.learn.liferay.com/images/dxp/latest/en/content-authoring-and-management/documents-and-media/devops/enabling-ffmpeg-for-audio-and-video-previews/images/02.png)

Once configured, audio and video previews are automatically generated for audio and video files at upload or update.

\
