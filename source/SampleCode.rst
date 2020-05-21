Sample 代码
=============================

一对一视频通话
-------------------

iOS
>>>>>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: objective-c

**关键代码实现：**

1.模块初始化

::

    -(void)initialize 
    {
      //登录模块初始化
      JCClient *client = [JCClient create:@"your appkey" callback:self creatParam:nil];
      //设备模块初始化
      JCMediaDevice *mediaDevice = [JCMediaDevice create:client callback:self];
      //通话模块初始化
      JCCall *call = [JCCall create:client mediaDevice:mediaDevice callback:self];
    }


AppKey ：是应用在菊风云平台中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCCall <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCall.html>`_  ：一对一通话模块

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理

2. 登录

::

    -(void)login 
    {
       //发起登录
       [client login:@"账号" password:@"123" loginParam:nil];
    }


client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_ 实例


3.拨打视频通话

::

    -(void)videoCall 
    {
        // 调用接口发起呼叫
        [call call:@"对端号码" video:true extraParam:@"自定义透传字符串"];
    }

call 为 `JCCall <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCall.html>`_ 实例

4.本地视图渲染

::

    -(void)setLocalCanvas
    {
        // 创建本地视图Canvas对象
        JCMediaDeviceVideoCanvas *localCanvas = [mediaDevice startCameraVideo:JCMediaDeviceRenderFullContent];
    }

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ ：视频对象，用于UI层处理视频

JCMediaDeviceRenderFullContent：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRender <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceRender.html>`_

5.应答通话

::

    -(void)onCallItemAdd:(JCCallItem*)item {
        // 应答通话
        [call answer:item video:true];
    }

`JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCallItem.html>`_ ：通话对象，此类主要记录通话的一些状态，UI可以根据其中的状态进行更新

6.远端视图渲染

::

    -(void)setRemoteCanvas
    {
        // 获取远端视频画面，renderId来源JCCallItem对象
        JCMediaDeviceVideoCanvas *remoteCanvas = [mediaDevice startVideo:item.renderId renderType:JCMediaDeviceRenderFullContent]; 
    }

item ：`JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCallItem.html>`_  实例

JCMediaDeviceRenderFullContent：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRender <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceRender.html>`_

7.挂断通话

::

    -(void)term 
    {
      [call term:item reason:JCCallReasonNone description:@"主叫挂断"];
    }


call 为 `JCCall <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCall.html>`_ 实例

item 为 `JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCallItem.html>`_ 实例

mediaDevice 为 `JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ 实例


Android
>>>>>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: java

**关键代码实现：**


1.模块初始化

::

    public void initialize() 
    {
        // AppKey为创建应用获取的AppKey
        JCClient client = JCClient.create(Context, "your appkey", this, null);
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        JCCall call = JCCall.create(client, mediaDevice, this);
    }

AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCCall <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCall.html>`_  ：一对一通话模块

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理

2.登录

::

    public void login()
    {
        client.login("账号", "123", null);
    }

client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_ 实例

3.拨打视频通话

::

    public void videoCall() 
    {
        // 调用接口发起呼叫
        call.call("peer number", true, "自定义透传字符串");
    }

call 为 `JCCall <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCall.html>`_ 实例


4.本地视图渲染

::

    public void videoCallsetLocalCanvas()
    {
        // 创建本地视图Canvas对象
        JCMediaDeviceVideoCanvas localCanvas = mediaDevice.startCameraVideo(JCMediaDevice.RENDER_FULL_CONTENT);
    }

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_ ：视频对象，用于UI层处理视频

RENDER_FULL_CONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDevice.RenderType <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.RenderType.html>`_

5.应答通话

::

    public void onCallItemAdd(JCCallItem item) 
    {
        // 应答通话
        call.answer(item, true);
    }

`JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCallItem.html>`_ ：通话对象，此类主要记录通话的一些状态，UI可以根据其中的状态进行更新

6.远端视图渲染

::

    public void setRemoteCanvas()
    {
        // 获取远端视频画面，renderId来源JCCallItem对象
        JCMediaDeviceVideoCanvas remoteCanvas = mediaDevice.startVideo(item.renderId, JCMediaDevice.RENDER_FULL_CONTENT); 
    }

item 为 `JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCallItem.html>`_ 实例

RENDER_FULL_CONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDevice.RenderType <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.RenderType.html>`_

7.挂断通话

::

    public void endCall {
        JCCallItem item = call.getCallItems().get(0);
        call.term(item, JCCall.REASON_NONE, "自己挂断");
        //停止视频
        mediaDevice.stopVideo(canvas);
    }

call 为 `JCCall <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCall.html>`_ 实例

item 为 `JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCallItem.html>`_ 实例

mediaDevice 为 `JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDeviceVideoCanvas.html>`_ 实例


Windows
>>>>>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: csharp

**关键代码实现：**


1.模块初始化

::

    public void initialize() 
    {
        //登录模块初始化
        JCClient client = JCClient.create(app ,"AppKey", this, null);
        //设备模块初始化
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        //通话模块初始化
        JCCall call = JCCall.create(client, mediaDevice, this);
    }


AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCCall <http://developer.juphoon.com/portal/reference/V2.0/windows/html/e1a40c0e-ec58-49c2-3063-295fb883e86f.htm>`_  ：一对一通话模块

`JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_  ：设备模块，用于音视频设备管理


2.登录

::

    public void login()
    {
        //发起登录
        client.login("用户名", "密码", null);
    }

client 为 `JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_ 实例

3.拨打视频通话

::

    public void videoCall() 
    {
        // 调用接口发起呼叫
        call.call("peer number", true, "自定义透传字符串");
    }

call 为 `JCCall <http://developer.juphoon.com/portal/reference/V2.0/windows/html/e1a40c0e-ec58-49c2-3063-295fb883e86f.htm>`_ 实例

4.本地视图渲染

::

    public void videoCallsetLocalCanvas()
    {
        // 创建本地视图Canvas对象
        JCMediaDeviceVideoCanvas loacalCanvas = mediaDevice.startCameraVideo(JCMediaDeviceRenderMode.FULLCONTENT);
    }

`JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ ：视频对象，用于UI层处理视频

FULLCONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRenderMode <http://developer.juphoon.com/portal/reference/V2.0/windows/html/44604552-33eb-5a81-6b10-6c512d127a4b.htm>`_

5.应答通话

::

    public void onCallItemAdd(JCCallItem item) {
        // 应答通话
        call.answer(item, true);
    }

`JCCallItem <http://developer.juphoon.com/portal/reference/V2.0/windows/html/0267696e-79ee-8d46-c086-3c071a2b2b3a.htm>`_ ：通话对象，此类主要记录通话的一些状态，UI可以根据其中的状态进行更新

6.远端视图渲染

::

    public void setRemoteCanvas()
    {
        // 创建远端视频画面对象，renderId来源JCCallItem对象
        JCMediaDeviceVideoCanvas remoteCanvas = mediaDevice.startVideo(item.renderId, JCMediaDevice.JCMediaDeviceRenderMode.FULLSCREEN); 
    }

item ：`JCCallItem <http://developer.juphoon.com/portal/reference/V2.0/windows/html/0267696e-79ee-8d46-c086-3c071a2b2b3a.htm>`_  实例

FULLCONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRenderMode <http://developer.juphoon.com/portal/reference/V2.0/windows/html/44604552-33eb-5a81-6b10-6c512d127a4b.htm>`_

7.挂断通话

::

    public void endCall {
        //挂断通话
        JCCallItem item = call.callItems[0];
        call.term(item, JCCallReason.TermBySelf, "自己挂断");
        //停止视频
        mediaDevice.stopVideo(canvas);
    }

call 为 `JCCall <http://developer.juphoon.com/portal/reference/V2.0/windows/html/e1a40c0e-ec58-49c2-3063-295fb883e86f.htm>`_ 实例

item 为 `JCCallItem <http://developer.juphoon.com/portal/reference/V2.0/windows/html/0267696e-79ee-8d46-c086-3c071a2b2b3a.htm>`_ 实例

mediaDevice 为 `JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ 实例



一对一语音通话
---------------------

iOS
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: objective-c

**关键代码实现：**

1.模块初始化

::

    -(void)initialize 
    {
      //登录模块初始化
      JCClient *client = [JCClient create:@"your appkey" callback:self creatParam:nil];
      //设备模块初始化
      JCMediaDevice *mediaDevice = [JCMediaDevice create:client callback:self];
      //通话模块初始化
      JCCall *call = [JCCall create:client mediaDevice:mediaDevice callback:self];
    }


AppKey ：是应用在菊风云平台中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCCall <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCall.html>`_  ：一对一通话模块

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理


2. 登录

::

    -(void)login 
    {
       //发起登录
       [client login:@"账号" password:@"123" loginParam:nil];
    }


client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_ 实例


3. 拨打语音通话

::

    -(void)voiceCall 
    {
      //发起语音呼叫
      [call call:@"对方账号" video:false extraParam:@"自定义透传字符串"];
    }


call 为 `JCCall <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCall.html>`_ 实例


4.应答通话

::

    -(void)onCallItemAdd:(JCCallItem*)item {
        //应答通话
        [call answer:item video:false];
    }


`JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCallItem.html>`_ ：通话对象，此类主要记录通话的一些状态，UI可以根据其中的状态进行更新


5.挂断通话

::

    -(void)term 
    {
      [call term:item reason:JCCallReasonNone description:@"自己挂断"];
    }

call 为 `JCCall <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCall.html>`_ 实例

item 为 `JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCCallItem.html>`_ 实例


Android
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: java

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        // AppKey为创建应用获取的AppKey
        JCClient client = JCClient.create(Context, "your appkey", this, null);
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        JCCall call = JCCall.create(client, mediaDevice, this);
    }

AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCCall <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCall.html>`_  ：一对一通话模块

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理

2.登录

::

    public void login()
    {
        client.login("账号", "123", null);
    }

client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_ 实例

3.拨打语音通话

::

    public void videoCall() 
    {
        // 调用接口发起呼叫
        call.call("peer number", false, "自定义透传字符串");
    }

call 为 `JCCall <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCall.html>`_ 实例

4.应答通话

::

    public void onCallItemAdd(JCCallItem item) 
    {
        // 应答通话
        call.answer(item, false);
    }

`JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCallItem.html>`_ ：通话对象，此类主要记录通话的一些状态，UI可以根据其中的状态进行更新

5.挂断通话

::

    public void endCall {
        JCCallItem item = call.getCallItems().get(0);
        call.term(item, JCCall.REASON_NONE, "自己挂断");
    }

call 为 `JCCall <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCall.html>`_ 实例

item 为 `JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCallItem.html>`_ 实例



Windows
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: csharp

**关键代码实现：**


1.模块初始化

::

    public void initialize() 
    {
        //登录模块初始化
        JCClient client = JCClient.create(app ,"AppKey", this, null);
        //设备模块初始化
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        //通话模块初始化
        JCCall call = JCCall.create(client, mediaDevice, this);
    }


AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCCall <http://developer.juphoon.com/portal/reference/V2.0/windows/html/e1a40c0e-ec58-49c2-3063-295fb883e86f.htm>`_  ：一对一通话模块

`JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_  ：设备模块，用于音视频设备管理


2.登录

::

    public void login()
    {
        //发起登录
        client.login("用户名", "密码", null);
    }

client 为 `JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_ 实例

3.拨打语音通话

::

    public void voiceCall() 
    {
        // 调用接口发起呼叫
        call.call("peer number", false, "自定义透传字符串");
    }

call 为 `JCCall <http://developer.juphoon.com/portal/reference/V2.0/windows/html/e1a40c0e-ec58-49c2-3063-295fb883e86f.htm>`_ 实例

4.应答通话

::

    public void onCallItemAdd(JCCallItem item) {
        // 应答通话
        call.answer(item, false);
    }

`JCCallItem <http://developer.juphoon.com/portal/reference/V2.0/windows/html/0267696e-79ee-8d46-c086-3c071a2b2b3a.htm>`_ ：通话对象，此类主要记录通话的一些状态，UI可以根据其中的状态进行更新

5.挂断通话

::

    public void endCall {
        //挂断通话
        JCCallItem item = call.callItems[0];
        call.term(item, JCCallReason.TermBySelf, "自己挂断");
    }

call 为 `JCCall <http://developer.juphoon.com/portal/reference/V2.0/windows/html/e1a40c0e-ec58-49c2-3063-295fb883e86f.htm>`_ 实例

item 为 `JCCallItem <http://developer.juphoon.com/portal/reference/V2.0/windows/html/0267696e-79ee-8d46-c086-3c071a2b2b3a.htm>`_ 实例


多方视频通话
-------------------

iOS
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: objective-c


**关键代码实现：**

1.模块初始化

::

    -(void)initialize
    {
        //登录模块初始化
        JCClient *client = [JCClient create:@"your appkey" callback:self creatParam:nil];
        //设备模块初始化
        JCMediaDevice *mediaDevice = [JCMediaDevice create:client callback:self];
        //频道模块初始化
        JCMediaChannel *mediaChannel = [JCMediaChannel create:client mediaDevice:mediaDevice callback:self];
    }

AppKey ：是应用在 菊风云平台 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理


2. 登录

::

    -(void)login 
    {
       //发起登录
       [client login:@"账号" password:@"123" loginParam:nil];
    }


client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_ 实例


3.发送本地音视频流

::

    // 发送本地音频流
    [mediaChannel enableUploadAudioStream:true];
    // 发送本地视频流
    [mediaChannel enableUploadVideoStream:true];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


4.加入频道

::

    // 加入会议
    [mediaChannel join:@"频道id" joinParam:nil];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


5.本地视频渲染

::

    -(void)setLocalCanvas
    {
        // 创建本地视图Canvas对象
        JCMediaDeviceVideoCanvas *localCanvas = [mediaDevice startCameraVideo:JCMediaDeviceRenderFullContent];
    }

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ ：视频对象，用于UI层处理视频

JCMediaDeviceRenderFullContent：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRender <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceRender.html>`_


6.远端视频渲染

::

    -(void)onParticipantJoin:(JCMediaChannelParticipant*)participant {
        // 远端视频渲染
        JCMediaDeviceVideoCanvas *remote = [mediaDevice startVideo:participant.renderId renderType:JCMediaDeviceRenderFullContent];
        // 请求远端视频流
        [mediaChannel requestVideo:participant pictureSize:JCMediaChannelPictureSizeSmall];
    }

`JCMediaChannelParticipant <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannelParticipant.html>`_ ：媒体频道成员

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ ：视频对象，用于UI层处理视频

JCMediaDeviceRenderFullContent：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRender <http://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceRender.html>`_

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

