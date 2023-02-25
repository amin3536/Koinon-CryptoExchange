# CryptoExchange-Koinon
这是一套成熟的交易所系统，后端语言为Java，前端语言为uniapp，一站式安装
系统仅供学习使用，如有疑问可联系 移动端界面如图，全套功能已实现，包括：买卖交易、自动充值、自动提现、注册邮件接入、otc交易等。 将于稍后提供应用程序 如需交易所搭建演示请联系飞机：【koinon888】
截图如下

![16](https://user-images.githubusercontent.com/75057109/217417378-0cabc65f-b5a6-49b2-9b36-3e1fa780fc09.png)
![12](https://user-images.githubusercontent.com/75057109/217417447-765e3e6b-22b6-48de-81f9-0ca1848883ef.png)
![38](https://user-images.githubusercontent.com/75057109/217418257-7ab8c08c-8066-4185-8c16-bf2407360637.png)
![50](https://user-images.githubusercontent.com/75057109/217418266-39bdd8dd-5e10-4028-83d9-5c61111fbf9c.png)


功能列表

<img width="544" alt="Screenshot 2023-02-07 at 15 59 25" src="https://user-images.githubusercontent.com/75057109/217417497-07dd27c6-7870-4cf8-9155-f20a7de55ee4.png">


### 主要技术

- 后端：Spring、SpringMVC、SpringData、SpringCloud、SpringBoot
- 数据库：Mysql、Mongodb
- 其他：redis、kafka、阿里云OSS、腾讯防水校验、环信推送
- 前端：Vue、iView、less
- 同时提供IOS和Android版本。

1. admin
- 提供管理后台的所有服务接口，必须部署
- 依赖服务：mysql,redis,mongodb

2. agent-api
- 提供代理商管理后台的所有服务接口
- 依赖服务：mysql,redis,mongodb

3. chat
- 提供实时通讯接口，基础模块，需要部署
- 依赖服务：mysql,redis,mongodb

4. cloud
- 提供SpringCloud微服务注册中心功能，为基础模块，必须部署
- 依赖服务：无

5. exchange
- 提供撮合交易服务，场外交易不需要部署
- 依赖服务：mysql,mongodb,kafka

10. exchange-api
- 提供币币交易接口，没有币币交易的项目可以不部署
- 依赖服务：mysql,redis,mongodb,kafka

11. kline-robot
- K线机器人（获取历史K线）
- 依赖服务：mysql,redis,mongodb,kafka,cloud

12. market
- 提供币种价格、k线、实时成交等接口服务，场外交易不需要部署
- 依赖服务：mysql,redis,mongodb,kafka,cloud

12. kline-robot
- K线机器人（获取历史K线）
- 依赖服务：mysql,redis,mongodb,kafka,cloud

14. ucenter-api
- 提供用户相关的接口（如登录、注册、资产列表）,该模块为基础为基础模块，必须部署
- 依赖服务：mysql,kafka,redis,mongodb,短信接口，邮箱账号

13. otc-api
- 提供场外交易功能接口，没有场外交易的可以不部署
- 依赖服务：mysql,redis,mongodb,短信接口

15. wallet
- 提供充币、提币、获取地址等钱包服务，为基础模块，必须部署
- 依赖服务：mysql,mongodb,kafka,cloud



##  重点业务介绍

    后端框架的核心模块为 exchange,market模块。

    其中exhcnge模块完全采用Java内存处理队列,大大加快处理逻辑,中间不牵涉数据库操作,保证处理速度快,其中项目启动后采用继承ApplicationListener方式，自动运行；

    启动后自动加载未处理的订单,重新加载到JVM中，从而保证数据的准确，exchange将订单处理后，将成交记录发送到market;

    market模块主要都是数据库操作，将用户变化信息持久化到数据库中。主要难点在于和前端交互socket推送，socket推送采用两种方式，web端socket采用SpringSocket，移动端采用Netty推送,其中netty推送通过定时任务处理。
	
	

## 环境搭建
- Centos 7.8
- MySQL 5.7.16
- Redis 6.0
- Mongodb 4.0
- kafka_2.11-2.2.1
- nginx-1.19.0
- JDK 1.8
- Vue
- Zookeeper

## 服务部署准备

1. 项目用了Lombok插件，无论用什么IDE工具，请务必先安装Lombok插件
2. 项目用了QueryDsl，如果遇见以Q开头的类找不到，请先编译一下对应的core模块，例如core、exchange-core、xxx-core这种模块
3. 找不到的jar包在项目jar文件夹下
4. jdk版本1.8以上
5. 初始化sql在sql文件夹中配置文件
配置文件打开这个设置会自动建表
#jpa
#spring.jpa.hibernate.ddl-auto=update.

## 修改服务配置文件
请根据服务实际部署情况修改以下配置。配置文件位置如下，如果配置文件中没有某一项配置，说明该模块未使用到该项功能，无需添加：

```
各个模块/src/main/resources/application.properties


### 服务启动
 1. maven构建打包服务

 2. 将各个模块target文件夹下的XX.jar上传到自己的服务器

 3. 先启动cloud模块，再启动market，exchange模块，剩下的没有顺序。

### 提问和建议
- 使用Issuse，我们会及时跟进解答。
- 联系：


注意事项：
当内存不足时，在linux控制台输入top可以查看java进程占用了大量内存（一个java进程占用1G以上），因为有很多jar包需要运行，所以需要控制某些jar包使用的内存，目前控制以下4个：

- java -jar -Xms512m -Xmx512m -Xmn200m -Xss256k  admin-api.jar

- java -jar -Xms512m -Xmx512m -Xmn200m -Xss256k  cloud.jar

- java -jar -Xms512m -Xmx512m -Xmn200m -Xss256k  wallet.jar
