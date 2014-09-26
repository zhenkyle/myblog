---
layout: post
title:  "Struts Hibernate Spring 巧妙注入"
date:   2014-09-26 14:11
categories: 
---

###Struts Action 注入 Service的传统方式

在`struts.xml`中

```
		<!-- 门店列表Action -->
		<action name="shopList" class="ShopManagerAction" method="shopList">
			<result name="success">/pages/shop/shopList.jsp</result>
		</action>
```

在 spring 的 `applicationContext.xml`中

```
	<!-- 门店Action注入 -->
	<bean id="shopManagerAction"
		class="com.company.mis.system.action.order.ShopManagerAction" scope="prototype">
		<property name="shopService" ref="shopService" />
	</bean>
```

###Struts Action 注入 Service的自动方式

在`struts.xml`中 写全限定的类名

```
		<!-- 门店列表Action -->
		<action name="shopList" class="com.company.mis.system.action.shop.ShopManagerAction" method="shopList">
			<result name="success">/pages/shop/shopList.jsp</result>
		</action>
```

在 spring 的 `applicationContext.xml`中

不需要再定义`shoManagerAction`了,会自动注入`shopService`


###Service 注入 DAO 的传统方式

在spring 的 `applicationContext.xml`中

```
	<!-- 门店Service -->
	<bean id="shopService"
		class="com.company.mis.system.service.impl.ShopServiceImpl">
		<property name="shopDao" ref="shopDao" />
	</bean>
```

###Service 注入 DAO 的自动方式

在spring 的 `applicationContext.xml`中，首先定义

```
	<!-- 定义业务逻辑组件模板 -->
	<!-- 为之注入DAO组件 -->
	<bean id="myTemplate" abstract="true" lazy-init="true"
		p:shopDao-ref="shopDao"
		p:purviewDao-ref="orderDao" />
```

然后再定义

```
	<!-- 定义我的逻辑组件，继承业务逻辑组件的模板 -->
	<bean id="shopService"
		class="com.company.mis.system.service.impl.ShopServiceImpl" 
		parent="myTemplate"/>
```
就不用定义具体的注入了
