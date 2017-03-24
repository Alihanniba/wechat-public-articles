###嗷嗷项目重构模块总结

>author: 刘立

>date: 2017-03-23

* 1.window全局变量能否抽统一的全局model
* 2.数据中心table抽统一的组件
* 3.业务中心->产品服务设置模块不清晰
* 4.业务中心-> 供应商管理与项目管理页面 大部分代码重复，可抽组件
* 5.业务中心->供应商管理详情组件可调用公共的table组件自定义化
* 6.财务中心->日账单与月账单组件可复用
* 7.个人中心->我的服务商与我的账号模块可复用
* 8.新手指南两个部分可复用

**以下说的中心模块指侧边栏单个tab,一个tab代表一个中心模块**

|模块| 组件 | 重复组件 |
|-----|-------|-------|
|数据中心|订单详情|table/search|
|数据中心|骑士订单|table/search|
|数据中心|商家订单|table/search|
|数据中心|区域订单|table/search|
|数据中心|订单明细|table/search|
|业务中心|团队管理->员工列表|table/search|
|业务中心|团队管理->团队列表|table/search|
|业务中心|商家管理->商家列表|table/search|
|业务中心|商家管理->签约列表|table/search|
|业务中心|供应商管理|table/search|
|业务中心|项目管理|table/search|
|财务中心|日账单/月账单|table/search|
|个人中心|我的服务商|table/search|
以上组件可以中心字段抽重复组件，也可以页面组件抽重复组件

**model层拆分**

* 数据中心model 划分太细
* 以中心为模块抽公共model
* 单个模块一个model
* subscription以中心模块为界抽离

**view层**

* 状态组件与无状态组件分开
* 大部分table都是无状态组件（无状态组件相对于状态组件具有性能优势）

**core模块**

* 二维码与服务商信息
* pop组件（弹出提示）
* 分页组件（所有table采用按需加载）
* 弹出框组件（弹出填写信息）
* 以中心模块拆分ActionName
* localStorage与sessionStorage统一格式

格式例如

```
/**
 * localStorage
 *
 * WATCH                      -->     视频播放数据
 * ALL_CHANNEL                -->     默认全部频道
 * USER_SELECTED_CHANNEL      -->     用户订购的频道
 * WELCOME_PAGE               -->     欢迎页key
 * USER_TOKEN                 -->     用户token
 * USER_NAME                  -->     用户名
 * HEAD_IMG_URL               -->     用户头像
 * NICK_NAME                  -->     用户昵称
 *
 *
 *
 * ------------------------------------------------
 * sessionStorage
 *
 * BACK_KEY                   -->     其他页面返回首页需定位在某个tab的key
 * BACK_POSITION_KEY          -->     其他页面返回首页 高亮导航 需定位的位置
 * HOME_SCROLL_KEY            -->     从频道点击频道进入首页相同频道时下滑距离
 * LIGHT_KEY                  -->     高亮tab key
 * LIVE_PAGE_KEY              -->     进入直播页面的key
 * PLAY_PAGE_KEY              -->     进入播放页面的key
 * ME_PAGE_KEY                -->     进入我的页面的key
 */

Storage.KEYS = {
  /** localStorage begin */
  SYNC_DATA: 'SYNC_DATA',
  WATCH: 'VIDEO_WATCH',
  ALL_CHANNEL: 'ALL_CHANNEL',
  USER_SELECTED_CHANNEL: 'USER_SELECTED_CHANNEL',
  WELCOME_PAGE: 'WELCOME_PAGE',
  HISTORY: 'WATCH_HISTORY',
  FAVOURITE: 'WATCH_FAVOURITE',
  USER_TOKEN: 'USER_TOKEN',
  USER_NAME: 'USER_NAME',
  HEAD_IMG_URL: 'HEAD_IMG_URL',
  NICK_NAME: 'NICK_NAME',
  USER_ID: 'USER_ID',
  SEARCH_HISTORY: 'SEARCH_HISTORY',
  HOME_LOCALS: {
    CURRENT_CHANNEL_ID: 'CURRENT_CHANNEL_ID',
    EPISODE_DATA: 'EPISODE_DATA',
    LOAD_MORE: 'LOAD_MORE',
    FIRST_KEY: 'FIRST_KEY',
    LAST_KEY: 'LAST_KEY'
  },
  /** localStorage over */
  /**-------------------------------------------------------- */
  /** sessionStorage begin */
  BACK_KEY: 'BACK_KEY',
  BACK_POSITION_KEY: 'BACK_POSITION_KEY',
  HOME_SCROLL_KEY: 'HOME_SCROLL_KEY',
  LIGHT_KEY: 'LIGHT_KEY',
  LIVE_PAGE_KEY: 'LIVE_PAGE_KEY',
  PLAY_PAGE_KEY: 'PLAY_PAGE_KEY',
  ME_PAGE_KEY: 'ME_PAGE_KEY',
  SEARCH_PAGE_KEY: 'SEARCH_PAGE_KEY',
  RECORD_PAGE_KEY: 'RECORD_PAGE_KEY',
  COLLECT_PAGE_KEY: 'COLLECT_PAGE_KEY',
  SEARCH_TEXT: 'SEARCH_TEXT',
  VERIFY_DATA: 'VERIFY_DATA'
  /** sessionStorage over */
```

**search组件属性**

* formItem
	* data
	* value
	* callback
	* placeholder
	* validate
		* value
		* rules

* pickdater
	* defaultValue
	* callback

* input
	* placeholder
	* callback
	* validate

* select
	* callback
	* data
	* defaultValue
* submit
	* callback
	* text