JCMediaChannelPictureSizeSmall 为视频小尺寸，更多视频尺寸参见 `JCMediaChannelPictureSize <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaChannelPictureSize.html>`_

7.离开频道

::

    [mediaChannel leave];
    // 停止视频
    [mediaDevice stopVideo:canvas];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

mediaDevice 为 `JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ 实例


8.解散频道

::

    [mediaChannel stop];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


Android
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: java

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        // AppKey为创建应用获取的AppKey
        JCClient client = JCClient.create(Context, "your appkey", this, null);
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);
    }

AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理

`JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话

2.登录

::

    public void login()
    {
        client.login("账号", "123", null);
    }

client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_ 实例

3.发送本地音视频流

::

    // 发送本地音频流
    mediaChannel.enableUploadAudioStream(true);
    // 发送本地视频流
    mediaChannel.enableUploadVideoStream(true);

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


4.加入频道

::

    // 加入频道
    mediaChannel.join("频道id", null);

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


5.本地视图渲染

::

    public void setLocalCanvas()
    {
        // 创建本地视图Canvas对象
        JCMediaDeviceVideoCanvas localCanvas = mediaDevice.startCameraVideo(JCMediaDevice.RENDER_FULL_CONTENT);
    }

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_ ：视频对象，用于UI层处理视频

RENDER_FULL_CONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDevice.RenderType <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.RenderType.html>`_


6.远端视图渲染

::

    public void onParticipantJoin(JCMediaChannelParticipant participant) {
        // 创建远端视频画面对象，renderId来源JCMediaChannelParticipant对象
        JCMediaDeviceVideoCanvas remoteCanvas = mediaDevice.startVideo(renderId, JCMediaDevice.RENDER_FULL_CONTENT);
        // 请求远端视频流，participant为JCMediaChannelParticipant对象
        mediaChannel.requestVideo(participant, JCMediaChannel.PICTURESIZE_LARGE);
    }

`JCMediaChannelParticipant <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannelParticipant.html>`_ ：媒体频道成员

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  实例

RENDER_FULL_CONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDevice.RenderType <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.RenderType.html>`_

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例

PICTURESIZE_LARGE 为视频小尺寸，更多视频尺寸参见 `PictureSize <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.PictureSize.html>`_

7.离开频道

::

    // 离开频道
    mediaChannel.leave();
    // 停止视频
    mediaDevice.stopVideo(canvas);

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例

mediaDevice 为 `JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  实例

canvas 为 `JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  实例


8.解散频道

::

    mediaChannel.stop();

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例



Windows
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: csharp

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        //登录模块初始化
        JCClient client = JCClient.create(app ,"AppKey", this, null);
        //设备模块初始化
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        //频道模块初始化
        JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);
    }


AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_  ：设备模块，用于音视频设备管理

`JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话


2.登录

::

    public void login()
    {
        //发起登录
        client.login("用户名", "密码", null);
    }

client 为 `JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_ 实例


3.发送本地音视频流

::

    // 发送本地音频流
    mediaChannel.enableUploadAudioStream(true);
    // 发送本地视频流
    mediaChannel.enableUploadVideoStream(true);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例


4.加入频道

::

    // 加入频道
    mediaChannel.join("频道id", null);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

5.本地视图渲染

::

    public void setLocalCanvas()
    {
        // 创建本地视频画面对象
        JCMediaDeviceVideoCanvas localCanvas = mediaDevice.startCameraVideo(JCMediaDeviceRenderMode.FULLCONTENT);
    }

`JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ ：视频对象，用于UI层处理视频

FULLCONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRenderMode <http://developer.juphoon.com/portal/reference/V2.0/windows/html/44604552-33eb-5a81-6b10-6c512d127a4b.htm>`_


6.远端视图渲染

::

    // 成员加入回调
    public void onParticipantJoin(JCMediaChannelParticipant participant) {
        // 创建远端视频画面对象，renderId来源JCMediaChannelParticipant对象
        JCMediaDeviceVideoCanvas remoteCanvas = mediaDevice.startVideo(renderId, JCMediaDeviceRenderMode.FULLSCREEN);
        // 请求远端视频流，participant为JCMediaChannelParticipant对象
        mediaChannel.requestVideo(participant, JCMediaChannelPictureSize.Large);
    }


`JCMediaChannelParticipant <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8ad58616-3028-b8d3-8106-81b8b805c1ea.htm>`_ ：媒体频道成员

`JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ ：视频对象，用于UI层处理视频

FULLCONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRenderMode <http://developer.juphoon.com/portal/reference/V2.0/windows/html/44604552-33eb-5a81-6b10-6c512d127a4b.htm>`_

JCMediaChannelPictureSize 为视频小尺寸，更多视频尺寸参见 `JCMediaChannelPictureSize <http://developer.juphoon.com/portal/reference/V2.0/windows/html/a8c3f23e-c3b9-ce29-b594-412995b0e4ca.htm>`_

7.离开频道

::

    // 离开频道
    mediaChannel.leave();
    // 停止视频
    mediaDevice.stopVideo(canvas);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

