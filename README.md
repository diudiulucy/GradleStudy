# GradleStudy

## 定义任务
  ```
  task hello {
   doLast {
      println 'hello world'
   }
}
```
可以通过为 doLast 语句指定快捷方式（表示符号 <<）来简化此 hello 任务

```
task hello << {
   println 'hello world'
}
```

在存储 build.gradle 文件的目录位置执行以下命令，应该看到输出结果如下 

`gradle -q hello`

 hello world
 
 ## 任务依赖关系
```
task hello << {
    println 'Hello world!'
}
task intro(dependsOn: hello) << {
    println "I'm Gradle"
}
```

执行，应该看到输出结果如下 

Hello world!

I'm Gradle

要添加依赖关系，相应的任务不需要存在。懒依赖 - 其他任务不存在 加双引号""

```
task taskX(dependsOn: 'taskY') << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}
```