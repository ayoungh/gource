## Built-in Support ##

As of **0.29**, Gource includes SVN parsing and log generation built-in:

```
cd my-svn-project
gource
```

You can also save a copy of the log first and use that, to avoid the SVN server having to generate it each time:

```
cd my-svn-project
svn log -r 1:HEAD --xml --verbose --quiet > my-project-log.xml
gource my-project-log.xml
```

## Previous Versions ##

There are a couple ways to use older versions of Gource with an SVN repository. First is using **git-svn** (detailed below), the second, currently less robust way is to use **svn-gource.py**, a script that converts the SVN xml log to Gource's custom log format.

### Using Gource with SVN projects via git-svn ###

The most reliable (in terms of output) way to use Gource with SVN is to import your project into a Git repository using the '`git svn`' command.

The below insturctions will work if your SVN repository has the standard three directories - trunk, tags and branches
(Otherwise, see the '`git svn`' documentation):

```
git svn init --stdlayout https://myrepo.example.org/svn my-repo.git
cd my-repo.git
git svn fetch
```

**Note:** `git svn fetch` may take hours if your repository is large.

You can pull new changes into the Git copy of your SVN repository at any time using the following command:

```
git svn rebase
```

### svn-gource.py ###

[Bitshifter](http://bitshifter.net.nz/) has contributed a python script [svn-gource.py](http://gource.googlecode.com/files/svn-gource-1.2.tar.gz) that converts the XML output of the `svn log` command into a format that can be read by the gource custom log format option.

Synopsis:

```
cd my-svn-project
svn log --verbose --xml > my-project.log
python svn-gource.py --filter-dirs my-project.log > my-project-gource.log
gource --log-format custom my-project-gource.log
```


Caveats:

  * The `--filter-dirs` option treats paths without extensions as directories and filters them out (files and directories are ambiguous in the SVN log output so this is a work around).
  * Not all the commands in the SVN log are currently converted (e.g. folder copying).

This script requires Python version 2.5 or above.