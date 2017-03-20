# Tomcat server.xml 配置详解
本次使用的配置文件基于 apache tomcat 8.0.18 版本，下面贴出直接从 tomcat 目录复制出的配置文件内容
```xml
<?xml version='1.0' encoding='utf-8'?>
<Server port="8005" shutdown="SHUTDOWN">
	<Listener className="org.apache.catalina.startup.VersionLoggerListener" />
	<Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
	<Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
	<Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
	<Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />

	<GlobalNamingResources>
		<Resource name="UserDatabase" auth="Container"
			type="org.apache.catalina.UserDatabase"
			description="User database that can be updated and saved"
			factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
			pathname="conf/tomcat-users.xml" />
	</GlobalNamingResources>

	<Service name="Catalina">
		<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
		<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />

		<Engine name="Catalina" defaultHost="localhost">
			<Realm className="org.apache.catalina.realm.LockOutRealm">
				<Realm className="org.apache.catalina.realm.UserDatabaseRealm" resourceName="UserDatabase"/>
			</Realm>

			<Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
				<Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
					prefix="localhost_access_log" suffix=".txt"
					pattern="%h %l %u %t &quot;%r&quot; %s %b" />
			</Host>
		</Engine>
	</Service>
</Server>
```
## Server
```xml
<Server port="8005" shutdown="SHUTDOWN">
```
`<Server>`元素代表了整个Catalina Servler容器,它是Tomcat实例的顶层元素。

由`org.apache.catalina.Server`接口来定义<Server>元素中可以包含一个或者多个`<Service>`元素,但`<Server>`元素不能作为任何其他元素的子元素。

####Server 属性说明：
- **className：**指定实现`org.apache.catalina.Server`接口的类,默认值为`org.apache.catalina.core.StandardServer`。

- **port：**指定 Tomcat 服务器监听`shutdown`命令的端口，终止 Tomcat 服务运行时，必须在 Tomcat 服务器所在的机器上发出`Shutdown`命令，该属性是必须设定的。

- **shutdown：**指定终止 Tomcat 服务器运行时，发给 Tomcat 服务器的`shutdown`监听端口的字符串，该属性是必须设定的。

## Service
```xml
<Service name="Catalina">
```
`<Service>`元素由`org.apache.catalina.Service`接口定义，它包含一个`<Engine>`元素，以及一个或多个`<Connector>`元素，这些`<Connector>`元素共享一个`<Engine>`元素，例如：在范例文件中配置了两个`<Service>`元素。

#### Service 属性说明：
- **className：**指定实现`org.apache.catalina.Service`接口的类，默认值为`org.apache.catalina.core.StandardService`。

- **name：**定义Service的名字。

## Connector
```xml
<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
```
`<Connector>`元素由`org.apache.catalina.Connector`接口定义，`<Connector>`元素代表与客户程序实际交互的组件，它负责接收客户的请求以及向客户返回响应结果。

`<Context>`元素由`org.apache.catalina.Context`接口定义，`<Context>`元素是使用最频繁的元素。
每个`<Context>`元素代表了运行在虚拟主机上的单个 Web 应用，一个`<Host>`元素中可以包含多个`<Context>`元素。

第一个`<Connector>`元素定义了一个 HTTP Connector，它通过`8080`端口接收HTTP请求。

第二个`<Connector>`元素定义了一个 JK Connector，它通过`8009`端口接收由其他 HTTP 服务器（如`Apache`服务器）转发过来的客户请求。

#### Connector 属性说明

所有的`<Connector>`元素都具有一些共同的属性,这些属性如下:
```xml
<Connector port="8080"
    maxThreads="150" minSpareThreads="25" maxSpareThreads="75"
    enableLookups="false" redirectPort="8443" acceptCount="100"
    debug="0" connectionTimeout="20000"
    disableUploadTimeout="true" />
```
**className：**指定实现`org.apache.catalina.Connector`接口的类，默认值为`org.apache.catalina.core.StandardConnector`。

**enableLookups：**如果设为`true`，表示支持域名解析，可以把IP地址解析为主机名，Web应用调用`request.getRemostHost`方法将返回客户的主机名，该属性默认值为`true`。

**redirectPort：**指定转发端口，如果当前端口只支持`non-SSL`请求，在需要安全通信的场合，将把客户请求转发到基于`SSL`的`redirectPort`的端口。

#### HTTP Connector 类型属性说明

- **calssName：**指定实现`org.apache.catalina.Connector`接口的类，默认值为`org.apache.coyote.tomcat5.CoyoteConnector`。

- **enableLookups：**同上。

- **redirectPort：**同上。

- **prot：**设定 `TCP/IP` 断口号,默认为`8080`。

- **address：**如果服务器有两个以上 IP 地址，该属性可以设定端口监听的 IP 地址，默认情况下端口会监听服务器上所有 IP 地址。

- **bufferSize：**设定由端口创建的输入流的缓存大小，默认值为`2048byte`。

- **protocol：**设定 HTTP 协议，默认值为`HTTP/1.1`。

- **maxThreads：**设定处理客户请求的线程的最大数目，这个值也决定了服务器可以同时响应客户请求的最大数目，默认值为`200`。

