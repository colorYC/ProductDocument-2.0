Windows 进阶
==========================

.. highlight:: csharp

**集成进阶功能前，请确保您已经集成了基础的音视频通话功能**

.. _查询频道(windows):

查询频道
---------------------------

如需查询频道相关信息，例如频道名称、是否存在、成员名、成员数，可以调用 query 接口进行查询操作
::

        /// <summary>
        /// 查询频道相关信息，例如是否存在、人数等
        /// </summary>
        /// <param name="channelId">频道标识</param>
        /// <returns>操作Id，与onQuery回调中的operationI对应</returns>
        public int query(string channelId)

示例代码::

    mediaChannel.query("channelId");

查询操作发起后，UI 通过以下方法监听回调查询的结果：
::

        /// <summary>
        /// 查询频道信息结果回调
        /// </summary>
        /// <param name="operationId">操作id，由query接口返回</param>
        /// <param name="result">ture表示查询成功，false表示查询失败</param>
        /// <param name="reason">查询失败原因，当result为false时该值有效</param>
        /// <param name="queryInfo">查询到的频道信息，当失败时值关注channelId</param>
        void onQuery(int operationId, bool result, JCMediaChannelReason reason, JCMediaChannelQueryInfo queryInfo);

其中，JCMediaChannelQueryInfo 有下面几个属性
::

        /// <summary>
        /// 媒体频道标识
        /// </summary>
        public string channelId;
        /// <summary>
        /// 媒体频道号
        /// </summary>
        public int number;
        /// <summary>
        /// 频道uri
        /// </summary>
        public string confUri;
        /// <summary>
        /// 成员数
        /// </summary>
        public int clientCount;
        /// <summary>
        /// 成员列表
        /// </summary>
        public List<string> participants;

