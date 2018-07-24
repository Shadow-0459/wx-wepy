# 起始
## 代码构成
小程序配置app.json：

1. pages字段 —— 用于描述当前小程序所有页面路径，这是为了让微信客户端知道当前你的小程序页面定义在哪个目录。
1. window字段 —— 小程序所有页面的顶部背景颜色，文字颜色定义在这里的。

工具配置 project.config.json

页面配置 page.json

WXML 模板 

	转 div p span 一类 为 view, button, text 一类
	不要再让 JS 直接操控 DOM，JS只需要管理状态即可，然后再通过一种模板语法来描述状态和界面结构的关系即可。

WXSS 样式

	新增 rpx 底层换算，浮点数，有偏差

JS 交互逻辑

	this 控制 view标签：
	<view>{{ msg }}</view>
	<button bindtap="clickMe">点击我</button>
	Page({
	  clickMe: function() {
	    this.setData({ msg: "Hello World" })
	  }
	})

# 框架

## 结构
组成：框架 + 组件 + 内置方法（wx.）

Page 是一个页面构造器
### 逻辑层
### 视图层

数据渲染 {{}}
列表渲染 wx:for
条件渲染 wx:if 有更高的切换消耗而 hidden 有更高的初始渲染消耗。因此，如果需要频繁切换的情景下，用 hidden 更好，如果在运行时条件不大可能改变则 wx:if 较好。

#### 模版
	定义： <template>
	使用： is="" data="{{...xxx}}"（可以使用选择语句判断使用哪个模版）
#### 事件
	绑定函数
	冒泡事件/非冒泡事件：触发键盘/鼠标 是否向父节点移动
	事件对象： 逻辑层绑定该事件的处理函数会收到一个事件对象。
#### 引用
	import
		import 有作用域的概念，即只会 import 目标文件中定义的 template，而不会 import 目标文件 import 的 template。
	include
		include 可以将目标文件除了 <template/> <wxs/> 外的整个代码引入，相当于是拷贝到 include 位置
		
#### WXS
WXS 代码可以编写在 wxml 文件中的`<wxs>` 标签内，或以 .wxs 为后缀名的文件内。

	wxs 与 javascript 是不同的语言，有自己的语法，并不和 javascript 一致。
	wxs 的运行环境和其他 javascript 代码是隔离的，wxs 中不能调用其他 javascript 文件中定义的函数，也不能调用小程序提供的API。
	wxs 函数不能作为组件的事件回调。
	由于运行环境的差异，在 iOS 设备上小程序内的 wxs 会比 javascript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。
	
* require 函数。只能引用 .wxs 文件模块，且必须使用相对路径。
* src 属性可以用来引用其他的 wxs 文件模块。
* module 属性是当前 `<wxs>` 标签的模块名。
* 每个 wxs 模块均有一个内置的 module 对象。

使用 `<include>` 或 `<import>` 时，`<wxs>` 模块不会被引入到对应的 WXML 文件中。
		   
	注释用 /* */	

数据类型：

	number ： 数值
	string ：字符串
	boolean：布尔值
	object：对象
	function：函数
	array : 数组
	date：日期
	regexp：正则

### WXSS
rpx（responsive pixel）: 可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。	

使用@import语句可以导入外联样式表，@import后跟需要导入的外联样式表的相对路径，用;表示语句结束。
内联样式:

	style：静态的样式统一写到 class 中。style 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 style 中，以免影响渲染速度。
	class：用于指定样式规则，其属性值是样式规则中类选择器名(样式类名)的集合，样式类名不需要带上.，样式类名之间用空格分隔。

## 插件
#### 开发插件
#### 使用插件

需要跳转到插件页面时， url 应使用 plugin:// 前缀。

代码示例：

	<navigator url="plugin://myPlugin/hello-page">
	  Go to pages/hello-page!
	</navigator>
#### 插件的限制
只能使用部分API
#### 插件功能页
## 分包加载
某些情况下，开发者需要将小程序划分成不同的子包，在构建时打包成不同的分包，用户在使用时按需进行加载。

