## Custom Log Format ##

### Overview ###

If you want to use Gource to visualize something other than the supported log formats, there is a pipe ('|') delimited custom log format option:

  * **timestamp** - A [unix timestamp](http://en.wikipedia.org/wiki/Unix_time) of when the update occured.
  * **username**  - The name of the user who made the update.
  * **type**      - initial for the update type - (**A**)dded, (**M**)odified or (**D**)eleted.
  * **file**      - Path of the file updated.
  * **colour**    - A colour for the file in hex (FFFFFF) format. _Optional_.

This format is detected by Gource, so you don't need any extra command line options to use it:

```
gource custom.log
```

### Custom Log Example ###

The user _andrew_ adding the file _src/main.cpp_:

```
1275543595|andrew|A|src/main.cpp
```

Adding the same file coloured red (FF0000):

```
1275543595|andrew|A|src/main.cpp|FF0000
```

### Streaming Custom Log Format via STDIN ###

If you want to stream live data into Gource, a good way to do this is to make a script that feeds custom log format entries into Gource via STDIN:

```
my-custom-log-script.pl | gource --realtime --log-format custom -
```

Assuming 'my-custom-log-script.pl' periodically prints custom log format lines to STDOUT. The '-' at the end of the Gource command tells it to expect data on STDIN.

--realtime tells Gource to increment the clock in real time.

**NOTE:** This examples assumes Gource 0.27 or later (STDIN support was buggy in earlier versions).

## Custom Log Scripts ##

### PacmanLog2Gource ###

A script to visualize the activity of the Arch Linux package manager, [pacman](https://wiki.archlinux.org/index.php/Pacman):

https://bbs.archlinux.org/viewtopic.php?id=138928

### Bugzilla ###

[Martin](http://code.google.com/u/@VhhSSl1WAxlCVwh5/) contributes this query to generate a Gource custom log from a Bugzilla database:

```
SELECT
        REPLACE(
                CONCAT(date_changed, '|', login_name, '|', x, '|', description),
        ' ', '_')
FROM (
        SELECT
                UNIX_TIMESTAMP(a.bug_when) AS date_changed,
                p.login_name AS login_name,
                CASE
                        WHEN (CHAR_LENGTH(added) > 0 AND CHAR_LENGTH(removed) = 0) THEN 'A'
                        WHEN (CHAR_LENGTH(added) = 0 AND CHAR_LENGTH(removed) > 0)THEN 'D'
                        WHEN (CHAR_LENGTH(added) > 0 AND CHAR_LENGTH(removed) > 0) THEN 'M'
                END AS x,
                CONCAT(cl.name, '/', pr.name, '/', c.name, '/', b.bug_id, ' - ', b.short_desc) AS description
        FROM
                bugs_activity a
                LEFT JOIN
                profiles p
                        ON a.who = p.userid
                LEFT JOIN
                bugs b
                        ON a.bug_id = b.bug_id
                LEFT JOIN
                products pr
                        ON b.product_id = pr.id
                LEFT JOIN
                components c
                        ON b.component_id = c.id
                LEFT JOIN
                classifications cl
                        ON pr.classification_id = cl.id
        ORDER BY a.bug_when ASC
)
```

### DokuWiki ###

_WolverineX02_ has created a script to display [DokuWiki](http://www.dokuwiki.org) change history with Gource which you can get here:

http://www.dokuwiki.org/tips:gource_analysis

### Perforce ###

Gource doesn't include any built in support for Perforce currently. Olaf has created this one shell one liner to convert Perforce logs to Gource custom log format:

```
p4 changes ...|awk '{print $2}'|p4 -x - describe -s|awk '(/^Change / || /^... /) {if ($1 == "Change") {u=substr($4,1,index($4,"@")-1); t = $(NF-1) " " $NF; gsub("/"," ",t); gsub(":"," ",t);time=mktime(t);} else {if ($NF=="add") {c="A";} else if ($NF=="delete") {c="D";} else {c="M";};f=substr($2,3,index($2,"#")-3);print time "|" u "|" c "|" f;}}'|sort -n
```