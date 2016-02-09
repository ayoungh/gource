## Recording Videos ##

### Video Capture Software ###

By far the easiest way to record a video of Gource is using a third party video capture program. The only downside of this approach is you will have less control over the final encoding quality of the video and you may need to manually start and stop recording.

I recommend these programs:

  * [Open Broadcaster Software](http://obsproject.com) (Windows, support for other operating systems in development).
  * [Fraps](http://www.fraps.com) (Windows).

### Raw Video Output and Encoding ###

You can capture raw video from Gource using the `-o` (aka `--output-ppm-stream`) option. This creates an uncompressed sequence of screenshots in PPM format which can then be processed by a video encoder (such as `ffmpeg`) to produce a video.

You can also adjust the output frame rate with `-r` (aka `--output-framerate`). Note you will need to adjust the frame rate used by the encoder (eg `ffmpeg -r`) to match, unless you are going for a speed-up timelapse or slow motion effect.

Make sure you keep the window of Gource entirely visible while recording. If you minimize or partially obscure the window, your video card may decide not draw the output consistently and you will get artifacts in your recording. You may like to use the `-f` option to run in full-screen mode to stop this happening.

You may want to record your video in widescreen as that is the trend for online video now days. You can record in 720p by adding `-1280x720` to the command line.

Also check out the [Controls](http://code.google.com/p/gource/wiki/Controls) section of the wiki for steps to customize the look of Gource for your videos.

### Linux / Mac ###

## FFmpeg using x264 codec ##

The below command lines will create a video at 60fps using the [x264](http://en.wikipedia.org/wiki/X264) codec in an mp4 container.

This assumes you have recent version of ffmpeg **with libx264 support**. Some distributions like Ubuntu do not enable libx264 support by default in ffmpeg (See [this thread](http://ubuntuforums.org/showthread.php?t=1117283) for various ways to [enable libx264 support on Ubuntu](http://ubuntuforums.org/showthread.php?t=1117283)). Consult guides for your distribution if this is the case.

In one command (raw video piped directly into ffmpeg):

```
gource -1280x720 -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset ultrafast -pix_fmt yuv420p -crf 1 -threads 0 -bf 0 gource.mp4
```

As two commands:

```
gource -1280x720 -o gource.ppm
ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i gource.ppm -vcodec libx264 -preset ultrafast -pix_fmt yuv420p -crf 1 -threads 0 -bf 0 gource.mp4
```

This one has the advantage of keeping the raw video file (gource.ppm) so you can go back and tweak the encoding parameters if you don't like the output quality or need a different video format without recording the video again. The disadvantage is that this raw uncompressed video and you can expect it to use up 100gs of gigs, so keep that in mind!

You can interchange the extension 'mp4' with 'mkv' or 'avi' to get ffmpeg to use a different container format for the video. 'avi' is typically discouraged for long videos as there is a 2GB file limit associated with that format.

**Note**: **ffmpeg seems to be a constantly moving target**, so you may find you need slightly different arguments with your version. There is a good guide to using x264 with ffmpeg [here](https://wiki.archlinux.org/index.php/FFmpeg).

## FFmpeg using WebM codec ##

A good alternative codec to x264 is [WebM](http://en.wikipedia.org/wiki/WebM). Below is a very basic command line example of using ffmpeg to produce a Gource WebM video:

```
gource -1280x720 -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libvpx -b 10000K gource.webm
```

### Windows ###

Here is an example command-line using the built in PPM frame capture support and ffmpeg to generate a video:

```
gource -1280x720 -o gource.ppm C:\\path\\to\\code\\repository
C:\\ffmpeg\\bin\\ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i gource.ppm -vcodec libx264 -preset ultrafast -pix_fmt yuv420p -crf 1 -threads 0 -bf 0 gource.x264.avi
```

You can interchange the extension 'avi' with 'mp4' or 'mkv' to get ffmpeg to use a different container format for the video. 'avi' is typically discouraged for long videos as there is a 2GB file limit associated with that format.

You can get an `ffmpeg` build for Windows from [here](http://ffmpeg.zeranoe.com/).

**Note**: **ffmpeg seems to be a constantly moving target**, so you may find you need slightly different arguments with your version. There is a good guide to using x264 with ffmpeg [here](https://wiki.archlinux.org/index.php/FFmpeg).

## Videos ##

Video showing off the new Bloom effect:

<a href='http://www.youtube.com/watch?feature=player_embedded&v=NjUuAuBcoqs' target='_blank'><img src='http://img.youtube.com/vi/NjUuAuBcoqs/0.jpg' width='425' height=344 /></a>

A collage of different open source software projects displayed using Gource:

<a href='http://www.youtube.com/watch?feature=player_embedded&v=E5xPMW5fg48' target='_blank'><img src='http://img.youtube.com/vi/E5xPMW5fg48/0.jpg' width='425' height=344 /></a>

## Examples ##

Various videos of projects created with Gource.

### Minecraft ###

<a href='http://www.youtube.com/watch?feature=player_embedded&v=zRjTyRly5WA' target='_blank'><img src='http://img.youtube.com/vi/zRjTyRly5WA/0.jpg' width='425' height=344 /></a>

### Torchlight II ###

<a href='http://www.youtube.com/watch?feature=player_embedded&v=byNjZvZfvAM' target='_blank'><img src='http://img.youtube.com/vi/byNjZvZfvAM/0.jpg' width='425' height=344 /></a>

### Trade Me ###

<a href='http://www.youtube.com/watch?feature=player_embedded&v=34j9SvtYShQ' target='_blank'><img src='http://img.youtube.com/vi/34j9SvtYShQ/0.jpg' width='425' height=344 /></a>

### Koha Library System ###

<a href='http://www.youtube.com/watch?feature=player_embedded&v=Tl1a2VN_pec' target='_blank'><img src='http://img.youtube.com/vi/Tl1a2VN_pec/0.jpg' width='425' height=344 /></a>

### OpenOffice ###

<a href='http://www.youtube.com/watch?feature=player_embedded&v=a-gAoYapM8U' target='_blank'><img src='http://img.youtube.com/vi/a-gAoYapM8U/0.jpg' width='425' height=344 /></a>

### PHP ###

<a href='http://www.youtube.com/watch?feature=player_embedded&v=jhbzEwxbCfI' target='_blank'><img src='http://img.youtube.com/vi/jhbzEwxbCfI/0.jpg' width='425' height=344 /></a>

### Homebrew ###

<a href='http://www.youtube.com/watch?feature=player_embedded&v=ZX0xCWANfW4' target='_blank'><img src='http://img.youtube.com/vi/ZX0xCWANfW4/0.jpg' width='425' height=344 /></a>

## Git ##

<a href='http://www.youtube.com/watch?feature=player_embedded&v=GTMC3g2Xy8c' target='_blank'><img src='http://img.youtube.com/vi/GTMC3g2Xy8c/0.jpg' width='425' height=344 /></a>