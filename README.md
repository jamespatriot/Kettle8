# Pentaho Data Integration #

Pentaho-kettle 官方8.1.0.2-457版本，但是在maven打包过程中很容易出现各种问题，以及在Debug调试过程中很容易出现jar包冲突和其它问题,经过多次尝试后修改的一个版本，可以直接打包和启动，现将修改处记录下来

编译测试环境：
win10 64 IDEA

修改记录：

1.将kettle 源文件中出现 8.1.0.2-457 的位置(主要是pom文件)全部替换为8.1.0.2-499,原因主要是pom依赖中很多依赖包不存在8.1.0.2-457版本的
(如果你的版本不是8.1.0.2-457请忽略该条)


2.删除./plugins文件夹和./assemblies/plugins文件夹,同时将./assemblies中pom文件夹中引用./plugins中依赖的地方全注释掉（调试完成后将需要的plugins加到根目录下，但是请去掉不用的插件，因为可能出现不必要的jar包冲突）


3.将模块中有依赖为kettle-core 或者 kettle-ui-swt 或者 kettle-engine的且scop标注为test的全都去掉（dependency全去掉）


4.将与linux和mac有关的swt依赖去掉(因为在win64上调试报错)


5.将./assemblies/static/src/main/resource/ui/下的文件copy到./ui下


6.其它几处小修改

7.警告：不建议拉取我的版本，该版本可能存在巨坑（且可以运行调试，上述操作方案可以用来作为运行调试的指导，所以此工程仅该文件有用）


Pentaho Data Integration ( ETL ) a.k.a Kettle

### Project Structure

* **assemblies:** 
Project distribution archive is produced under this module
* **core:** 
Core implementation
* **dbdialog:** 
Database dialog
* **ui:** 
User interface
* **engine:** 
PDI engine
* **engine-ext:** 
PDI engine extensions
* **[plugins:](plugins/README.md)** 
PDI core plugins
* **integration:** 
Integration tests

How to build
--------------

Pentaho Data Integration uses the maven framework. 


#### Pre-requisites for building the project:
* Maven, version 3+
* Java JDK 1.8
* This [settings.xml](https://raw.githubusercontent.com/pentaho/maven-parent-poms/master/maven-support-files/settings.xml) in your <user-home>/.m2 directory

#### Building it

This is a maven project, and to build it use the following command

```
$ mvn clean install
```
Optionally you can specify -Drelease to trigger obfuscation and/or uglification (as needed)

Optionally you can specify -Dmaven.test.skip=true to skip the tests (even though
you shouldn't as you know)

The build result will be a Pentaho package located in ```target```.

#### Running the tests

__Unit tests__

This will run all unit tests in the project (and sub-modules). To run integration tests as well, see Integration Tests below.

```
$ mvn test
```

If you want to remote debug a single java unit test (default port is 5005):

```
$ cd core
$ mvn test -Dtest=<<YourTest>> -Dmaven.surefire.debug
```

__Integration tests__

In addition to the unit tests, there are integration tests that test cross-module operation. This will run the integration tests.

```
$ mvn verify -DrunITs
```

To run a single integration test:

```
$ mvn verify -DrunITs -Dit.test=<<YourIT>>
```

To run a single integration test in debug mode (for remote debugging in an IDE) on the default port of 5005:

```
$ mvn verify -DrunITs -Dit.test=<<YourIT>> -Dmaven.failsafe.debug
```

To skip test

```
$ mvn clean install -DskipTests
```

To get log as text file

```
$ mvn clean install test >log.txt
```


__IntelliJ__

* Don't use IntelliJ's built-in maven. Make it use the same one you use from the commandline.
  * Project Preferences -> Build, Execution, Deployment -> Build Tools -> Maven ==> Maven home directory


### Contributing

1. Submit a pull request, referencing the relevant [Jira case](http://jira.pentaho.com/secure/Dashboard.jspa)
2. Attach a Git patch file to the relevant [Jira case](http://jira.pentaho.com/secure/Dashboard.jspa)

Use of the Pentaho checkstyle format (via `mvn site` and reviewing the report) and developing working 
Unit Tests helps to ensure that pull requests for bugs and improvements are processed quickly.

