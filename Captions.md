## Captions ##

### Overview ###

Gource can display captions along the timeline by specifying a caption file
(using **--caption-file**) in the pipe ('|') delimited format below:

  * **timestamp** - A unix timestamp of when to display the caption.
  * **caption**   - The caption to appear.

### Example ###

```
1275543595|John joins the project
1327553595|Version 1.0 released
```

### Other Options ###

  * **caption-size** SIZE        - Font size of captions.
  * **caption-colour** FFFFFF    - Caption colours in hex (eg FF0000).
  * **caption-duration** SECONDS - How long each captions appears for in seconds.
  * **caption-offset** PIXELS    - Horizontal offset of captions (0=centered).