在构建小程序分包项目时，构建会输出一个或多个功能的分包，其中每个分包小程序必定含有一个主包，所谓的主包，即放置默认启动页面/TabBar 页面，以及一些所有分包都需用到公共资源/JS 脚本，而分包则是根据开发者的配置进行划分。

## 多线程 Worker
## 基础库
与客户端之间的关系：

	小程序的能力需要微信客户端来支撑，每一个基础库都只能在对应的客户端版本上运行，高版本的基础库无法兼容低版本的微信客户端。
## 兼容


小程序的功能不断的增加，但是旧版本的微信客户端并不支持新功能，所以在使用这些新能力的时候需要做兼容。
微信客户端和小程序基础库的版本号风格为 Major.Minor.Patch（主版本号.次版本号.修订号）。 开发者可以根据版本号去做兼容
方式：接口，参数，组件


## 运行机制
小程序启动会有两种情况，一种是「冷启动」，一种是「热启动」。 假如用户已经打开过某小程序，然后在一定时间内再次打开该小程序，此时无需重新启动，只需将后台态的小程序切换到前台，这个过程就是热启动；冷启动指的是用户首次打开或小程序被微信主动销毁后再次打开的情况，此时小程序需要重新加载启动。

运行机制：

	当小程序进入后台，客户端会维持一段时间的运行状态，超过一定时间后（目前是5分钟）会被微信主动销毁
	当短时间内（5s）连续收到两次以上收到系统内存告警，会进行小程序的销毁
## 性能优化
setData：

	setData 是小程序开发中使用最频繁的接口，也是最容易引发性能问题的接口。在介绍常见的错误用法前，先简单介绍一下 setData 背后的工作原理。
	
工作原理：用户传输的数据，需要将其转换为字符串形式传递，同时把转换后的数据内容拼接成一份 JS 脚本，再通过执行 JS 脚本的形式传递到两边独立环境。

工具：Trace工具

#组件

#### 视图容器：
view scroll-view swiper(滑块试图容器) movable-view（可移动的试图容器） cover-view（覆盖在原组件的试图容器）
#### 基础内容：
icon text rich-text（富文本） progress（进度条）
#### 表单组件：
button checkbox form input label picker（底部弹出滚动选择） picker-view（嵌入页面的滚动选择器） radio（group） slider（滑动选择器） switch（开关选择器） textarea
#### 导航：
navigator（页面链接） functional-page-navigator（跳转插件功能页）
#### 媒体组件：
audio（1.6.0 版本开始，该组件不再维护，替换为wx.createInnerAudioContext 接口）
image video camera（需要授权） live-player（实时音视频播放） live-pusher（实时音视频录制）
#### 地图
map：markers标记 callout气泡 position cover-view地图空间 circles地图上的圆 polyline指定一系列坐标点
#### 画布
cnavas （相关api：wx.createCanvasContext）
#### 开放能力
* open-data 展示微信开放的数据
* web-view 用来承载网页的容器
* ad 广告（邀请制开放）

## 自定义组件
* 注意：在组件wxss中不应使用ID选择器、属性选择器和标签名选择器。
* 自定义组件由 json wxml wxss js 4个文件组成
* 在组件模板中可以提供一个 <slot> 节点，用于承载组件引用时提供的子节点。

#### component 构造器
#### 组件通信
* WXML 数据绑定
* 事件
* 父组件还可以通过 this.selectComponent 方法获取子组件实例对象，这样就可以直接访问组件的任意数据和方法。

监听事件 bind：
触发事件 ：自定义组件触发事件时，需要使用 triggerEvent 方法，指定事件名、detail对象和事件选项：
#### behaviors
代码共享 behavior 需要使用 Behavior() 构造器定义。

#### 组件间关系
有相互间的关系，相互间的通信往往比较复杂。此时在组件定义时加入 relations 定义段，可以解决这样的问题。

可使用这个behavior来代替组件路径作为关联的目标节点

relations 定义段包含目标组件路径及其对应选项
#### 抽象节点
自定义组件模版中的一些节点，其对应的自定义组件不是由自定义组件本身确定的，而是自定义组件的调用者确定的。这时可以把这个节点声明为“抽象节点”。

