---
layout: post
title: Git Archive
date:   2017-05-12 08:54:01 +0800
categories: git archive bundle patch am apply
tag: git
---

* content
{:toc}

Git Archive
------------------------

# Archive

```
git archive [--format=<fmt>] [--list] [--prefix=<prefix>/] [<extra>] [-o <file> | --output=<file>] [--worktree-attributes] [--remote=<repo> [--exec=<git-upload-archive>]] <tree-ish> [<path>...]

git archive master --format=zip --output=../my_archive_as_of_date.zip

git archive master --format=tar --output=../my_archive_as_of_date.tar

git archive --format=zip mytag subdir -9 > ../myarchive.zip
```

# Bundle

```
git bundle create <file> <git-rev-list-args> 
git bundle verify <file>
git bundle list-heads <file> [<refname>...] 
git bundle unbundle <file> [<refname>...]
```

```
git bundle create ../myrepo.bundle master

git clone -b master ../myrepo.bundle myrepo
git bundle create ../myrepo.bundle -5 master  # 5 commits ago

git bundle create ../myrepo.bundle --since=5.days

git bundle create ../myrepo.bundle master~5..master
git bundle create ../myrepo.bundle firstBundleTag..master
```

# Patch
```
git format-patch
    [-k] [(-o|--output-directory) <dir> | --stdout] [--no-thread | --thread[=<style>]] [(--attach|--inline)[=<boundary>] | --no-attach] [-s | --signoff]
    [--signature=<signature> | --no-signature] [--signature-file=<file>]
    [-n | --numbered | -N | --no-numbered]
    [--start-number <n>] [--numbered-files] [--in-reply-to=Message-Id] [--suffix=.<sfx>] [--ignore-if-in-upstream]
    [--subject-prefix=Subject-Prefix] [(--reroll-count|-v) <n>] [--to=<email>] [--cc=<email>]
    [--[no-]cover-letter] [--quiet] [--notes[=<ref>]] [<common diff options>]
    [ <since> | <revision range> ]
```

```
git format-patch -3 HEAD

```

# Apply
```
git apply [--stat] [--numstat] [--summary] [--check] [--index] 
          [--3way] [--apply] [--no-add] [--build-fake-ancestor=<file>] 
          [-R | --reverse] [--allow-binary-replacement | --binary] [--reject] [-z]
          [-p<n>] [-C<n>] [--inaccurate-eof] [--recount] [--cached] 
          [--ignore-space-change | --ignore-whitespace ] [--whitespace=(nowarn|warn|fix|error|error-all)] 
          [--exclude=<path>] [--include=<path>] [--directory=<root>] [--verbose] [--unsafe-paths] [<patch>...]
```
```
git apply <name of file in patch format>
```
```
git apply --cached|--index <name of file in patch format>
```

# am - apply mailbox

```
git am [--signoff] [--keep] [--[no-]keep-cr] [--[no-]utf8] [--[no-]3way] [--interactive] 
       [--committer-date-is-author-date] [--ignore-date] [--ignore-space-change | --ignore-whitespace] 
       [--whitespace=<option>] [-C<n>] [-p<n>] [--directory=<dir>] [--exclude=<path>] [--include=<path>] 
       [--reject] [-q | --quiet] [--[no-]scissors] [-S[<keyid>]] [--patch-format=<format>] [(<mbox> | <Maildir>)...]
git am (--continue | --skip | --abort)
```
```
git am <name of file in patch format>

```

# email patches

```
git send-email [options] <file|directory|rev-list options>...
git send-email --dump-aliases
```
```
git config --global sendemail.from <your email address>
git config --global sendemail.smtpserver <your email smtp server>
```
```
git send-email --to bcl@nclasters.org *.patch
```

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
