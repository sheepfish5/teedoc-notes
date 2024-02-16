# git找回丢失的提交

某些情况下会回到更早的提交，如果没有分支指向最新的提交，那么`git log`等命令无法显示最新的提交，看起来就像提交“丢失”了。

您好，我可以尝试帮您找回这3个提交。根据`网上的一些资料` [^1] [^2] [^3]，您可以使用以下步骤：

1. 使用`git reflog`命令查看您的提交历史，找到您想要恢复的提交的哈希值，例如`7823b45`。
2. 使用`git checkout 7823b45`命令切换到该提交，此时您的HEAD分支会处于游离状态。
3. 使用`git checkout -b new_branch`命令创建一个新的分支，例如`new_branch`，并将该提交作为该分支的起点。
4. 使用`git checkout master`命令切换回master分支。
5. 使用`git merge new_branch`命令将新分支合并到master分支，如果有冲突，可以手动解决。

希望这些步骤能够帮助您找回丢失的提交。如果您还有其他问题，欢迎继续提问。😊

源: 与必应的对话， 6/23/2023

[^1]: https://juejin.cn/post/7093718347753193503 "Git进阶系列 | 8. 用Reflog恢复丢失的提交 - 掘金"

[^2]: https://blog.csdn.net/u010121001/article/details/109268074 "Git HEAD 分支游离，切回master导致代码丢失的处理"

[^3]: https://zhuanlan.zhihu.com/p/42081574 "如何在 Git 中重置、恢复，返回到以前的状态 - 知乎"
