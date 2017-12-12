[gradle offical](https://gradle.org/)  
[Gradle cn](http://www.yiibai.com/gradle/)   
[Gradle User Guide](http://wiki.jikexueyuan.com/project/GradleUserGuide-Wiki/)  
[groovy offical](http://www.groovy-lang.org/)  
[Groovy cn](https://www.w3cschool.cn/groovy)  
[github bulidship](https://github.com/eclipse/buildship/blob/master/docs/user/Installation.md)

# 依赖项目
此项目是为了测试Gradle的多模块管理特性，此项目依赖于以下两个模块：
1. [gradle-demo](https://github.com/Donaldhan/gradle-demo)  
2. [gradle-webapp-demo](https://github.com/Donaldhan/gradle-webapp-demo)
*gradle-demo* 为基础工具项目common， *gradle-webapp-demo* 为web应用模块，*gradle-webapp-demo* 依赖于 *gradle-demo*，
此项目，用于管理两个模块。

*gradle-webapp-demo* 依赖于 *gradle-demo* 主要在 *build.gradle* 中配置编译依赖，具体如下：

```
dependencies {
    ...
    compile project(":gradle-demo")
}
```
本项目管理 *gradle-webapp-demo* 和 *gradle-demo* 首先在 *setting.gradle* 配置项目引用：
```
rootProject.name = 'gradle-modules-demo'
// To declare projects as part of a multi-project build use the 'include' method
include ':gradle-demo', ':gradle-webapp-demo'
project(':gradle-demo').projectDir = new File(settingsDir, '../gradle-demo')
project(':gradle-webapp-demo').projectDir = new File(settingsDir, '../gradle-webapp-demo')
```

同时在 *build.gradle* 中添加 *eclipse* 插件，同时配置模块依赖：
```
apply plugin: 'eclipse'
```

```
//子项目模块
dependencies {
    compile project(':gradle-demo')
    compile project(':gradle-webapp-demo')
}
```

然后在## 运行web项目
1. 在 Eclipse 菜单中 Window -> Show View -> Other... -> Gradle -> Gradle Tasks 打开 *Gradle任务*，
右键单击 *build* 并选择 *Run Gradle Tasks* 执行任务。
2. 在项目名称上点击右键，在弹出的菜单选项中选择 Run As -> Run Configurations... 在弹出的界面中选择 *New*，输入以下几个内容：
* Name: tomcatRun
* Gradle Tasks: tomcatRun
* Working Directory: ${workspace_loc:/gradle-webapp-demo},如有必要，还需要指定 Gradle和JDK的安装的目录.
具体可以参考 *gradle-webapp-demo* 的  *readme* 中的[运行项目小节](https://github.com/Donaldhan/gradle-webapp-demo#%E8%BF%90%E8%A1%8Cweb%E9%A1%B9%E7%9B%AE)。

运行项目控制输出：
```
Working Directory: F:\github\gradle-modules-demo
Gradle User Home: D:\.gradle
Gradle Distribution: Local installation at D:\gradle-3.1
Gradle Version: 3.1
Java Home: D:\JDK8
JVM Arguments: None
Program Arguments: None
Build Scans Enabled: false
Offline Mode Enabled: false
Gradle Tasks: tomcatRun

:gradle-demo:compileJava UP-TO-DATE
:gradle-demo:processResources UP-TO-DATE
:gradle-demo:classes UP-TO-DATE
:gradle-demo:jar UP-TO-DATE
:gradle-webapp-demo:compileJava UP-TO-DATE
:gradle-webapp-demo:processResources UP-TO-DATE
:gradle-webapp-demo:classes UP-TO-DATE
:gradle-webapp-demo:tomcatRun
Started Tomcat Server
The Server is running at http://localhost:8080/gradle-webapp-demo

```
访问 http://localhost:8080/gradle-webapp-demo 即可。

命令行进入本项目的跟目录，使用 *gradle tasks* 查看项目的任务，命令行输出：
```
donald@donaldHP MINGW64 /f/github/gradle-modules-demo (master)
$ gradle tasks
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles main classes.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.
testClasses - Assembles test classes.
war - Generates a war archive with all the compiled classes, the web-app content and the libraries.

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code.

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'gradle-modules-demo'.
components - Displays the components produced by root project 'gradle-modules-demo'. [incubating]
dependencies - Displays all dependencies declared in root project 'gradle-modules-demo'.
dependencyInsight - Displays the insight into a specific dependency in root project 'gradle-modules-demo'.
help - Displays a help message.
model - Displays the configuration model of root project 'gradle-modules-demo'. [incubating]
projects - Displays the sub-projects of root project 'gradle-modules-demo'.
properties - Displays the properties of root project 'gradle-modules-demo'.
tasks - Displays the tasks runnable from root project 'gradle-modules-demo' (some of the displayed tasks may belong to subprojects).

IDE tasks
---------
cleanEclipse - Cleans all Eclipse files.
eclipse - Generates all Eclipse files.

Verification tasks
------------------
check - Runs all checks.
test - Runs the unit tests.

Web application tasks
---------------------
tomcatJasper - Runs the JSP compiler and turns JSP pages into Java source.
tomcatRun - Uses your files as and where they are and deploys them to Tomcat.
tomcatRunWar - Assembles the webapp into a war and deploys it to Tomcat.
tomcatStop - Stops Tomcat.

Rules
-----
Pattern: clean<TaskName>: Cleans the output files of a task.
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.

To see all tasks and more detail, run gradle tasks --all

To see more detail about a task, run gradle help --task <task>

BUILD SUCCESSFUL

Total time: 3.628 secs

donald@donaldHP MINGW64 /f/github/gradle-modules-demo (master)
$
```
从任务输出来看，任务列表中包括了 *Web application tasks* 分类任务, 同时多了一个 *IDE tasks* 分类任务，用于清理所有eclipse文件和所有生成的eclipse文件。