- **acceptCount：**设定在监听端口队列中的最大客户请求数,默认值为`10`，如果队列已满客户请求将被拒绝。

- **connectionTimeout：**定义建立客户连接超时的时间，以毫秒为单位。如果设置为`-1`表示不限制建立客户连接的时间。

### JK Connector 类型属性说明
- **className：**指定实现`org.apache.catalina.Connector`接口的类，默认值为`org.apache.coyote.tomact5.CoyoteCnnector`。

- **enableLookups：**同上。

- **redirectPort：**同上。

- **port：**设定`AJP`端口号。

- **protocol：**必须设定为`AJP/1.3`协议。

## Engine
```xml
<Engine name="Catalina" defaultHost="localhost">
```
`<Engine>`元素由`org.apahe.catalina.Engine`接口定义，每个`<Service>`元素只能包括一个`<Engine>`元素，`<Engine>`元素处理在同一个`<Service>`中所有`<Connector>`元素接收到的客户请求。

在<Engine>元素中可以包含如下的子元素:
```xml
<Logger>
<Realm>
<Valve>
<Host>
```

#### Engine 属性说明

- **className：**指定实现`org.apache.catalina.Service`接口的类，默认值为`org.apache.catalina.core.StandardService`。

- **name：**定义 Service 的名字。

## Host
```xml
<Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
```
`<Host>`元素由`org.apache.catalina.Host`接口定义，一个`<Engine>`元素可以包含多个`<Host>`元素，每个`<Host>`元素定义了一个虚拟主机，它可以包含一个或多个 Web 应用。

在`<Host>`元素中可以包含如下的子元素:
```
<Logger>
<Realm>
<Valve>
<Context>
```
- **className：**指定实现`org.apache.catalina.Host`接口的类，默认值为`org.apache.catalina.core.StandardHost`。

- **appBase：**指定虚拟主机的目录，可以指定绝对目录，也可以指定相对于`<CATALINA_HOME>`的相对目录，如果此项没有设定，默认值为`<CATALINA_HOME>/webapps`。

- **unpackWARs：**如果此项设为`true`，表示将把 Web 应用的 WAR 文件先展开为开放目录结构后再运行，如果设为`false`将直接运行WAR文件。

- **autoDeploy：**如果此项设为`true`，表示当 Tomcat 服务器处于运行状态时，能够监测`appBase`下的文件，如果有新的 Web 应用加入进来，会自动发布这个 Web 应用。

- **alias：**指定虚拟主机的别名，可以指定多个别名。

- **deployOnStartup：**如果此项设为`true`，表示 Tomcat 服务器启动时会自动发布`appBase`目录下的所有Web应用，如果 Web 应用在`server.xml`中没有相应的`<Context>`元素，将采用 Tomcat 默认的`Context deployOnStartup`的默认值为`true`。

- **name：**定义虚拟主机的名字。

## Context
```xml
<Context path="/MyProject" docBase="MyProject" reloadable="true"/>
```
`<Context>`元素由`org.apache.catalina.Context`接口定义，`<Context>`元素是使用最频繁的元素，每个`<Context>`元素代表了运行在虚拟主机上的单个 Web 应用，一个`<Host>`元素中可以包含多个`<Context>`元素。

在<Context>元素中可以包含如下的子元素:
```xml
<Logger>
<Realm>
<Valve>
<Resource>
<ResourceParams>
```

#### Context 属性说明
- **className：**指定实现`org.apache.catalina.Context`接口的类，默认值为`org.apache.catalina.core.StandardContext`。

- **path：**指定访问该 Web 应用的 URL 入口。

- **docBase：**指定 Web 应用的文件路径，可以给定绝对路径，也可以给定相对于`<Host>`的`appBase`属性的相对路径，如果 Web 应用采用开放目录结构，那就指定 Web 应用的根目录，如果 Web 应用是个 WAR 文件那就指定 WAR 文件的路径。

- **reloadable：**如果这个属性设为`true`，Tomcat 服务器在运行状态下会监视在`WEB-INF/class和WEB-INF/lib`目录下`calss`文件的改动，如果检测到有`calss`文件被更新,服务器会自动重新加载 Web 应用。

- **cookies：**指定是否通过Cookie来支持Session，默认为true。

- **useNaming：**指定是否支持JNDI，默认为true。

## 使用 Context 实现通过 IP 直接访问项目
正常情况下，在访问在Tomcat中部署的项目是 http://localhost:8080/demo 方式

其中，IP，端口，项目名(Demo)都是必须的。

那么，怎么样才能通过 http://localhost 就能直接访问我们的项目呢？

```
<Context path="" docBase="demo" reloadable="true"/>
```
通过`<Context>`配置`path`为`""`，`demo`为`"需要访问的应用"`，来去掉`/demo`后缀

好了现在我们可以通过 http://localhost:8080 来访问我们的项目了。

```
<Connector port="80" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
```
通过`<Connector>`配置`port`为`80`，来去掉端口号。

好了现在我们可以通过 http://localhost 来访问我们的项目了。
