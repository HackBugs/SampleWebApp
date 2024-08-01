### Apache Tomcat and jenkins maven project
Author : ✍️ HackBugs
```sh
Download requrement software 
Apache Tomcat - 32-bit/64-bit Windows Service Installer (pgp, sha512) - https://tomcat.apache.org/download-90.cgi
Jenkins - Generic Java package (.war) - https://www.jenkins.io/download/
Install java JDK - jdk-8u202-windows-x64.exe 

Set environment Variable 
New System Variable 
Name - JAVA_HOME
Value - Path

Create Folder - C:\JENKINSHOME
Set environment Variable 
New System Variable 
Name - JENKINS_HOME
Value - Path

Start Apache Tomcat
http://localhost:8080/

Paste -  jenkins.war
C:\Program Files (x86)\Apache Software Foundation\Tomcat 9.0\webapps

http://localhost:8080/Jenkins
set the password of jenkins 
select plugins to install don't choose suggested plugins
select none > install > create usename password
start jeniks user interface

Download ant
ant - apache-easyant-core-0.9-incubating-bin.zip - https://ant.apache.org/easyant/download.cgi
extract in C drive 
Set environment Variable 
New System Variable 
Path - till bin location paste

create folder githubworkspace
clone github project inside this folder

Now final work with jenkins
what jenkins will do 
First - githubpull
Second - Build
Third - unitest
Forth - deploy

____________________________________________________________________________________________________________________
### 1
Now install jenkins necessary for this project
1. Github

1. Create New Item
Name - githubpull
select - Freestyle Project
configuratin > advanced > use custom workspce
path > ${JENKINS_HOME}/workspace/SampleWebApp

Git > paste > github project path 
Save
______________________________________________________________________________________________________________________
### 2
Now install jenkins necessary for this project
1. warnings next generation
2. Ant

2. Create New Item
Name - buildandcodereview
select - Freestyle Project
configuratin > advanced > use custom workspce
path > ${JENKINS_HOME}/workspace/SampleWebApp

Git > paste > github project path

Build
select > invoke Ant
target path leave empty

Post-Build action
select > record compiler warnings and static analysis results
Tool > CheckStyle
Report file pattern > checkstyle-result.xml
Save
______________________________________________________________________________________________________________________
### 3
Now install jenkins necessary for this project
1. JUnit Realtime test reporter

3. Create New Item
Name - unitest
select - Freestyle Project
configuratin > advanced > use custom workspce
path > ${JENKINS_HOME}/workspace/SampleWebApp

Git > paste > github project path

Build
select > invoke Ant
Target > junit

Post-Build action
select > Publish Junit test result report
Paste > github project which your already created .xml file name
Save
___________________________________________________________________________________________________________________________
### 4
Need to change some value in this folder - C:\Program Files (x86)\Apache Software Foundation\Tomcat 9.0\conf
file name - tomcat-users.xml
Paste this like already changed - <user username="admin" password="admin" roles="manager-gui,manager-script,admin" />
file name  - server.xml - http://localhost/8080 to http://localhost/8090

Now install jenkins necessary for this project
1. Deploy to Container

4. Create New Item
Name - deploy
select - Freestyle Project
configuratin > advanced > use custom workspce
path > ${JENKINS_HOME}/workspace/SampleWebApp

Git > paste > github project path

Build
select > invoke Ant
Target > war

Post-Build action
select > Deploy war/ear to container
War/Ear Files box
paste in box > **/*.war

Context Path box
samplewebapp

Add container 
select > tomcat version which you installed

Tomcat URL box 
Paste > Tomcat URL > http://localhost/8090
Save
________________________________________________________________________________________________________________________

Restart tomcat
you changed port 8080 to 8090 now start URL with - http://localhost/8090

Now install jenkins necessary for this project
1. Build pipeline

Now change in job configuration 
1. githubpull > Post-Build action > Build other projects > buildandcodereview
2. buildandcodereview > Post-Build action > Build other projects > unitest
3. unitest > Post-Build action > Build other projects > deploy
4. deploy > Post-Build action > Build other projects >

Add new > where these all job created on home Dashboard click on + icon click
Name - complate pipeline
select - build pipeline view > ok

upstream / downstream config
select first job > githubpull

githubpull > congiguration > poll SCM 
* * * * *
Save
____________________________________________________________________________________________________________________________
Go on Dashboard select > complate pipeline > click on "Run"
```