示例代码::

    public void onQuery(int operationId, bool result, JCMediaChannelReason reason, JCMediaChannelQueryInfo queryInfo) {
       // 查询成功
       if (result) {
            // 查询频道标识
            String channelId = queryInfo.channelId;
            // 查询频道号
            int number = queryInfo.number;
            // 查询频道成员数
            int clientCount = queryInfo.clientCount;
            // 查询频道成员列表
            List<string> members = queryInfo.members;
       } else {
            // 查询失败
       }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^

获取频道成员
---------------------------

通过 userId 获取频道成员对象
::

        /// <summary>
        /// 获取频道内成员
        /// </summary>
        /// <param name="userId">用户唯一标识</param>
        /// <returns>成员对象</returns>
        public JCMediaChannelParticipant getParticipant(string userId)

^^^^^^^^^^^^^^^^^^^^^^^^^^^

踢出成员
---------------------------

调用下面的方法将成员踢出会议
::

        /// <summary>
        /// 踢人
        /// </summary>
        /// <param name="participant">成员</param>
        /// <returns>调用结果</returns>
        public bool kick(JCMediaChannelParticipant participant)

示例代码::

    JCMediaChannelParticipant participant = mediaChannel.getParticipant("userId");
    if (participant != nil) {
        mediaChannel.kick(participant);
    }

^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. _文件视频源(windows):

使用文件作为视频输入源
------------------------------

在视频通话中，可以将文件作为视频输入源，典型的场景如在线课堂的文稿显示，文件和摄像头作为视频输入源 **只能存在一种**，如果将文件作为视频输入源，而当前摄像头已开启的话，则会关闭摄像头。

开启视频文件作为视频输入源接口如下
::

        /// <summary>
        /// 开启视频文件作为视频输入源，文件和摄像头作为视频输入源只能存在一种，当前摄像头开启的话会关闭摄像头
        /// </summary>
        /// <returns>是否成功</returns>
        public bool startVideoFile()

逐帧采集视频画面
::

        /// <summary>
        /// 逐帧采集视频画面
        /// </summary>
        /// <param name="srcFrame">画面二进制数据</param>
        /// <param name="format">视频像素格式</param>
        /// <param name="width">宽</param>
        /// <param name="height">高</param>
        public void setVideoFileFrame(byte[] srcFrame, JCMediaDeviceVideoPixelFormat format, int width, int height)

如果想关闭文件视频源，调用下面的接口
::

        /// <summary>
        /// 关闭逐帧采集画面
        /// </summary>
        /// <returns>ture表示关闭成功，false表示关闭失败</returns>
        public bool stopVideoFile()


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _屏幕共享(windows):

桌面或窗口共享
----------------------

桌面或窗口共享可以让您和频道中的其他成员一起分享设备里的精彩内容，您可以在频道中利用桌面或窗口共享的功能进行文档演示、在线教育演示、视频会议以及游戏过程分享等。

开启桌面或窗口共享前需要先设置要共享的桌面或窗口id，具体通过 JCMediaDevice 类的 screenshareWindow 属性进行设置。
::

        /// <summary>
        /// 屏幕分享的窗口
        /// </summary>
        public JCMediaDeviceWindow screenshareWindow


设置要共享的桌面或窗口id
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

首先通过下面的属性获取桌面列表或者窗口列表
::

        /// 桌面列表
        public List<JCMediaDeviceWindow> desktops

        /// 窗口列表
        public List<JCMediaDeviceWindow> windows


其中，JCMediaDeviceWindow 类有以下两个属性
::

        /// 名称
        public string windowName
        /// id
        public string windowId


示例代码
::

    List<JCMediaDeviceWindow> desktops = mediaDevice.desktops;
    List<JCMediaDeviceWindow> windows = mediaDevice.windows;

列表获取后，可以选取要共享的窗口或桌面id
::

    mediaDevice.screenshareWindow = desktops[0];
    string windowId = mediaDevice.screenshareWindow.windowId;


屏幕共享采集属性设置
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

您可以调用 JCMediaDevice 类中的 setScreenCaptureProperty 方法设置屏幕共享采集属性，包括采集的高度、宽度和帧速率。
::

        /// <summary>
        /// 设置屏幕桌面共享采集属性
        /// </summary>
        /// <param name="width">采集宽度</param>
        /// <param name="height">采集高度</param>
        /// <param name="framerate">帧速率</param>
        public void setScreenCaptureProperty(int width, int height, int framerate)

.. note:: 该方法可以在开启屏幕共享前调用，也可以在屏幕共享中调用；如果在屏幕共享中调用，则设置的采集属性要在下次屏幕共享开启时生效。


开启或关闭屏幕共享
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

开启或关闭屏幕共享需要根据当前的屏幕共享状态进行判断，具体通过 screenUserId 进行判断。当 screenUserId 不为空时说明当前存在屏幕共享，不能再发起屏幕共享，只有当 screenUserId 为空时才可以发起屏幕共享。

屏幕共享状态是否变化通过 JCMediaChannelPropChangeParam 对象的 screenShare 属性判断。如果屏幕共享状态发生了改变会触发 onMediaChannelPropertyChange 回调
::

        /// <summary>
        /// 属性变化回调，目前主要关注屏幕共享和窗口共享状态的更新
        /// </summary>
        /// <param name="propChangeParam">变化集合</param>
        void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam);

如果当前不存在屏幕共享或者自己发起了屏幕共享，可以调用下面的方法开启或关闭屏幕共享
::

        /// <summary>
        /// 开启关闭桌面屏幕共享，内部根据当前状态决定是否开启
        /// </summary>
        /// <param name="enable">是否开启屏幕共享</param>
        /// <returns>返回true表示调用成功，false表示调用失败</returns>
        public bool enableScreenOrWindowShare(bool enable)


请求屏幕共享的视频流
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

如果频道中有成员开启了屏幕共享，其他成员将收到 onMediaChannelPropertyChange 的回调，并通过 screenUserId 属性获得发起屏幕共享的用户标识。

获得发起屏幕共享的用户标识后，可以调用 requestScreenVideo 方法请求屏幕共享的视频流
::

        /// <summary>
        /// 请求屏幕共享的视频流
        /// 当pictureSize未None表示关闭请求
        /// </summary>
        /// <param name="screenUri">屏幕分享uri</param>
        /// <param name="pictureSize">视频请求尺寸类型</param>
        /// <returns>返回true表示调用成功，false表示调用失败</returns>
        public bool requestScreenVideo(string screenUri, JCMediaChannelPictureSize pictureSize)

