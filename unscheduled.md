# Tomcat常见的几种部署方式
[摘抄自https://www.cnblogs.com/mafly/p/tomcat.html](https://www.cnblogs.com/mafly/p/tomcat.html)

通常，我们在同一台服务器上对 Tomcat 部署需求可以分为以下几种：单实例单应用，单实例多应用，多实例单应用，多实例多应用。实例的概念可以理解为上面说的一个 Tomcat 目录。
- **单实例单应用**:比较常用的一种方式，只需要把你打好的 war 包丢在 webapps目录下，执行启动 Tomcat 的脚本就行了。
- **单实例多应用**:有两个不同的 Web 项目 war 包，还是只需要丢在webapps目录下，执行启动 Tomcat 的脚本，访问不同项目加上不同的虚拟目录。这种方式要慎用在生产环境，因为重启或挂掉 Tomcat 后会影响另外一个应用的访问
- **多实例单应用**: 多个 Tomcat 部署同一个项目，端口号不同，可以利用 Nginx 这么做负载均衡，当然意义不大。
- **多实例多应用**:多个 Tomcat 部署多个不同的项目。这种模式在服务器资源有限，或者对服务器要求并不是很高的情况下，可以实现多个不同项目部署在同一台服务器上的需求，来实现资源使用的最大化。

当你同一台服务器部署了多个不同基于 Tomcat 的 Web 服务时，会迎来下面几个极其现实的问题
- 当你需要对数十台 Tomcat 版本进行升级的时候，你需要怎么做？
- 当你需要针对每一个不同的 Web 服务分配不用的内存时，你需要怎么做？
- 当你需要启动多台服务器时，你需要怎么做？

按照下图可以实现更优雅的 Tomcat 单机多实例部署：
![tomcat推荐部署方式](../images/tomcatbushujiegou.png)
上图中的 `CATALINA_HOME` 指Tomcat安装路径，CATALINA_BASE 指实例所在位置。CATALINA_HOME 路径下只需要包含 bin 和 lib 目录，而 CATALINA_BASE 只存放 conf、webapps、logs 等这些文件，这样部署的好处在于升级方便，配置及安装文件间互不影响，在不影响 Tomcat 实例的前提下，替换掉 CATALINA_HOME 中的安装文件。


# 启动脚本参考

```

#!/bin/bash

export CATALINA_HOME=/software/apache-tomcat-8.5.11
export CATALINA_BASE=${1%/}
# export CATALINA_OPTS=""
# export JAVA_OPTS=""

echo $CATALINA_BASE

TOMCAT_ID=`ps aux |grep "java"|grep "Dcatalina.base=$CATALINA_BASE "|grep -v "grep"|awk '{ print $2}'`


if [ -n "$TOMCAT_ID" ] ; then
echo "tomcat(${TOMCAT_ITOMCAT_ID}) still running now , please shutdown it firest";
    exit 2;
fi

TOMCAT_START_LOG=`$CATALINA_HOME/bin/startup.sh`


if [ "$?" = "0" ]; then
    echo "$0 ${1%/} start succeed"
else
    echo "$0 ${1%/} start failed"
    echo $TOMCAT_START_LOG
fi

```

# 停止参考脚本

```

#!/bin/bash
export CATALINA_HOME=/software/apache-tomcat-8.5.11
export CATALINA_BASE=${1%/}
# export CATALINA_OPTS=""
# export JAVA_OPTS=""

echo $CATALINA_BASE

TOMCAT_ID=`ps aux |grep "java"|grep "[D]catalina.base=$CATALINA_BASE "|awk '{ print $2}'`

if [ -n "$TOMCAT_ID" ] ; then
TOMCAT_STOP_LOG=`$CATALINA_HOME/bin/shutdown.sh`
else
    echo "Tomcat instance not found : ${1%/}"
    exit

fi

if [ "$?" = "0" ]; then
    echo "$0 ${1%/} stop succeed"
else
    echo "$0 ${1%/} stop failed"
    echo $TOMCAT_STOP_LOG
fi

```


{% mermaid %}
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
{% endmermaid %}