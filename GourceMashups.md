# Mashups with Gource #

Sometimes it may be interesting to show the history of multiple projects in the same Gource animation. For instance: development activity in sub modules used by a project, multiple projects worked on by the same developers / company, or maybe different forks of a project.

There are lots of possibilities.

Here are a few simple steps to create a mash-up using some shell commands:

## Steps ##

1. Generate a Gource custom log files for each repository using the --output-custom-log FILE option:

```
  gource --output-custom-log log1.txt repo1
  gource --output-custom-log log2.txt repo2
  ...
```

2. (optional) If you want each repo to appear on a separate branch instead of merged onto each other (which might also look interesting), you can use a 'sed' regular expression to add an extra parent directory to the path of the files in each project:

```
  sed -i -r "s#(.+)\|#\1|/repo1#" log1.txt
```

(note: sed -i does an in-place replacement on the file)

3. Join the logs together, and sort them numerically by the first column (the time):

```
  cat log1.txt log2.txt | sort -n > combined.txt
```

4. Feed result into gource:

```
  gource combined.txt
```

You can also add the **--hide-root** option to not connect the top level directories to make them look more distinct.