示例代码::

    public void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam) {
        if (propChangeParam.screenShare) {
            if (mediaChannel.screenUserId = null) {
                // 开启屏幕共享
                mediaChannel.enableScreenOrWindowShare(true);
                // 请求屏幕共享的视频流
                JCMediaDeviceVideoCanvas screenShare = mediaDevice.startVideo(mediaChannel.screenRenderId, JCMediaDeviceRenderMode.FULLCONTENT);
                mediaChannel.requestScreenVideo(mediaChannel.screenRenderId,JCMediaChannelPictureSize.Large);
            } else if (mediaChannel.screenUserId != null && "自己开启了屏幕共享") {
                // 关闭屏幕共享
                mediaChannel.enableScreenOrWindowShare(false);
            }
        }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _CDN 推流(windows):

CDN 推流
----------------------

CDN 推流服务适用于各类音视频直播场景，如企业级音视频会议、赛事、游戏直播、在线教育、娱乐直播等。

CDN 推流集成简单高效，开发者只需调用相关 API 即可将 CDN 推流无缝对接到自己的业务应用中。

推流地址设置
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

如要开启 CDN 推流，需在 **加入频道前** 进行 CDN 推流地址的设置。具体为通过 JoinParam 对象中的 cdn 属性进行设置
  
示例代码
::

    // 设置 CDN 推流地址
    JCMediaChannel.JoinParam joinParam = new JCMediaChannel.JoinParam();
    joinParam.cdn = "推流地址";
    // 加入频道
    mediaChannel.join("channelId", joinParam);


CDN 状态获取
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

开启 CDN 推流前需要判断 CDN 的状态，通过 JCMediaChannel 类的 cdnState 属性获取推流器状态。只有 CDN 当前状态不为 JCMediaChannelCdnStateNone 时才可以进行 CDN 推流。其中，CDN 推流状态有以下几种：
::

        /// <summary>
        /// 无法进行CDN推流
        /// </summary>
        None,
        /// <summary>
        /// 可以开启CDN推流
        /// </summary>
        Ready,
        /// <summary>
        /// CDN推流中
        /// </summary>
        Running


CDN 状态的变化通过 onMediaChannelPropertyChange 回调上报
::

        /// <summary>
        /// 属性变化回调，目前主要关注屏幕共享和窗口共享状态的更新
        /// </summary>
        /// <param name="propChangeParam">变化集合</param>
        void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam);


开启或关闭 CDN 推流
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

CDN 状态获取后，即可根据 CDN 的状态开启或关闭 CDN 推流，接口如下
::

        /// <summary>
        /// 开关Cdn推流，内部根据当前状态决定是否开启
        /// 在收到onMediaChannelPropertyChange回调时检查cdnState
        /// </summary>
        /// <param name="enable">是否开启cdn推流</param>
        /// <param name="keyInterval">推流关键帧间隔(毫秒)，当 enable 为 true 时有效，-1表示使用默认值(5000毫秒)</param>
        /// <returns>返回true表示调用成功，false表示调用失败</returns>
        public bool enableCdn(bool enable, int keyInterval)