抽象节点可以指定一个默认组件，当具体组件未被指定时，将创建默认组件的实例。默认组件可以在 componentGenerics 字段中指定

# API

#### 网络 
* 发起请求
	* wx.request(OBJECT) 发起网络请求
* 上传下载
	* wx.uploadFile(OBJECT) 上传到开发者服务器
	* wx.downloadFile(OBJECT) 下载文件资源到本地
* WebSocket
	* wx.connectSocket(OBJECT) 创建一个 WebSocket 连接
	* wx.onSocketOpen(CALLBACK) 监听WebSocket连接打开事件
	* wx.onSocketError(CALLBACK) 监听WebSocket错误
	* wx.sendSocketMessage(OBJECT) 通过 WebSocket 连接发送数据
	* wx.onSocketClose(CALLBACK) 监听WebSocket关闭
	* SocketTask WebSocket 任务

#### 媒体
* 图片
	* wx.chooseImage(OBJECT) 从本地相册选择图片或使用相机拍照
	* wx.previewImage(OBJECT) 预览图片
	* wx.getImageInfo(OBJECT) 获取图片信息，倘若为网络图片，需先配置download域名才能生效

	* wx.saveImageToPhotosAlbum(OBJECT) 保存图片到系统相册
* 录音
	* wx.startRecord(OBJECT) 开始录音
	* wx.stopRecord() 主动调用停止录音
* 录音管理
	* wx.getRecorderManager() 获取全局唯一的录音管理器 recorderManager
* 音频播放控制
	* wx.playVoice(OBJECT) 开始播放语音（唯一播放）
	* wx.pauseVoice() 暂停正在播放的语音
	* wx.stopVoice() 结束播放语音
* 音乐播放控制
	* wx.getBackgroundAudioPlayerState(OBJECT) 获取后台音乐播放状态。
	* wx.playBackgroundAudio(OBJECT) 使用后台播放器播放音乐
	* wx.pauseBackgroundAudio() 暂停播放音乐
	* wx.seekBackgroundAudio(OBJECT) 控制音乐播放进度
	* wx.stopBackgroundAudio() 停止播放音乐
	* wx.onBackgroundAudioPlay(CALLBACK) 监听音乐播放
	* wx.onBackgroundAudioPause(CALLBACK) 监听音乐暂停
	* wx.onBackgroundAudioStop(CALLBACK) 监听音乐停止
* 背景音频播放管理
	* wx.getBackgroundAudioManager() 获取全局唯一的背景音频管理器 backgroundAudioManager
* 音频组件控制
	* wx.createAudioContext(audioId, this) 创建并返回 audio 上下文 audioContext 对象
	* wx.createInnerAudioContext() 创建并返回内部 audio 上下文 innerAudioContext 对象
	* wx.getAvailableAudioSources(OBJECT) 获取当前支持的音频输入源
* 视频
	* wx.chooseVideo(OBJECT) 拍摄视频或从手机相册中选视频，返回视频的临时文件路径
	* wx.saveVideoToPhotosAlbum(OBJECT) 保存视频到系统相册
* 视频组件控制
	* wx.createVideoContext(videoId, this) 创建并返回 video 上下文 videoContext 对象
* 相机组件控制
	* wx.createCameraContext(this) 创建并返回 camera 上下文 cameraContext 对象
* 实时音视频
	* wx.createLivePlayerContext(domId, this) 操作对应的 <live-player/> 组件
	* wx.createLivePusherContext() 创建并返回 live-pusher 上下文 LivePusherContext 对象
* 动态加载字体
	* wx.loadFontFace(OBJECT) 动态加载网络字体

#### 文件

* wx.saveFile(OBJECT) 保存文件到本地
* wx.getFileInfo(OBJECT) 获取文件信息
* wx.getSavedFileList(OBJECT) 获取本地已保存的文件列表
* wx.getSavedFileInfo(OBJECT) 获取本地文件的文件信息
* wx.removeSavedFile(OBJECT) 删除本地存储的文件
* wx.openDocument(OBJECT) 新开页面打开文档，支持格式：doc, xls, ppt, pdf, docx, xlsx, pptx

