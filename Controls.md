**Note**: some options may require the latest version of Gource.

Check out the [Videos](http://code.google.com/p/gource/wiki/Videos) section for examples and instructions for creating your own videos with Gource.

## Display Settings ##

You can set the screen resolution with **`-WIDTHxHEIGHT`** and switch to full-screen mode with **`-f`**. To set the view to full-screen 720p:

```
gource -f -1280x720
```

For additional anti-aliasing use the --multi-sampling option.

## Camera Controls ##

### Camera Mode ###

Gource has two camera modes:

  * **overview** - keeps the entire repository in view.
  * **track** - follows the current active users or the selected user.

The default mode is 'overview'. To set the camera mode to 'track':

```
gource --camera-mode track
```

You can also toggle the mode in Gource with the **V** key and the **middle mouse button**. Pressing **Tab** cycles though the current visible users.

### Panning ###

Dragging the **left mouse button** on the background pans the camera. This will also disable automatic camera movement until you toggle the camera mode. You can also move the camera with the **arrow keys**.

### Rotation ###

As of 0.27, Gource will automatically rotate the view to make best use of the available screen space. If this is not to your liking, you can turn this off with --disable-auto-rotate.

You can manually rotate the view by dragging **right mouse button** on the background. This will also disable automatic camera rotation until you toggle the camera mode.

### Zoom ###

You can change the maximum zoom distance with the **mouse wheel**.

## Time-line / Chronology ##

### Seeking ###

You can seek to any point in the project using the time-line widget that appears along the bottom of the screen when the mouse passes over it. Mousing over the time-line shows the date at that position in the history file.

### Pause ###

Pause the action by pressing **space**. While paused, you can still move the camera around and inspect the details of files and users.

### Start / Stop Position ###

Use --start-position and --stop-position to specify the period of the project history to display. The argument is a fraction of the total length of the project history file:

```
gource --start-position 0.5 --stop-position 0.75
```

### Seconds Per Day ###

Seconds per day controls the amount of time Gource will take to display each day in the log file (10 seconds by default).

```
gource --seconds-per-day 1
```

As a convenience, the option **`--realtime`** sets seconds-per-day to 60\*60\*24.

### Auto Skip Seconds ###

Some projects may have gaps in activity in the span of days or weeks. To avoid the boring bits, Gource will advance to the next entry (after 3 seconds by default) if nothing is happening.

To change the auto skip seconds value:

```
gource --auto-skip-seconds 1
```

The sister command `--disable-auto-skip` may be used to turn this feature off:

```
gource --disable-auto-skip
```

### File Idle Time ###

By default, Gource 'expires' files which haven't been touched by a user in the 60 seconds. You can change the amount of time files remain without changing with:

```
--file-idle-time SECONDS
```

To turn expiring files off:

```
--file-idle-time 0
```

### File Lag ###

Gource doesn't actually apply updates immediately as they come up in the log, instead the user will move towards the area of the new files before adding them. This is easier to follow but it also means if you are using a small value for 'seconds-per-day', it can get quite far behind.

You can change the time limit for files to be added in seconds (default 5) with `--max-file-lag SECONDS`:

```
gource --max-file-lag 0.1
```

Above, files must appear within 0.1 seconds (100ms).


## Appearance ##

### Legend / File Extension Key ###

Pressing **K** or launching gource with --key enables a file extension key, showing the colours used for each file extension and the current number of files with that extension listed in descending order:

```
gource --key
```

### Bloom ###

Gource has some command line options to control the appearance of the 'light bloom' effect that appears around directories. You can affect the radius of the bloom using `--bloom-multiplier`, and the intensity using `--bloom-intensity`.

To get double the radius and double the normal intensity you would use:

```
gource --bloom-multiplier 2.0 --bloom-intensity 1.5
```

If you're sick of the bloom effect, you can turn it off with `--disable-bloom`.

### Elasticity ###

Elasticity is a fun if somewhat pointless effect that controls how 'rubbery' directory branches behave when they have a force applied to them:

```
gource -e 0.5
```

### Background ###

The background colour can be set with the --background option followed by a colour in hex format (like in HTML):

```
gource --background 555555
```

This will set the background colour to a darkish grey

You can set the alpha channel to be transparent with --transparent. This is useful if you want to take screenshots (**F12**) and manipulate them in image editing software. Note ppm output does not have an alpha channel so will not be affected.

To set a background image:

```
gource --background-image background.png
```

### Colours ###

Press **S** to cycle through alternate colours for files and users. You can also specify an alternate colour hashing 'seed' on the command line with --hash-seed.

### Date Format ###

Change the strftime format string Gource uses to display the current time (See strftime manual for [valid format strings](http://opengroup.org/onlinepubs/007908799/xsh/strftime.html)).

```
gource --date-format "%D"
```

### Fonts ###

You can change the default font size and colour of the timer and titles with:

```
gource --font-size 18 --font-colour FFFF00
```

### Logo ###

Set a logo to appear in the foreground:

```
gource --logo logo.png
```

The logo position can be changed with --logo-offset XxY.

### Title ###

Set a title to appear in the bottom left corner of the screen:

```
gource --title "My Project"
```

### Hiding Elements ###

Sometimes it's hard to see the forest through the trees. To cull the brush:

```
gource --hide bloom,date,dirnames,files,filenames,mouse,progress,tree,users,usernames
```

(your screen should now be blank).