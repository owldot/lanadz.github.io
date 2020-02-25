---
layout: post
title: 'Add git name and email per project'
date: 2019-07-05
tags:
  - terminal
keywords:
  - add git name and email per project
---

Sometimes you need to have different user names and emails for git for different projects (for example for personal and for work projects).

In this case you might find the following interesting.
So, in general, git related wconfiguration is stored in `~/.gitconfig` file and looks like

<!--more-->

```
[user]
        name = Your Name
        email = email@domain.com
```

More fields might be stored there.

This is global settings and git will use that conf for each pull/push

In case if you need to override that setting per project, please navigate to `project_dir/.git` and open `config` file there

```bash
$vi .git/config
```

and add section

```
[user]
        name = Your Name
        email = another_email@domain.com
```

and that's it, this will override global settings
