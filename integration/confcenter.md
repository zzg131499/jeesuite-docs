## 介绍
配置中心提供应用管理，配置统一管理，应用监控等。

## 使用说明

### 添加依赖

```
<dependency>
  <groupId>com.jeesuite</groupId>
  <artifactId>jeesuite-config-client</artifactId>
 <version>1.1.5</version>
</dependency>
```

### 本地设置host
```
192.168.1.94 config.iyuangong.net
```

### 添加配置
普通项目添加在`confcenter.properties`,springboot项目添加在`application.properties`

```
#是否启用配置中心，默认：true
jeesuite.configcenter.enabled=false
#应用名，springboot默认：${spring.application.name}
jeesuite.configcenter.appName=passport
jeesuite.configcenter.base.url=http://config.iyuangong.net:7777
#当前环境
jeesuite.configcenter.profile=dev
#同步方式，默认:http
jeesuite.configcenter.sync-type=zookeeper
# 同步间隔，同步方式为：http时生效
jeesuite.configcenter.sync-interval-seconds=30
# 如果需要对配置RSA加密，则配置以下内容，证书需要提前生成
jeesuite.configcenter.encrypt-keyStore-location=classpath:passport.jks
jeesuite.configcenter.encrypt-keyStore-password=123456
jeesuite.configcenter.encrypt-keyStore-type=JCEKS
jeesuite.configcenter.encrypt-keyStore-alias=passport
jeesuite.configcenter.encrypt-keyStore-keyPassword=123456
```

### 通过JVM参数外部设置配置
```
-Djeesuite.configcenter.profile=dev
-Djeesuite.configcenter.encrypt-keyStore-location=/Users/jiangwei/secretkey/test1.jks
-Djeesuite.configcenter.encrypt-keyStore-password=123456
-Djeesuite.configcenter.encrypt-keyStore-alias=test1
-Djeesuite.configcenter.encrypt-keyStore-keyPassword=123456
```
### docker外部设置配置
```
-e jeesuite.configcenter.profile="dev"
-e jeesuite.configcenter.encrypt-keyStore-location="/Users/jiangwei/secretkey/test1.jks"
-e jeesuite.configcenter.encrypt-keyStore-password="123456"
-e jeesuite.configcenter.encrypt-keyStore-alias="test1"
-e jeesuite.configcenter.encrypt-keyStore-keyPassword="123456"
```

### 配置中心新增配置
1. 申请新增应用，分配账号
2. 用分配账号登录[http://192.168.1.94:7777(开发环境)](http://192.168.1.94:7777)  新增配置即可。
3. 测试

```
http://192.168.1.94:7777/api/fetch_all_configs?appName=${jeesuite.configcenter.appName}&env=dev&version=0.0.0
```

 
### 普通spring项目配置xml
1. 去掉原加载配置相关配置
2. 新增配置

```
<bean class="com.jeesuite.confcenter.spring.CCPropertyPlaceholderConfigurer">
	<property name="remoteEnabled" value="true" />
	<!-- 本地配置文件，无本地配置可不配置 -->
	<property name="locations">
	  <list>
		<value>classpath*:application.properties</value>
	  </list>
	</property>
</bean>
```


 
 