#### 数据缓存
* wx.setStorage(OBJECT) 将数据存储在本地缓存中指定的 key 中 异步
* wx.setStorageSync(KEY,DATA) 将 data 存储在本地缓存中指定的 key 中 同步
* wx.getStorage(OBJECT) 从本地缓存中异步获取指定 key 对应的内容
* wx.getStorageSync(KEY) 从本地缓存中同步获取指定 key 对应的内容
* wx.getStorageInfo(OBJECT) 异步获取当前storage的相关信息
* wx.getStorageInfoSync 同步获取当前storage的相关信息
* wx.removeStorage(OBJECT) 从本地缓存中异步移除指定 key 
* wx.removeStorageSync(KEY) 从本地缓存中同步移除指定 key 
* wx.clearStorage() 清理本地数据缓存
* wx.clearStorageSync() 同步清理本地数据缓存

#### 位置
* 获取位置
	* wx.getLocation(OBJECT) 获取当前的地理位置、速度
	* wx.chooseLocation(OBJECT) 打开地图选择位置
* 查看位置
	* wx.openLocation(OBJECT) 使用微信内置地图查看位置
* 地图组件控制
	* wx.createMapContext(mapId, this) 创建并返回 map 上下文 mapContext 对象

#### 设备
* 系统信息
	* wx.getSystemInfo(OBJECT) 获取系统信息
	* wx.getSystemInfoSync() 获取系统信息同步接口
	* wx.canIUse(String) 判断小程序的API，回调，参数，组件等是否在当前版本可用
	* 内存
	* wx.onMemoryWarning(callback) 监听内存不足的告警事件
* 网络状态
	* wx.getNetworkType(OBJECT) 获取网络类型
	* wx.onNetworkStatusChange(CALLBACK) 监听网络状态变化
* 加速度计
	* wx.onAccelerometerChange(CALLBACK) 监听加速度数据
	* wx.startAccelerometer(OBJECT) 开始监听加速度数据
	* wx.stopAccelerometer(OBJECT) 停止监听加速度数据
* 罗盘
	* wx.onCompassChange(CALLBACK) 监听罗盘数据
	* wx.startCompass(OBJECT) 开始监听罗盘数据
	* wx.stopCompass(OBJECT) 停止监听罗盘数据
* 拨打电话
	* wx.makePhoneCall(OBJECT) 打电话
* 扫码
	* wx.scanCode(OBJECT) 调起客户端扫码界面，扫码成功后返回对应的结果
* 剪贴板
	* wx.setClipboardData(OBJECT) 设置系统剪贴板的内容
	* wx.getClipboardData(OBJECT) 获取系统剪贴板内容
* 蓝牙
	* wx.openBluetoothAdapter(OBJECT) 初始化小程序蓝牙模块
	* wx.closeBluetoothAdapter(OBJECT) 关闭蓝牙模块，使其进入未初始化状态
	* wx.getBluetoothAdapterState(OBJECT) 获取本机蓝牙适配器状态
	* wx.onBluetoothAdapterStateChange(CALLBACK) 监听蓝牙适配器状态变化事件
	* wx.startBluetoothDevicesDiscovery(OBJECT) 开始搜寻附近的蓝牙外围设备
	* wx.stopBluetoothDevicesDiscovery(OBJECT) 停止搜寻附近的蓝牙外围设备
	* wx.getBluetoothDevices(OBJECT) 获取在小程序蓝牙模块生效期间所有已发现的蓝牙设备
	* wx.getConnectedBluetoothDevices(OBJECT) 根据 uuid 获取处于已连接状态的设备
	* wx.createBLEConnection(OBJECT) 连接低功耗蓝牙设备
	* wx.closeBLEConnection(OBJECT) 断开与低功耗蓝牙设备的连接
	* wx.getBLEDeviceServices(OBJECT) 获取蓝牙设备所有 service（服务)
	* wx.getBLEDeviceCharacteristics(OBJECT) 获取蓝牙设备某个服务中的所有 characteristic（特征值）
	* wx.readBLECharacteristicValue(OBJECT) 读取低功耗蓝牙设备的特征值的二进制数据值
	* wx.writeBLECharacteristicValue(OBJECT) 向低功耗蓝牙设备特征值中写入二进制数据
	* wx.notifyBLECharacteristicValueChange(OBJECT) 启用低功耗蓝牙设备特征值变化时的 notify 功能，订阅特征值
	* wx.onBLEConnectionStateChange(CALLBACK) 监听低功耗蓝牙连接状态的改变事件
	* wx.onBLECharacteristicValueChange(CALLBACK) 监听低功耗蓝牙设备的特征值变化
