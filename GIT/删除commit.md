# 删除多余的commit 信息



+ git log 找到要删除的记录
+ git rebase -i +log记录 （这条log记录是要删除的commit信息的前一天commit号）
+ 将要删除的commit 之前dorp
+ esc  :wq保存退出
+ git push origin master --force 强制推送远程仓库