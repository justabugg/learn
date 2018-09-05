第三天啦 妈的早上五点半睡不着打鸡血了一样跑去吃早饭 现在上完12节继续回来学习
上午看了stash的暂存相关功能 还不错 但是我觉得可能用不上 等实战我再回来看看 主要都是分支的内容 强制删除什么的自带提示已经掌握了 
多人协作这块主要说的意思就是默认提交的是master分支 其他分支你可以视情况提交  多人协作先clone没问题 然后各自创建新分支提交时可能会报错 这是因为之前讲的冲突
先尝试pull下来 结果不行 说没有指定本地dev分支和远程origin/dev的链接 要gie branch --set-upstream-to=origin/dev dev
再pull成功 但是合并冲突  本地解决一下冲突后再提交push就ok了

睡了一觉美滋滋 起来迷了一会继续看
rebase开始有点迷 关于分支和log图感觉有点混乱了 主要pull和merge搞得烦人  其实冲突解决我也有点迷茫 不就是把重合的不一样的改掉再merge么
关于tag没啥说的  基本就打个标签version  
git tag v1.0   git tag v0.9 f52c633    git tag来打-a指定标签名 -m说明
git show v0.9来看
git tag -d <tagname>可以删除一个本地标签；
push origin :refs/tags/<tagname>可以删除一个远程标签。
最后就是一个码云的使用啦 看了一下差不多 需要的时候去remote一下GitHub和码云库
那这一个就结束啦 拜拜
下一个  python