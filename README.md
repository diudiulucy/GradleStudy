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

执行，输出结果

taskY

taskX

## 定位任务

代码访问任务作为属性

```
task hello

println hello.name
println project.hello.name

```

通过任务集合使用所有属性
```
task hello

println tasks.hello.name
println tasks['hello'].name

```

## 向任务添加依赖关系
 - 项目路径作为相应任务名称的前缀。
  ```
  task taskX << {
   println 'taskX'
}
task taskY(dependsOn: 'taskX') << {
   println "taskY"
}
  ```
 - 使用任务对象
  ```
  task taskY << {
   println 'taskY'
}
task taskX << {
   println 'taskX'
}
taskY.dependsOn taskX
  ```
  - 使用闭包
  ```
  task taskX << {
   println 'taskX'
}

taskX.dependsOn {
   tasks.findAll { 
     task -> task.name.startsWith('lib') 
   }
}
task lib1 << {
   println 'lib1'
}
task lib2 << {
   println 'lib2'
}
task notALib << {
   println 'notALib'
}
  ```
  执行后输出结果
  `
  lib1
lib2
taskX`


## 跳过任务

如果用于跳过任务的逻辑不能用谓词表示，则可以使用StopExecutionException。 如果操作抛出此异常，则会跳过此操作的进一步执行以及此任务的任何后续操作的执行。 构建继续执行下一个任务。
```
task compile << {
    println 'We are doing the compile.'
}

compile.doFirst {
    // Here you would put arbitrary conditions in real life.
    // But this is used in an integration test so we want defined behavior.
    if (true) { throw new StopExecutionException() }
}
task myTask(dependsOn: 'compile') << {
   println 'I am not affected'
}
```
输出
`I am not affected`
这里跳过了compile的执行
