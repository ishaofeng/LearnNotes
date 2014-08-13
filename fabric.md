#Fabric使用
***
##1.功能
 - 一个能够使用命令行方式执行任意Python函数的工具
 - 使用子程序方便的通过SSH执行shell命令
 - 通常用户使用Fabric编写并且执行Python函数或者任务来与远端服务器进行交互集成.

##2.快速教程
  fab是用来以命令行执行函数或者任务的工具, 任务文件或者是默认使用当前目录下的
fabfile.py或者使用-f filepath指定.
  * 任务参数
    <task name>:<arg>,<kwarg>=<value>,...
  * 失败处理
    from fabric.api import local, settings, abort
    from fabric.contrib.console import confirm

    def test():
        with settings(warn_only=True):
            result = local("./cmd", capture=True)
        if result.failed and not confirm("Test failed. Continue anyway?")
            abort("Aborting at user request")

    settings context manager用来对一个指定的代码块进行特殊设置
    local 用户执行本地命令
    run 用户执行远端命令

  * 指定执行任务的服务器
    全局指定: env.hosts = ["host1", "host2"],  env.hosts.extend
    命令行指定: fab -H host1,host2 mytask
    任务指定: fab mytask:hosts="host1;host2"
    装饰器指定: @hosts('host1', 'host2')
                def mytask():
                    run("ls")

    env.roledefs = {
        "db": ["db1", "db2"],
        "web": ["web1", "web2", "web3"]
    }

    @roles("db")
    def migrate():
        pass

    @roles("web")
    def update():
        pass
 * 用户自动登陆
    env.use_ssh_config 使用系统ssh的配置文件
