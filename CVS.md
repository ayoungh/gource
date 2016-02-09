## Using Gource with CVS projects ##

# cvs2cl #

[cvs2cl](http://www.red-bean.com/cvs2cl/) is script that converts CVS log output into more useful formats, including XML, which Gource is able to read as of version **0.29**:

```
cd my-cvs-project
cvs2cl --chrono --stdout --xml -g-q > my-cvs-project.xml
gource my-cvs-project.xml
```

# cvs-exp #

**NOTE:** support for cvs-exp is deprecated.

[cvs-exp.pl](http://gource.googlecode.com/files/cvs-exp.pl) is a tool which converts the CVS log output into a more conventional changes grouped together into chronological order that Gource knows how to read.

Assuming cvs-expl.pl is installed (and you have [perl](http://www.perl.org/)), you can generate a log from your CVS project and run in Gource by doing the following:

```
cd my-cvs-project
cvs-exp.pl -notree > my-cvs-project.log
gource my-cvs-project.log
```