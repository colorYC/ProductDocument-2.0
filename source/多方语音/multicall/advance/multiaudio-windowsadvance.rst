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

.. _音视频录制(windows):

服务器音频录制
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