mediaDevice 为 `JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ 实例

8.解散频道

::

    mediaChannel.stop();

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例



多方语音通话
---------------------

iOS
>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: objective-c

**关键代码实现：**

1.模块初始化

::

    -(void)initialize
    {
        //登录模块初始化
        JCClient *client = [JCClient create:@"your appkey" callback:self creatParam:nil];
        //设备模块初始化
        JCMediaDevice *mediaDevice = [JCMediaDevice create:client callback:self];
        //频道模块初始化
        JCMediaChannel *mediaChannel = [JCMediaChannel create:client mediaDevice:mediaDevice callback:self];
    }

AppKey ：是应用在 菊风云平台 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理


2. 登录

::

    -(void)login 
    {
       //发起登录
       [client login:@"账号" password:@"123" loginParam:nil];
    }


client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_ 实例


3.发送本地音频流

::

    // 发送本地音频流
    [mediaChannel enableUploadAudioStream:true];
    // 停止发送本地视频流
    [mediaChannel enableUploadVideoStream:true];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


4.加入频道

::

    // 加入会议
    [mediaChannel join:@"频道id" joinParam:nil];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

5.离开频道

::

    [mediaChannel leave];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

mediaDevice 为 `JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ 实例


6.解散频道

::

    [mediaChannel stop];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


Android
>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: java

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        // AppKey为创建应用获取的AppKey
        JCClient client = JCClient.create(Context, "your appkey", this, null);
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);
    }

AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理

`JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话

2.登录

::

    public void login()
    {
        client.login("账号", "123", null);
    }

client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_ 实例

3.发送本地音视频流

::

    // 发送本地音频流
    mediaChannel.enableUploadAudioStream(true);
    // 停止发送本地视频流
    mediaChannel.enableUploadVideoStream(false);

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


4.加入频道

::

    // 加入频道
    mediaChannel.join("频道id", null);

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例

5.离开频道

::

    // 离开频道
    mediaChannel.leave();

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


6.解散频道

::

    mediaChannel.stop();

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


Windows
>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: csharp

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        //登录模块初始化
        JCClient client = JCClient.create(app ,"AppKey", this, null);
        //设备模块初始化
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        //频道模块初始化
        JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);
    }


AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_  ：设备模块，用于音视频设备管理

`JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话


2.登录

::

    public void login()
    {
        //发起登录
        client.login("用户名", "密码", null);
    }

client 为 `JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_ 实例


3.发送本地音频流

::

    // 发送本地音频流
    mediaChannel.enableUploadAudioStream(true);
    // 停止发送本地视频流
    mediaChannel.enableUploadVideoStream(false);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例


4.加入频道

::

    // 加入频道
    mediaChannel.join("频道id", null);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

5.离开频道

::

    // 离开频道
    mediaChannel.leave();
    // 停止视频
    mediaDevice.stopVideo(canvas);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

mediaDevice 为 `JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ 实例

6.解散频道

::

    mediaChannel.stop();

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例



视频互动直播
---------------------

iOS
>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: objective-c

**关键代码实现：**

1.模块初始化

::

    -(void)initialize
    {
        //登录模块初始化
        JCClient *client = [JCClient create:@"your appkey" callback:self creatParam:nil];
        //设备模块初始化
        JCMediaDevice *mediaDevice = [JCMediaDevice create:client callback:self];
        //频道模块初始化
        JCMediaChannel *mediaChannel = [JCMediaChannel create:client mediaDevice:mediaDevice callback:self];
    }

AppKey ：是应用在 菊风云平台 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理


2.登录

::

    -(void)login 
    {
       //发起登录
       [client login:@"账号" password:@"123" loginParam:nil];
    }

client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_ 实例


3.角色设置

::

    //自定义主播角色，根据JCMediaChannelCustomState枚举值自定义角色
    JCMediaChannelCustomRole ROLE_BROASCASTER = JCMediaChannelCustomRole0;
    //自定义观众角色，根据JCMediaChannelCustomState枚举值自定义角色
    JCMediaChannelCustomRole ROLE_AUDIENCE = JCMediaChannelCustomRole1;
    // 设置角色，participant值为nil代表设置自身的角色
    [mediaChannel setCustomRole:ROLE_BROASCASTER participant:nil];

JCMediaChannelCustomRole 为自定义角色，参考 JCMediaChannelConstants.h 文件

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

`JCMediaChannelParticipant <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannelParticipant.html>`_ ：媒体频道成员


4.发送本地音视频流

::

    // 发送本地音频流，主播需要发送，观众则不需要
    [mediaChannel enableUploadVideoStream:customeRole == ROLE_BROASCASTER];
    // 发送本地视频流，主播需要发送，观众则不需要
    [mediaChannel enableUploadAudioStream:customeRole == ROLE_BROASCASTER];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


5.加入频道

::

    // 加入直播
    [mediaChannel join:@"频道id" joinParam:nil];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


6.本地视图渲染