示例代码
::

    public void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam) {
        if (propChangeParam.cdnState) { // CDN 状态变化
            // 根据CDN推流状态判断是否开启推流
            if (mediaChannel.cdnState == JCMediaChannelCdnState.None) {
                // 无法使用 CDN 推流
            } else if (mediaChannel.cdnState == JCMediaChannelCdnState.Ready) {
                // 可以开启 CDN 推流
                mediaChannel.enableCdn(true, 0);
            } else if (mediaChannel.cdnState == JCMediaChannelCdnState.Running) {
                // CDN 推流中，可以关闭 CDN 推流
                mediaChannel.enableCdn(false, 0);
            }
        }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _音视频录制(windows):

服务器音视频录制
----------------------

设置录制参数
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

服务器音频视频录制将录制的文件保存在七牛云上，因此，需要在七牛云注册账号并获取 AccessKey、SecretKey、BucketName、fileKey。

AccessKey、SecretKey、BucketName、fileKey 获取之后，利用 RecordParam 对象中的 buildQiniuRecordParam 方法构造七牛录制参数

::

    /// <summary>
    /// 七牛录制参数构造
    /// </summary>
    /// <param name="video">是否是视频录制</param>
    /// <param name="bucketName">七牛云 bucketName</param>
    /// <param name="secretKey">七牛云 secretKey</param>
    /// <param name="accessKey">七牛云 accessKey</param>
    /// <param name="fileName">录制文件名</param>
    /// <returns>录制参数字符串</returns>
    public static string buildQiniuRecordParam(bool video, string bucketName, string secretKey, string accessKey, string fileName)

录制参数构造之后，**在加入频道之前** 通过 JoinParam 对象中的 RecordParam 对象传入录制参数。

其中，RecordParam 对象有以下属性：

::

            /// <summary>
            /// 录制字符串参数 buildQiniuRecordParam
            /// </summary>
            public string recordString;


示例代码::

    // 设置录制参数
    JCMediaChannel.JoinParam joinParam = new JCMediaChannel.JoinParam();
    joinParam.record = new JCMediaChannel.RecordParam();
    joinParam.record.recordString = JCMediaChannel.RecordParam.buildQiniuRecordParam(
            true,
            "输入bucketName",
            "输入secretKey",
            "输入accessKey",
            "输入filename"
    );
    // 加入频道
    mediaChannel.join("channelId", joinParam);


.. note:: 

       如果进行音频录制，需要将构造方法中的 video 值设为 false。


获取录制状态
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

录制参数设置好后，需要根据目前的录制状态来判断是否启音视频录制。其中录制状态可通过 recordState 属性获得。

录制状态（JCMediaChannelRecordState）有以下几种：
::

        /// <summary>
        /// 无法进行视频录制
        /// </summary>
        None,
        /// <summary>
        /// 可以开启视频录制
        /// </summary>
        Ready,
        /// <summary>
        /// 视频录制中
        /// </summary>
        Running

录制状态的变化通过 onMediaChannelPropertyChange 回调上报
::

        /// <summary>
        /// 属性变化回调，目前主要关注屏幕共享和窗口共享状态的更新
        /// </summary>
        /// <param name="propChangeParam">变化集合</param>
        void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam);


开启或关闭音视频录制
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

录制状态获取后，即可根据录制状态调用下面的接口开启或关闭音视频录制
::

        /// <summary>
        /// 开关视频录制，内部根据当前状态决定是否开启
        /// </summary>
        /// <param name="enable">是否开启屏幕录制</param>
        /// <param name="recordParam">recordParam 录制参数，当 enable 为 true 时，可以更改由 join 时传入的录制参数，不需更改则填 null</param>
        /// <returns>返回true表示调用成功，false表示调用失败</returns>
        public bool enableRecord(bool enable, RecordParam recordParam)

.. note::  
      
      recordParam 录制参数，当 enable 为 true 时，可以更改由 join 传入的录制参数，不需更改则填 nil。