* iBeancon
	* wx.startBeaconDiscovery(OBJECT) 开始搜索附近的iBeacon设备
	* wx.stopBeaconDiscovery(OBJECT) 停止搜索附近的iBeacon设备
	* wx.getBeacons(OBJECT) 获取所有已搜索到的iBeacon设备
	* wx.onBeaconUpdate(CALLBACK) 监听 iBeacon 设备的更新事件
	* wx.onBeaconServiceChange(CALLBACK) 监听 iBeacon 服务的状态变化
* 屏幕亮度
	* wx.setScreenBrightness(OBJECT) 设置屏幕亮度
	* wx.getScreenBrightness(OBJECT) 获取屏幕亮度
	* wx.setKeepScreenOn(OBJECT) 设置是否保持常亮状态
* 用户截屏
	* wx.onUserCaptureScreen(CALLBACK) 监听用户主动截屏事件
* 振动
	* wx.vibrateLong(OBJECT) 使手机发生较长时间的振动（400ms）
	* wx.vibrateShort(OBJECT) 使手机发生较短时间的振动（15ms）
* 手机联系人
	* wx.addPhoneContact(OBJECT) 手机通讯录联系人和联系方式的增加
* NFC	
	* wx.getHCEState(OBJECT) 判断当前设备是否支持 HCE 能力
	* wx.startHCE(OBJECT) 初始化 NFC 模块
	* wx.stopHCE(OBJECT) 关闭 NFC 模块
	* wx.onHCEMessage(CALLBACK) 监听 NFC 设备的消息回调，并在回调中处理
	* wx.sendHCEMessage(OBJECT) 发送 NFC 消息
* Wi-Fi
	* wx.startWifi(OBJECT) 初始化 Wi-Fi 模块
	* wx.stopWifi(OBJECT) 关闭 Wi-Fi 模块
	* wx.connectWifi(OBJECT) 连接 Wi-Fi
	* wx.getWifiList(OBJECT) 请求获取 Wi-Fi 列表
	* wx.onGetWifiList(CALLBACK) 监听在获取到 Wi-Fi 列表数据时的事件 在回调中将返回 wifiList
	* wx.setWifiList(OBJECT) 利用接口设置 wifiList 中 AP 的相关信息
	* wx.onWifiConnected(CALLBACK) 监听连接上 Wi-Fi 的事件
	* wx.getConnectedWifi(OBJECT) 获取已连接中的 Wi-Fi 信息

#### 界面
* 交互反馈
	* wx.showToast(OBJECT) 显示消息提示框
	* wx.showLoading(OBJECT) 显示loading提示框
	* wx.hideToast() 隐藏消息提示框
	* wx.hideLoading() 隐藏loading提示框
	* wx.showModal(OBJECT) 显示模态弹窗
	* wx.showActionSheet(OBJECT) 显示操作菜单
* 设置导航条
	* wx.setNavigationBarTitle(OBJECT) 动态设置当前页面的标题
	* wx.showNavigationBarLoading() 在当前页面显示导航条加载动画
	* wx.hideNavigationBarLoading() 隐藏导航条加载动画
	* wx.setNavigationBarColor(OBJECT) 设置导航条
* 设置tabBar
	* wx.setTabBarBadge(OBJECT) 为 tabBar 某一项的右上角添加文本
	* wx.removeTabBarBadge(OBJECT) 移除 tabBar 某一项右上角的文本
	* wx.showTabBarRedDot(OBJECT) 显示 tabBar 某一项的右上角的红点
	* wx.hideTabBarRedDot(OBJECT) 隐藏 tabBar 某一项的右上角的红点
	* wx.setTabBarStyle(OBJECT) 动态设置 tabBar 的整体样式
	* wx.setTabBarItem(OBJECT) 动态设置 tabBar 某一项的内容
	* wx.showTabBar(OBJECT) 显示 tabBar
	* wx.hideTabBar(OBJECT) 隐藏 tabBar
