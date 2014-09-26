---
layout: post
title:  "Struts 传递 model 到 view"
date:   2014-09-26 15:41
categories: 
---

###Struts 在 action 和 view 传递 model 的传统方式： 传model的每一个属性

把model中的每一个属性传递到view, 以下是一个例子

在`struts.xml`中

```
		<action name="forwardToAddShop" class="com.company.mis.system.action.shop.ShopManagerAction"
			method="forwardToAddShop">
			<result name="success">/pages/shop/addShop.jsp</result>
		</action>
		<action name="addShop" class="com.company.mis.system.action.shop.ShopManagerAction" method="addShop">
			<result name="success" type="chain">
				<param name="actionName">confirm</param>
				<param name="namespace">/</param>
			</result>
		</action>
```

`ShopmanagerAction.java`

```
public class ShopManagerAction extends ActionSupport {
	// 注入接口属性
	private ShopService shopService;
	// 门店名称
	private String shopName;
	// 地址
	private String address;
	// 邮编
	private String zipCode;
	// 电话
	private String telNumber;
	
		public void setShopService(ShopService shopService) {
		this.shopService = shopService;
	}

	public void setShopName(String shopName) {
		this.shopName = shopName;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	public void setZipCode(String zipCode) {
		this.zipCode = zipCode;
	}

	public void setTelNumber(String telNumber) {
		this.telNumber = telNumber;
	}

	/**
	 * 跳转到添加门店界面
	 * 
	 * @return
	 * @throws Exception
	 */
	public String forwardToAddShop() throws Exception {
		return SUCCESS;
	}

	/**
	 * 添加门店
	 * 
	 * @return
	 * @throws Exception
	 */
	public String addShop() throws Exception {
    
    	Shop shop = new Shop();
		shop.setShopName(shopName);
		shop.setAddress(address);
		shop.setZipCode(zipCode);
		shop.setTelNumber(telNumber);
	
		boolean result = shopService.addShop(shop);
		Confirm confirm = new Confirm();
		if (result) {
			confirm.setIsSuccess("right");
			confirm.setMessage("门店添加成功");
			confirm.setUrl("shopList.action");
			confirm.setRetName("门店列表");
		} else {
			confirm.setIsSuccess("error");
			confirm.setMessage("门店添加失败");
			confirm.setUrl("shopList.action");
			confirm.setRetName("门店列表");
		}
		ActionContext.getContext().put("confirm", confirm);
		return SUCCESS;
	}
}

```

`/pages/shop/addShop.jsp`中

```
		<form id="myForm" action="addShop.action" method="post">
					<table>
						<tr>
							<td>
								门店名称：
							</td>
							<td>
								<input type="text" name="shopName" />
							</td>
						</tr>
						<tr>
							<td >
								地址：
							</td>
							<td >
								<input type="text" name="address" />
							</td>
						</tr>
						<tr>
							<td >
								邮编：
							</td>
							<td >
								<input type="text" name="zipCode" />
							</td>
						</tr>
						<tr>
							<td>
								电话：
							</td>
							<td>
								<input type="text" name="telNumber" />
							</td>
						</tr>
						<tr>
							<td colspan="2" >
								<input type="submit" name="保存" />
								<input type="reset" name="重置" />
							</td>
						</tr>
					</table>			
		</form>
```

###改进为 Struts 在 action 和 view 直接传递 model

在`struts.xml`中

```
		<action name="forwardToAddShop" class="com.company.mis.system.action.shop.ShopManagerAction"
			method="forwardToAddShop">
			<result name="success">/pages/shop/addShop.jsp</result>
		</action>
		<action name="addShop" class="com.company.mis.system.action.shop.ShopManagerAction" method="addShop">
			<result name="success" type="chain">
				<param name="actionName">confirm</param>
				<param name="namespace">/</param>
			</result>
		</action>
```

`ShopmanagerAction.java`

```
public class ShopManagerAction extends ActionSupport {
	// 注入接口属性
	private ShopService shopService;

	// Model PO对象
	private Shop shop;

	public void setShopService(ShopService shopService) {
		this.shopService = shopService;
	}

	// 往view传递时用
	public Shop getShop() {
		return shop;
	}

	// 从view获取时用
	public void setShop(Shop shop) {
		this.shop = shop;
	}


	/**
	 * 跳转到添加门店界面
	 * 
	 * @return
	 * @throws Exception
	 */
	public String forwardToAddShop() throws Exception {
		return SUCCESS;
	}

	/**
	 * 添加门店
	 * 
	 * @return
	 * @throws Exception
	 */
	public String addShop() throws Exception {
    
    	// 这里已经自动从view获得shop值了,简化了不少

		boolean result = shopService.addShop(shop);
		Confirm confirm = new Confirm();
		if (result) {
			confirm.setIsSuccess("right");
			confirm.setMessage("门店添加成功");
			confirm.setUrl("shopList.action");
			confirm.setRetName("门店列表");
		} else {
			confirm.setIsSuccess("error");
			confirm.setMessage("门店添加失败");
			confirm.setUrl("shopList.action");
			confirm.setRetName("门店列表");
		}
		ActionContext.getContext().put("confirm", confirm);
		return SUCCESS;
	}
}

```

`/pages/shop/addShop.jsp`中

```
		<form id="myForm" action="addShop.action" method="post">
					<table>
						<tr>
							<td>
								门店名称：
							</td>
							<td>
								<input type="text" name="shop.shopName" />
							</td>
						</tr>
						<tr>
							<td >
								地址：
							</td>
							<td >
								<input type="text" name="shop.address" />
							</td>
						</tr>
						<tr>
							<td >
								邮编：
							</td>
							<td >
								<input type="text" name="shop.zipCode" />
							</td>
						</tr>
						<tr>
							<td>
								电话：
							</td>
							<td>
								<input type="text" name="shop.telNumber" />
							</td>
						</tr>
						<tr>
							<td colspan="2" >
								<input type="submit" name="保存" />
								<input type="reset" name="重置" />
							</td>
						</tr>
					</table>			
		</form>
```

