---
sidebar_position: 6
---

走开源的流程，报名时有些同学对开源流程并不熟悉，可以先 google 或 chatgpt 下熟悉熟悉流程。基本流程是，1）checkout 分支，2）提交代码，3）git push，4）发起 PR 然后群里说下，5）我或其他同学会 Review 并合并代码到 master 分支。

$ git checkout -b xxx
$ git commit -am xxx
$ git push
$ gh pr create (这句不知道是什么意思)

question[1]: 本地分支落后远程分支,git push 出错
```sh
(base) sun@sundeMacBook-Pro chatgpt-client % git push
To github.com:sorrycc/chatgpt-client.git
 ! [rejected]        Carefree-happy -> Carefree-happy (non-fast-forward)
error: failed to push some refs to 'github.com:sorrycc/chatgpt-client.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
(base) sun@sundeMacBook-Pro chatgpt-client % git pull
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
```

resolved[会退代码,再拉取远程代码]: git reset --hard 848e0a48[希望回退到的commitID]

试错: 创建upstream, merge 均被阻止，真强