* 设置窗口背景
	* wx.setBackgroundColor(OBJECT) 动态设置窗口的背景色
	* wx.setBackgroundTextStyle(OBJECT) 动态设置下拉背景字体、loading 图的样式
* 设置置顶信息
	* wx.setTopBarText(OBJECT) 动态设置置顶栏文字内容
* 导航
	* wx.navigateTo(OBJECT) 保留当前页面，跳转到应用内的某个页面
	* wx.redirectTo(OBJECT) 关闭当前页面，跳转到应用内的某个页面
	* wx.switchTab(OBJECT) 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面
	* wx.navigateBack(OBJECT) 关闭当前页面，返回上一页面或多级页面
	* wx.reLaunch(OBJECT) 关闭所有页面，打开到应用内的某个页面
* 动画
	* wx.createAnimation(OBJECT) 创建一个动画实例animation
* 位置
	* wx.pageScrollTo(OBJECT) 将页面滚动到目标位置
* 在Canvas上画图 绘图
	* intro 画图
	* coordinates 坐标系
	* gradient 渐变
	* reference API接口
	* color 颜色
	* wx.createCanvasContext(canvasId, this) 创建 canvas 绘图上下文
	* wx.createContext (不推荐使用) 创建并返回绘图上下文
	* wx.drawCanvas (不推荐使用) 用所提供的 actions 在所给的 canvas-id 对应的 canvas 上进行绘图
	* wx.canvasToTempFilePath(OBJECT, this) 把当前画布指定区域的内容导出生成指定大小的图片，并返回文件路径
	* wx.canvasGetImageData(OBJECT, this） 返回一个数组，用来描述 canvas 区域隐含的像素数据
	* wx.canvasPutImageData(OBJECT, this) 将像素数据绘制到画布的方法
	* canvasContext.setFillStyle 设置填充色
	* canvasContext.setStrokeStyle 设置边框颜色
	* canvasContext.setShadow 设置阴影样式
	* canvasContext.createLinearGradient 创建一个线性的渐变颜色
	* canvasContext.createCircularGradient 创建一个圆形的渐变颜色
	* canvasContext.addColorStop 创建一个颜色的渐变点
	* canvasContext.setLineWidth 设置线条的宽度
	* canvasContext.setLineCap 设置线条的端点样式
	* canvasContext.setLineJoin 设置线条的交点样式
	* canvasContext.setLineDash 设置虚线样式的方法
	* canvasContext.setMiterLimit 设置最大斜接长度
	* canvasContext.rect  创建一个矩形
	* canvasContext.fillRect 填充一个矩形
	* canvasContext.strokeRect 画一个矩形
	* canvasContext.clearRect 清除画布上在该矩形区域内的内容
	* canvasContext.fill 对当前路径中的内容进行填充
	* canvasContext.stroke 画出当前路径的边框
	* canvasContext.beginPath 开始创建一个路径
	* canvasContext.closePath 关闭一个路径
	* canvasContext.moveTo 把路径移动到画布中的指定点，不创建线条
	* canvasContext.lineTo 增加一个新点
	* canvasContext.arc  画一条弧线
	* canvasContext.bezierCurveTo 创建三次方贝塞尔曲线路径
	* canvasContext.quadraticCurveTo  创建二次贝塞尔曲线路径
	* canvasContext.scale 倍数会相乘/缩放
	* canvasContext.translate 对当前坐标系的原点(0, 0)进行变换，默认的坐标系原点为页面左上角
	* canvasContext.clip clip() 方法从原始画布中剪切任意形状和尺寸
	* canvasContext.setFontSize 设置字体的字号
	* canvasContext.fillText 填充的文本
	* canvasContext.setTextAlign 设置文字的对齐
	* canvasContext.setTextBaseline 设置文字的水平对齐
	* canvasContext.drawImage 绘制图像到画布
	* canvasContext.setGlobalAlpha 设置全局画笔透明度
	* canvasContext.save 保存当前的绘图上下文
	* restore 恢复之前保存的绘图上下文
	* canvasContext.draw 将之前在绘图上下文中的描述画在canvas中
	* canvasContext.measureText 测量文本尺寸信息
	* canvasContext.globalCompositeOperation 该属性是设置要在绘制新形状时应用的合成操作的类型
	* canvasContext.arcTo 根据控制点和半径绘制圆弧路径
	* canvasContext.strokeText 给定的 (x, y) 位置绘制文本描边的方法
	* canvasContext.lineDashOffset 设置虚线偏移量的属性
	* canvasContext.createPattern 对指定的图像创建模式的方法，可在指定的方向上重复元图像
	* canvasContext.shadowBlur 设置阴影的模糊级别
	* canvasContext.shadowColor 设置阴影的颜色
	* canvasContext.shadowOffsetX 设置阴影相对于形状在水平方向的偏移
	* canvasContext.shadowOffsetY 设置阴影相对于形状在竖直方向的偏移
	* canvasContext.font 设置当前字体样式的属性
	* canvasContext.transform 使用矩阵多次叠加当前变换的方法
	* canvasContext.setTransform 使用矩阵重新设置（覆盖）当前变换的方法
* 下拉更新
	* onPullDownRefresh 监听该页面用户下拉刷新事件
	* wx.startPullDownRefresh(OBJECT) 开始下拉刷新
	* wx.stopPullDownRefresh() 停止当前页面下拉刷新
* WXML节点信息
	* wx.createSelectorQuery() 返回一个SelectorQuery对象实例
	* selectorQuery.in(component) 将选择器的选取范围更改为自定义组件component内
	* selectorQuery.select(selector) 获取节点信息
	* selectorQuery.selectAll(selector) 在当前页面下选择匹配选择器selector的节点，返回一个NodesRef对象实例
	* selectorQuery.selectViewport() 选择显示区域，可用于获取显示区域的尺寸、滚动位置等信息，返回一个NodesRef对象实例
	* nodesRef.boundingClientRect([callback]) 添加节点的布局位置的查询请求，相对于显示区域，以像素为单位
	* nodesRef.scrollOffset([callback]) 添加节点的滚动位置查询请求，以像素为单位
	* nodesRef.fields(fields, [callback]) 获取节点的相关信息，需要获取的字段在fields中指定
	* selectorQuery.exec([callback]) 执行所有的请求，请求结果按请求次序构成数组，在callback的第一个参数中返回
* WXML节点布局相交状态
	* wx.createIntersectionObserver([this], [options]) 创建并返回一个 IntersectionObserver 对象实例
	* intersectionObserver.relativeTo(selector, [margins]) 使用选择器指定一个节点，作为参照区域之
	* intersectionObserver.relativeToViewport([margins]) 指定页面显示区域作为参照区域之一
	* intersectionObserver.observe(targetSelector, callback) 指定目标节点并开始监听相交状态变化情况
	* intersectionObserver.disconnect() 停止监听。回调函数将不再触发

#### 第三方平台

* wx.getExtConfig(OBJECT) 获取第三方平台自定义的数据字段
* wx.getExtConfigSync() 获取第三方平台自定义的数据字段的同步接口

#### 开放接口
* 登录
	* wx.login(OBJECT) 获取临时登录凭证（code）
	* wx.checkSession(OBJECT) 校验用户当前session_key是否有效
	* 签名加密
* 授权
	* wx.authorize(OBJECT) 提前向用户发起授权请求
* 用户信息
	* wx.getUserInfo(OBJECT) 
		* 注意：此接口有调整，使用该接口将不再出现授权弹窗，请使用 <button open-type="getUserInfo"></button> 引导用户主动进行授权操作
		* 当用户未授权过，调用该接口将直接报错
		* 当用户授权过，可以使用该接口获取用户信息 
	* getPhoneNumber(OBJECT) 获取微信用户绑定的手机号，需先调用login接口
	* UnionID机制说明
* 微信支付
	* wx.requestPayment(OBJECT) 发起微信支付
* 接口调研凭证
	* 获取 access_token
* **模版消息**
	* 使用说明
	* 模版消息管理
	* 发送模板消息
* 客服消息
	* 接收消息和事件
		* 文本消息
		* 图片消息
		* 小程序卡片消息
		* 进入会话事件
	* 发送客服消息、
	* 转发消息
	* 临时素材接口
		* 获取临时素材
		* 新增临时素材
	* 客服输入状态
	* 接入指引
* 转发
	* onShareAppMessage(options) 
		* 只有定义了此事件处理函数，右上角菜单才会显示 “转发” 按钮
		* 用户点击转发按钮的时候会调用
		* 此事件需要 return 一个 Object，用于自定义转发内容 
	* wx.showShareMenu(OBJECT) 显示当前页面的转发按钮
	* wx.hideShareMenu(OBJECT) 隐藏转发按钮
	* wx.updateShareMenu(OBJECT) 更新转发属性
	* wx.getShareInfo(OBJECT) 获取转发详细信息
	* 获取更多转发信息
	* 页面内发起转发
* 获取二维码
* 收货地址
	* wx.chooseAddress(OBJECT) 调起用户编辑收货地址原生界面，并在编辑完成后返回用户选择的地址
* 卡券	
	* wx.addCard(OBJECT) 批量添加卡券
	* wx.openCard(OBJECT) 查看微信卡包中的卡券
	* 会员卡组件
* 设置
	* wx.openSetting(OBJECT) 调起客户端小程序设置界面，返回用户设置的操作结果
	* wx.getSetting(OBJECT) 获取用户的当前设置
* 微信运动
	* wx.getWeRunData(OBJECT)获取用户过去三十天微信运动步数，需要先调用 wx.login 接口
* 打开小程序
	* wx.navigateToMiniProgram(OBJECT) 打开同一公众号下关联的另一个小程序
	* wx.navigateBackMiniProgram(OBJECT) 返回到上一个小程序
* 打开APP
	* launchApp(OBJECT) 打开APP
* 获取发票抬头
	* wx.chooseInvoiceTitle(OBJECT) 选择用户的发票抬头
* 生物认证
	* wx.checkIsSupportSoterAuthentication(OBJECT) 获取本机支持的 SOTER 生物认证方式
	* wx.startSoterAuthentication(OBJECT) 开始 SOTER 生物认证
	* wx.checkIsSoterEnrolledInDevice(OBJECT) 获取设备内是否录入如指纹等生物信息的接口
* 附近
	* 添加地点 https://api.weixin.qq.com/wxa/addnearbypoi?access_token=ACCESS_TOKEN
	* 查看地点列表 https://api.weixin.qq.com/wxa/getnearbypoilist?page=1&page_rows=20&access_token=ACCESS_TOKEN
	* 删除地点（https://api.weixin.qq.com/wxa/delnearbypoi?access_token=ACCESS_TOKEN）
	* 展示/取消展示附近小程序 https://api.weixin.qq.com/wxa/setnearbypoishowstatus?access_token=ACCESS_TOKEN
* 插件管理
* 内容安全
	* imgSecCheck 校验一张图片是否含有违法违规内容
	* msgSecCheck 检查一段文本是否含有违法违规内容

#### 数据

* 常规分析
* 概况
	* 概况趋势
* 访问分析
	* 访问趋势
	* 访问分布
	* 访问留存
	* 访问页面
* 用户画像
* 自定义分析
	* 自定义数据上报

#### 更新

* wx.getUpdateManager() 获取全局唯一的版本更新管理器，用于管理小程序更新。

#### 多线程

* wx.createWorker(scriptPath) 创建一个 Worker 线程，并返回 Worker 实例

#### 监控

* wx.reportMonitor(name, value) 自定义业务数据监控上报接口

#### 调试接口

* wx.setEnableDebug(OBJECT) 设置是否打开调试开关，此开关对正式版也能生效

#### 日志

* wx.getLogManager() 获取日志管理器 logManager 对象





 
  
 





 









 





