## Using Gravatars with Gource ##

[Gravar](http://www.gravatar.com/) is a database of user submitted avatar images each associated with an email address.

Gource can be told to look for user images in a directory using the --user-image-dir argument. Using the email address associated with the author of each commit we can try and find images for the contributors of a project.

### Example Perl Script ###

This perl script when run in your Git project directory will try and fetch Gravar's for each commit author and store them under the folder .git/avatar/ as 'Username.png':

```
#!/usr/bin/perl
#fetch Gravatars

use strict;
use warnings;

use LWP::Simple;
use Digest::MD5 qw(md5_hex);

my $size       = 90;
my $output_dir = '.git/avatar';

die("no .git/ directory found in current path\n") unless -d '.git';

mkdir($output_dir) unless -d $output_dir;

open(GITLOG, q/git log --pretty=format:"%ae|%an" |/) or die("failed to read git-log: $!\n");

my %processed_authors;

while(<GITLOG>) {
    chomp;
    my($email, $author) = split(/\|/, $_);

    next if $processed_authors{$author}++;

    my $author_image_file = $output_dir . '/' . $author . '.png';

    #skip images we have
    next if -e $author_image_file;

    #try and fetch image

    my $grav_url = "http://www.gravatar.com/avatar/".md5_hex(lc $email)."?d=404&size=".$size; 

    warn "fetching image for '$author' $email ($grav_url)...\n";

    my $rc = getstore($grav_url, $author_image_file);

    sleep(1);

    if($rc != 200) {
        unlink($author_image_file);
        next;
    }
}

close GITLOG;
```

Having done this you can use this command line to use them with Gource:

```
gource --user-image-dir .git/avatar/
```