::

    //主播需要创建本地视图
    -(void)setLocalCanvas
    {
        // 创建本地视图Canvas对象
        JCMediaDeviceVideoCanvas *localCanvas = [mediaDevice startCameraVideo:JCMediaDeviceRenderFullContent];
    }

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ ：视频对象，用于UI层处理视频

JCMediaDeviceRenderFullContent：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRender <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceRender.html>`_


7.远端视图渲染

::

    //观众端需要请求主播的画面
    -(void)onParticipantJoin:(JCMediaChannelParticipant*)participant {
        // 远端视频渲染
        JCMediaDeviceVideoCanvas *remote = [mediaDevice startVideo:participant.renderId renderType:JCMediaDeviceRenderFullContent];
        // 请求远端视频流
        [mediaChannel requestVideo:participant pictureSize:JCMediaChannelPictureSizeSmall];
    }

`JCMediaChannelParticipant <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannelParticipant.html>`_ ：媒体频道成员

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ ：视频对象，用于UI层处理视频

JCMediaDeviceRenderFullContent：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRender <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaDeviceRender.html>`_

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

JCMediaChannelPictureSizeSmall 为视频小尺寸，更多视频尺寸参见 `JCMediaChannelPictureSize <https://developer.juphoon.com/portal/reference/V2.0/ios/Constants/JCMediaChannelPictureSize.html>`_


8.离开频道

::

    [mediaChannel leave];
    // 停止视频
    [mediaDevice stopVideo:canvas];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

mediaDevice 为 `JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDeviceVideoCanvas.html>`_ 实例


9.解散频道

::

    [mediaChannel stop];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例



Android
>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: java

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        // AppKey为创建应用获取的AppKey
        JCClient client = JCClient.create(Context, "your appkey", this, null);
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);
    }

AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理

`JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话

2.登录

::

    public void login()
    {
        client.login("账号", "123", null);
    }

client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_ 实例


3.角色设置

::

    //自定义主播角色，根据CustomState枚举值自定义角色
    int ROLE_BROASCASTER = JCMediaChannel.CUSTOM_ROLE_0;
    //自定义观众角色，根据CustomState枚举值自定义角色
    int ROLE_AUDIENCE = JCMediaChannel.CUSTOM_ROLE_1;
    // 设置角色，participant值为nil代表设置自身的角色
    mediaChannel.setCustomRole(ROLE_BROASCASTER, null);

CustomRole 为自定义角色，参考 JCMediaChannel 文件

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例

4.发送本地音视频流

::

    // 发送本地音频流，主播需要发送，观众则不需要
    mediaChannel.enableUploadAudioStream(customRole == ROLE_BROASCASTER);
    // 发送本地视频流，主播需要发送，观众则不需要
    mediaChannel.enableUploadVideoStream(customRole == ROLE_BROASCASTER);

5.加入频道

::

    // 加入频道
    mediaChannel.join("频道id", null);

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


6.本地视图渲染

::

    public void setLocalCanvas()
    {
        // 创建本地视图Canvas对象
        JCMediaDeviceVideoCanvas localCanvas = mediaDevice.startCameraVideo(JCMediaDevice.RENDER_FULL_CONTENT);
    }

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_ ：视频对象，用于UI层处理视频

RENDER_FULL_CONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDevice.RenderType <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.RenderType.html>`_


7.远端视图渲染

::

    public void onParticipantJoin(JCMediaChannelParticipant participant) {
        // 创建远端视频画面对象，renderId来源JCMediaChannelParticipant对象
        JCMediaDeviceVideoCanvas remoteCanvas = mediaDevice.startVideo(renderId, JCMediaDevice.RENDER_FULL_CONTENT);
        // 请求远端视频流，participant为JCMediaChannelParticipant对象
        mediaChannel.requestVideo(participant, JCMediaChannel.PICTURESIZE_LARGE);
    }

`JCMediaChannelParticipant <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannelParticipant.html>`_ ：媒体频道成员

`JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  实例

RENDER_FULL_CONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDevice.RenderType <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.RenderType.html>`_

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例

PICTURESIZE_LARGE 为视频小尺寸，更多视频尺寸参见 `PictureSize <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.PictureSize.html>`_

8.离开频道

::

    // 离开频道
    mediaChannel.leave();
    // 停止视频
    mediaDevice.stopVideo(canvas);

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例

mediaDevice 为 `JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  实例

canvas 为 `JCMediaDeviceVideoCanvas <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  实例


9.解散频道

::

    mediaChannel.stop();

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


Windows
>>>>>>>>>>>>>>>>>>>>>>>

.. highlight:: csharp

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        //登录模块初始化
        JCClient client = JCClient.create(app ,"AppKey", this, null);
        //设备模块初始化
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        //频道模块初始化
        JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);
    }


AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_  ：设备模块，用于音视频设备管理

`JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话


2.登录

::

    public void login()
    {
        //发起登录
        client.login("用户名", "密码", null);
    }

client 为 `JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_ 实例

3.角色设置

