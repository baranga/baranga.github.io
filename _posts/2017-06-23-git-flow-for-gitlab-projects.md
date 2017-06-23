---
layout: post
title:  "Use gitflow on gitlab projects with merge requests"
categories: blog
---

I use [gitflow-avh][gf-avh] and [gitlab][gl] for several years and I'm quiet happy
with these tools. It just happened some months ago my team and me started some
projects using gitflow together with gitlab merge requests (MR). Merge
request are really helpful for code reviews and discussions around features.
But unfortunately they don't integrate with gitflow very well.
<!--more-->
 
Gitlab encourages you to use [gitlab flow][gl-f] which works quiet different
than the gitflow [approach][gf-nvie]. Branches getting merged against
develop (like feature branches) are no problem. Just adjust your target
branch. But branches getting merged against master are another topic.

Gitflow does a lot more then gitlab when merging a release or hotfix
branch against master: it creates a tag and updates develop. So naturally one
would just get the approval of colleges and automated test results via CI in
the MR and do the merge locally afterwards.

Gitlab supports this approach and will mark a MR as merged as soon as it
detects it got merged into target branch. It "just" needs the source branch
to stay around for that. That's my problem: gitflow automatically deletes the
merged branch remotely and gitlab can't detect the MR got merged.

To work around one could do two things:
- finishing feature/hotfix/release with `--keepremote` flag
  ```
  git flow release finish 1.2.3 --keepremote
  ```
- or enable `--keepremote` flag permanently:
  ```
  git config --add gitflow.release.finish.keepremote true
  ```
  
Now gitlab will detect a MR got merged, mark it as this and provide a
"remove source" button. To enable this in all my projects a created a
[small script][gl-snippet] and run it in the relevant git repositories.

[gf-avh]: https://github.com/petervanderdoes/gitflow-avh
[gf-nvie]: http://nvie.com/posts/a-successful-git-branching-model/
[gl]: https://gitlab.com/
[gl-f]: https://docs.gitlab.com/ee/workflow/gitlab_flow.html
[gl-snippet]: https://gitlab.com/snippets/1665860
