# YunGouOS-PAY-SDK

![https://yungouos.oss-cn-shanghai.aliyuncs.com/YunGouOS/logo/merchant/logo.png](https://yungouos.oss-cn-shanghai.aliyuncs.com/YunGouOS/logo/merchant/logo.png)


# 关于我们

YunGouOS微信支付官方合作伙伴,YunGouOS-PAY是徐州市云宝网络科技有限公司研发的支付产品。

过去我们只将支付提供给自身系统使用，我们对市面上各种第四方支付深感痛恨 我们深知一些个人用户对支付的渴望。

为此，我们开放了重量级产品，YunGouOS旗下“微信个人支付”正式对外开放。

为更多开发者、个体户、个人创业者、小公司提供正规的个人支付产品支持个人、个体户、企业申请签约，资金由微信结算。

# 用户疑惑

很多用户对官方个人支付持有怀疑等态度，表示理解，过去市场上出现了大量的所谓个人支付 基本采取以下几种方式：

第一种 普通的微信号的收款码。这个不支持信用卡支付、受限于个人账户20万每年的限额、同时官方对这方面不提供接口回调。

有些第三方通过外挂挂机等形式完成 稳定性不高。

第二种 二次清算，某企业与微信签约。与使用者二次清算（ps：其中风险自行衡量）

我们提供的则与官网相差无异，我们是微信支付合作伙伴，拥有直接开户权限，支持个人开户、个体户开户、企业开户。

我们并非使用某种外挂等形式完成支付，与普通企业申请一样，提交资料-》微信审核-》审核通过后微信下发商户号-》根据官网文档完成API对接

个人开户支持接口：扫码支付、JSAPI支付、付款码支付、小程序支付、查询、退款等微信官方接口。单日限额30万。

个体户开户支持接口：扫码支付、JSAPI支付、付款码支付、查询、退款等微信官方接口。可使用微信官方营销产品。无限额。

企业开户支持接口：扫码支付、JSAPI支付、付款码支付、小程序支付、APP支付。可使用所有微信官方营销产品。无限额。

# 流程图

![开户流程](https://yungouos.oss-cn-shanghai.aliyuncs.com/YunGouOS/merchant/images/step.png)

# 如何使用

在官网提交资料，由微信审核，审核通过后下发商户号，对接使用。

# 相关地址

官网地址：[http://merchant.yungouos.com](http://merchant.yungouos.com "http://merchant.yungouos.com")

接口文档：[http://open.pay.yungouos.com](http://open.pay.yungouos.com "http://open.pay.yungouos.com")


# 小程序支付

针对小程序支付，我们在文档中使用大量的示例代码以及开源了一个小程序支付集成的Demo，希望帮助小程序开发者快速接入。

同时我们针对小程序也开发了小程序端的SDK，详情请看小程序内的SDK说明。只需要简单几句代码，即可接入。

小程序支付文档地址：[http://open.pay.yungouos.com/#/api/api/pay/wxpay/minPay](http://open.pay.yungouos.com/#/api/api/pay/wxpay/minPay "http://open.pay.yungouos.com/#/api/api/pay/wxpay/minPay")

小程序SDK地址：[https://gitee.com/YunGouOS/YunGouOS-PAY-SDK/tree/master/YunGouOS-WxApp-SDK](https://gitee.com/YunGouOS/YunGouOS-PAY-SDK/tree/master/YunGouOS-WxApp-SDK "https://gitee.com/YunGouOS/YunGouOS-PAY-SDK/tree/master/YunGouOS-WxApp-SDK")


## 在线体验

![https://yungouos.oss-cn-shanghai.aliyuncs.com/YunGouOS/merchant/mindemo/minapp.jpg](https://yungouos.oss-cn-shanghai.aliyuncs.com/YunGouOS/merchant/mindemo/minapp.jpg)

# 快速开始

第一步：下载小程序支付SDK

第二步：在 [http://merchant.yungouos.com](http://merchant.yungouos.com "http://merchant.yungouos.com") 微信支付->我的支付->开通小程序支付权限（如果您还未申请，通过 申请支付菜单在线申请）

第三步：修改 project.config.json 文件中的appid改成您自己的小程序

第四步：修改 /wxpay/config.js 中的微信支付商户号与商户密钥

第五步：预览或真机调试体验


# 已有项目如何快速集成？

第一步：在你的小程序app.json文件中 添加 “支付收银” 小程序的appid为：wxd9634afb01b983c0

	"navigateToMiniProgramAppIdList": [
        "wxd9634afb01b983c0"
     ]

第二步：复制SDK中的 wxpay文件夹到您项目的根目录，与pages目录同级，修改config.js中的微信支付参数

第三步：在您需要支付的方法中增加：

	wxPayUtil.toPay(out_trade_no,total_fee,body,notify_url,attach,title,(response)=>{
	      console.log(response);
		  //您的业务	
    });
		
这样您的小程序就已经可以打开支付，进行支付了。但是为了进一步优化体验我们还需要在编写支付结束后返回的一些事情处理。请继续往下看

第四步：在您的app.js文件中的onShow方法下 增加如下代码（注意此处需要在onShow方法中增加参数，修改后为 onShow(options) ）
	
    let extraData=options.referrerInfo.extraData;
    if(extraData){
      //不管成功失败 先把支付结果赋值
      this.globalData.payStatus=extraData.code==0?true:false;
      if(extraData.code!=0){
        wx.showToast({
          title: extraData.msg,//错误提示
          icon: 'none',
          duration: 3000
        });
        return;
      }
      //支付成功
      this.globalData.orderNo=extraData.data.orderNo;
    }

需要在 globalData里面增加2个属性，当然这个不是固定的您可以按需处理。主要原理就是把支付结果存储到这里面，然后在页面上通过app.globalData来获取数据继续使用 代码如下
	
	globalData: {
		payStatus:null,//支付状态
		orderNo:null//订单号
	}


第五步：我们可能需要在支付页面在做一些事情，比如支付成功进行页面跳转。在您支付页面的 onShow 方法中增加如下代码

	//支付完成返回，开始处理数据
     if(app.globalData.payStatus!=null&&app.globalData.payStatus!=undefined){
      let orderno=app.globalData.orderNo;
      console.log('接收到返回支付结果',app.globalData.payStatus);
      console.log('订单号',orderno);
      //处理您自己的业务
    }