::

    //自定义主播角色，根据JCMediaChannelCustomRole枚举值自定义角色
    JCMediaChannelCustomRole ROLE_BROASCASTER = JCMediaChannelConstants.CUSTOM_ROLE_0;
    //自定义观众角色，根据JCMediaChannelCustomRole枚举值自定义角色
    JCMediaChannelCustomRole ROLE_AUDIENCE = JCMediaChannelConstants.CUSTOM_ROLE_1;
    //设置角色，participant值为null代表设置自身的角色
    mediaChannel.setCustomRole(ROLE_BROASCASTER, null);


JCMediaChannelCustomRole 为自定义角色，参考 JCMediaChannelConstants.cs 文件

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

`JCMediaChannelParticipant <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8ad58616-3028-b8d3-8106-81b8b805c1ea.htm>`_ ：媒体频道成员


4.发送本地音视频流

::

    // 发送本地音频流，主播需要发送，观众则不需要
    mediaChannel.enableUploadAudioStream(customeRole == ROLE_BROASCASTER);
    // 发送本地视频流，主播需要发送，观众则不需要
    mediaChannel.enableUploadVideoStream(customeRole == ROLE_BROASCASTER);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例


5.加入频道

::

    // 加入频道
    mediaChannel.join("频道id", null);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

6.本地视图渲染

::

    //主播需要创建本地视图
    public void setLocalCanvas()
    {
        // 创建本地视频画面对象
        JCMediaDeviceVideoCanvas localCanvas = mediaDevice.startCameraVideo(JCMediaDeviceRenderMode.FULLCONTENT);
    }

`JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ ：视频对象，用于UI层处理视频

FULLCONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRenderMode <http://developer.juphoon.com/portal/reference/V2.0/windows/html/44604552-33eb-5a81-6b10-6c512d127a4b.htm>`_


7.远端视图渲染

::

    //观众端需要请求主播的画面
    public void onParticipantJoin(JCMediaChannelParticipant participant) {
        // 创建远端视频画面对象，renderId来源JCMediaChannelParticipant对象
        JCMediaDeviceVideoCanvas remoteCanvas = mediaDevice.startVideo(renderId, JCMediaDeviceRenderMode.FULLSCREEN);
        // 请求远端视频流，participant为JCMediaChannelParticipant对象
        mediaChannel.requestVideo(participant, JCMediaChannelPictureSize.Large);
    }


`JCMediaChannelParticipant <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8ad58616-3028-b8d3-8106-81b8b805c1ea.htm>`_ ：媒体频道成员

`JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ ：视频对象，用于UI层处理视频

FULLCONTENT：视频内容全显示，渲染模式选择参考 `JCMediaDeviceRenderMode <http://developer.juphoon.com/portal/reference/V2.0/windows/html/44604552-33eb-5a81-6b10-6c512d127a4b.htm>`_

JCMediaChannelPictureSize.Large 为视频大尺寸，更多视频尺寸参见 `JCMediaChannelPictureSize <http://developer.juphoon.com/portal/reference/V2.0/windows/html/a8c3f23e-c3b9-ce29-b594-412995b0e4ca.htm>`_

8.离开频道

::

    // 离开频道
    mediaChannel.leave();
    // 停止视频
    mediaDevice.stopVideo(canvas);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

mediaDevice 为 `JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ 实例


9.解散频道

::

    mediaChannel.stop();

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例



语音互动直播
-------------------

iOS
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: objective-c

**关键代码实现：**

1.模块初始化

::

    -(void)initialize
    {
        //登录模块初始化
        JCClient *client = [JCClient create:@"your appkey" callback:self creatParam:nil];
        //设备模块初始化
        JCMediaDevice *mediaDevice = [JCMediaDevice create:client callback:self];
        //频道模块初始化
        JCMediaChannel *mediaChannel = [JCMediaChannel create:client mediaDevice:mediaDevice callback:self];
    }

AppKey ：是应用在 菊风云平台 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理


2.登录

::

    -(void)login 
    {
       //发起登录
       [client login:@"账号" password:@"123" loginParam:nil];
    }

client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCClient.html>`_ 实例


3.角色设置

::

    //自定义主播角色，根据JCMediaChannelCustomState枚举值自定义角色
    JCMediaChannelCustomRole ROLE_BROASCASTER = JCMediaChannelCustomRole0;
    //自定义观众角色，根据JCMediaChannelCustomState枚举值自定义角色
    JCMediaChannelCustomRole ROLE_AUDIENCE = JCMediaChannelCustomRole1;
    // 设置角色，participant值为nil代表设置自身的角色
    [mediaChannel setCustomRole:ROLE_BROASCASTER participant:nil];

JCMediaChannelCustomRole 为自定义角色，参考 JCMediaChannelConstants.h 文件

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

`JCMediaChannelParticipant <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannelParticipant.html>`_ ：媒体频道成员


4.发送本地音频流

::

    // 发送本地音频流，主播需要发送，观众则不需要
    [mediaChannel enableUploadVideoStream:customeRole == ROLE_BROASCASTER];
    // 停止发送本地视频流，语音直播中不需要发送
    [mediaChannel enableUploadVideoStream:false];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


