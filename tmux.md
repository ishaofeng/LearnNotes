##tmux使用
---
tmux能够持久保存工作状态，允许我们重新连接或者断开连接

###2.复制模式
* `prefix+[`进入复制模式
* `Space`开始复制内容,在此之前可以移动光标选择开始赋值的位置,此时可以移动光标选择需要复制的内容
* `Esc`取消当前选择，
* `Enter`退出复制模式
* `prefix+]`将光标移动到指定位置粘贴
* `setw -g mode-keys vi`复制模式快捷键使用vi模式

###3.会话(Session)
* `tmux ls`列出所有的会话
* `tmux new -s sessionname`创建一个命名为sessionname的session
* `tmux new -s sessionname -d`创建一个命名为sessionname的后台session
* `tmux attach -t sessionname`进入一个指定的会话
* `tmux kill-session -t sessionname`关闭一个指定的会话