示例代码::

    public void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam) {
        if (changeParam.recordState) { // 录制状态变化 {
            // 根据音视频录制状态判断是否开启音视频录制
            if (mediaChannel.recordState == JCMediaChannelRecordState.None) {
                // 无法进行音视频录制
            } else if (mediaChannel.recordState == JCMediaChannelRecordState.Ready) {
                // 可以开启音视频录制
                mediaChannel.enableRecord(true, null);
            } else if (mediaChannel.recordState == JCMediaChannelRecordState.Running) {
                // 音视频录制中，可以关闭音视频录制
                mediaChannel.enableRecord(false, null);
            }
        }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _发送消息(windows):


发送消息
----------------------

如果想在频道中给其他成员发送消息，可以调用下面的接口
::

        /// <summary>
        /// 频道中发送消息，当 toUserId 不为 null 时，content 不能大于 4k
        /// </summary>
        /// <param name="type">消息类型</param>
        /// <param name="content">消息内容</param>
        /// <param name="toUserId">接收方成员的userid，值为null发送给所有人</param>
        /// <returns>是否发送成功</returns>
        public bool sendMessage(string type, string content, string toUserId)

其中，消息类型（type）为自定义类型。

示例代码::

    public void onJoin(bool result, JCMediaChannelReason reason, string channelId) {
        // 发送给所有成员
        mediaChannel.sendMessage("text", "content", null);
        // 发送给某个成员
        mediaChannel.sendMessage("text", "content", "userId");
    }


当频道中的其他成员收到消息时，会收到 onMessageReceive 回调
::

        /// <summary>
        /// 接收频道消息的回调
        /// </summary>
        /// <param name="type">消息类型</param>
        /// <param name="content">消息内容</param>
        /// <param name="fromUserId">消息发送成员userId</param>
        void onMessageReceive(string type, string content, string fromUserId);


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Sip 邀请(windows):

Sip 邀请
----------------------

利用 JC SDK 可以轻松实现多方音视频通话，但是如果出现用户所处的网络条件不好甚至没有网络条件的时候，就可以通过 PSTN 拨打 Sip 电话加入通话。

Sip 呼叫流程如下：

.. image:: images/sipworkflow.png

- Client 终端通过呼叫 Sip 用户号码邀请 Sip 用户加入频道；

- Sip 网关将 Sip 号码发送给 PSTN；

- PSTN 收到被邀请 Sip 号码后，登录该账号并加入频道；

- PSTN 加入频道成功后，向落地网关呼叫被邀请 Sip 号码；

- 落地网关呼叫 Sip 号码；

- Sip 用户接通后，PSTN 状态改变，频道中其他成员收到 PSTN 状态改变的回调。

邀请 Sip 用户，接口如下
::

        /// <summary>
        /// 邀请Sip用户，一般用于对接落地网关等
        /// </summary>
        /// <param name="userId">一般为号码</param>
        /// <param name="sipParam">sip 参数</param>
        /// <returns>成功返回值 >= 0，失败返回 -1</returns>
        public int inviteSipUser(string userId, SipParam sipParam)

其中，SipParam 对象有以下属性
::

    /// <summary>
    /// SIP呼叫 主叫号码 
    /// </summary>
    public string callerNumber;
    /// <summary>
    /// 核心网ID
    /// </summary>            
    public string coreNetwork;
    /// <summary>
    /// 额外参数对象
    /// </summary>
    public class Extra

Extra 有以下属性和方法
::

    /// <summary>
    /// JCMediaChannel.inviteSipUser 参数 userId 是号码还是 sipUri
    /// </summary>
    public bool sipUri;
    /// <summary>
    /// sipUri 为 true 才生效，决定 sip 信令是否路由到 userId 的 sip 域里
    /// </summary>
    public bool route;
    /// <summary>
    /// sip用户加入会议后的昵称
    /// </summary>
    public string displayName;
    /// <summary>
    /// JCMediaChannel.inviteSipUser 参数 userId 是否为 Mcu 会议
    /// </summary>
    public bool mcu;
    /// <summary>
    /// 是否需要视频接入
    /// </summary>
    public bool video;
    /// <summary>
    /// dtmf 密码
    /// </summary>
    public string dtmfPassowrd;

    /// <summary>
    /// 生成参数字符串
    /// </summary>
    /// <returns>参数字符串</returns>
    public string toParamString()


示例代码
::

    JCMediaChannel.SipParam sipParam = new JCMediaChannel.SipParam();
    sipParam.callerNumber = "主叫号码";
    sipParam.coreNetwork = "核心网ID";
    sipParam.extra = new JCMediaChannel.SipParam.Extra();
    sipParam.extra.sipUri = false;
    sipParam.extra.route = false;
    sipParam.extra.mcu = true;
    sipParam.extra.video = true;
    sipParam.extra.displayName = "昵称";
    sipParam.extra.dtmfPassowrd = "dtmf密码";
    mediaChannel.inviteSipUser("userId", sipParam);