5.加入频道

::

    // 加入直播
    [mediaChannel join:@"频道id" joinParam:nil];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例

6.离开频道

::

    [mediaChannel leave];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


7.解散频道

::

    [mediaChannel stop];

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/ios/Classes/JCMediaChannel.html>`_  实例


Android
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: java

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        // AppKey为创建应用获取的AppKey
        JCClient client = JCClient.create(Context, "your appkey", this, null);
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);
    }

AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_  ：设备模块，用于音视频设备管理

`JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话

2.登录

::

    public void login()
    {
        client.login("账号", "123", null);
    }

client 为 `JCClient <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCClient.html>`_ 实例


3.角色设置

::

    //自定义主播角色，根据CustomState枚举值自定义角色
    int ROLE_BROASCASTER = JCMediaChannel.CUSTOM_ROLE_0;
    //自定义观众角色，根据CustomState枚举值自定义角色
    int ROLE_AUDIENCE = JCMediaChannel.CUSTOM_ROLE_1;
    // 设置角色，participant值为nil代表设置自身的角色
    mediaChannel.setCustomRole(ROLE_BROASCASTER, null);

CustomRole 为自定义角色，参考 JCMediaChannel 文件

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例

4.发送本地音频流

::

    // 发送本地音频流，主播需要发送，观众则不需要
    mediaChannel.enableUploadAudioStream(customRole == ROLE_BROASCASTER);
    // 发送本地视频流，语音直播中不需要发送
    mediaChannel.enableUploadVideoStream(false);

5.加入频道

::

    // 加入频道
    mediaChannel.join("频道id", null);

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


6.离开频道

::

    //离开频道
    mediaChannel.leave();

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


7.解散频道

::

    mediaChannel.stop();

mediaChannel 为 `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_  实例


Windows
>>>>>>>>>>>>>>>>>>>>>

.. highlight:: csharp

**关键代码实现：**

1.模块初始化

::

    public void initialize() 
    {
        //登录模块初始化
        JCClient client = JCClient.create(app ,"AppKey", this, null);
        //设备模块初始化
        JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
        //频道模块初始化
        JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);
    }


AppKey ：是应用在 Juphoon Cloud Platform 中的唯一标识，通过 `注册开发者网站 <http://developer.juphoon.com/signup>`_ 创建应用（创建应用需要贴链接） 后获取

`JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_  ：登录模块，用于登录登出管理及账户属性存储

`JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_  ：设备模块，用于音视频设备管理

`JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  ：媒体频道模块，类似音视频房间的概念，可以通过频道号加入频道进行音视频通话


2.登录

::

    public void login()
    {
        //发起登录
        client.login("用户名", "密码", null);
    }

client 为 `JCClient <http://developer.juphoon.com/portal/reference/V2.0/windows/html/7d4a1961-552c-2ad5-9a54-74222c4135a3.htm>`_ 实例

3.角色设置

::

    //自定义主播角色，根据JCMediaChannelCustomRole枚举值自定义角色
    JCMediaChannelCustomRole ROLE_BROASCASTER = JCMediaChannelConstants.CUSTOM_ROLE_0;
    //自定义观众角色，根据JCMediaChannelCustomRole枚举值自定义角色
    JCMediaChannelCustomRole ROLE_AUDIENCE = JCMediaChannelConstants.CUSTOM_ROLE_1;
    //设置角色，participant值为null代表设置自身的角色
    mediaChannel.setCustomRole(ROLE_BROASCASTER, null);


JCMediaChannelCustomRole 为自定义角色，参考 JCMediaChannelConstants.cs 文件

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

`JCMediaChannelParticipant <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8ad58616-3028-b8d3-8106-81b8b805c1ea.htm>`_ ：媒体频道成员


4.发送本地音频流

::

    // 发送本地音频流，主播需要发送，观众则不需要
    mediaChannel.enableUploadAudioStream(customeRole == ROLE_BROASCASTER);
    // 发送本地视频流，主播需要发送，观众则不需要
    mediaChannel.enableUploadVideoStream(customeRole == ROLE_BROASCASTER);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例


5.加入频道

::

    // 加入频道
    mediaChannel.join("频道id", null);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

6.离开频道

::

    // 离开频道
    mediaChannel.leave();
    // 停止视频
    mediaDevice.stopVideo(canvas);

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

mediaDevice 为 `JCMediaDevice <http://developer.juphoon.com/portal/reference/V2.0/windows/html/034d5af6-ec04-5148-7ec5-04e27e93e8c2.htm>`_ 实例

canvas 为 `JCMediaDeviceVideoCanvas <http://developer.juphoon.com/portal/reference/V2.0/windows/html/6a5b853c-d890-c30e-d236-5728d789ace1.htm>`_ 实例


7.解散频道

::

    mediaChannel.stop();

mediaChannel 为 `JCMediaChannel <http://developer.juphoon.com/portal/reference/V2.0/windows/html/8289e4bf-8045-497b-f584-fc76cad8f8a1.htm>`_  实例

