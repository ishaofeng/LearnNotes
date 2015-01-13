##Gradle构建系统学习
---
1.build.gradle  构建系统配置文件, 
2.代码构建使用命令`gradle build`
3.构建过程基本概念: projects和tasks
    project是最终需要生成的东西，　要么是一个jar, 要么是一个war
    最终这个目的的实现是通过一个或多个任务来完成
    
    task是构建过程的一个步骤, 例如编译一个java文件为class文件, 构建
    过程是原子的
4.常用的task
    对于一个 task hello {println 'Hello world'}
    使用名 gradle hello执行, 输出为'Hello world'

    对于上面同样的命令也支持task hello << { println 'Hello World'}写法类似
    
    任务依赖:
        task hello << {pringln "Hello World"}
        task intro (dependsOn: hello) << {println "intro runs after hello"}

        任务内建接口:
            task hello << {
                pringln 'Hello Earth'
            }
            hello.doFirst {
                println 'Hello Venus'
            }
            hello.doLast {
                println 'Hello Mars'
            }
            hello << {
                pringln 'Hello Jupiter'
            }

            输出内容为:
                Hello Venus
                Hello Earth
                Hello Mars
                Hello Jupiter


5.添加以来包
    apply plugin: 'java'
    apply plugin: 'eclipse'

    //指定生成的jar基于的jdk的版本
    sourceCompatibility = 1.6
    targetCompatibility = 1.6
    //或者
    compileJava {
        sourceCompatibility = 1.6
        targetCompatibility = 1.6
    }

    //指定manifest文件细节
    jar {
        manifest {
            attributes 'Implementation-Title': 'demo gradle Quickstart' ,
                'Implementation-Version': 1.0
        }
    }
    //生成的jar文件中manifest将包含上述信息

    repositories {
        mavenCentral()
    }

    dependencies {
        compile 'org.springframework:spring-context:4.0.5.RELEASE'
    }


6. gradle -q build   将关闭log信息
7. test upper << {
    String some = "mY_nAmE"
    pringln "Original: " + some
    pringln "Upper case: " + some.toUpperCase()
}

task count << {
    4.times {print "$it"}
}

8.动态任务,动态创建任务
    4.times {counter ->
        task "task$counter" << {
            println "i'm task number of $counter"
        }
    }

    //任务依赖
    task0.dependsOn task2, task3

9.任务额外属性
    task myTask {
        ext.myProperty = "myvalue"
    }

    task pringTaskProperties << {
        pringln myTask.myProperty
    }

10. task loadfile << {
    def files = file('/home/shao').listFiles().sort()
    files.each{ File file -> 
        if (file.isFile()) {
            println " *** $file.name ***"
        }
    }
}

11.定义函数和使用函数
    File[] filesList(String dir) {
        file(dir).listFiles({file -> file.isFile()}) as FileFilter).sort()
    }

12.配置子项目
    在父项目的根项目之下,寻找settings.gradle文件, 在该文件中设置想要包括到
项目中构建的子项目
    在构建的初始化阶段, gradle会根据settings.gradle文件来判断有哪些子项目
被include到构建中, 并为每个子项目初始化一个Project对象, 在构建脚本中使用
project(':sub-project-name')来引用子项目对应的Project对象

13.共享配置
    allprojects
    subprojects
    configure 部分项目配置
        configure(subprojects.findAll{it.name.contains("war")}) {

        }

14.子项目独享配置
方法一:
   project(':core') {

   }
方法二:
    在子项目中使用独立的build.gradle
   
