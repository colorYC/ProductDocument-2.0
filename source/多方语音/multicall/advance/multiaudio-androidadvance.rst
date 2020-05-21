Android 进阶
=========================

.. highlight:: java

**集成进阶功能前，请确保您已经集成了基础的音视频通话功能**

.. note:: SDK 不支持模拟器运行，请使用真机。

.. _查询频道(android):

查询频道
---------------------------

如需查询频道相关信息，例如频道名称、是否存在、成员名、成员数，可以调用 query 接口进行查询操作
::

    /**
     * 查询频道相关信息，例如是否存在，人数等
     *
     * @param channelId 频道标识
     * @return          返回操作id，与 onQuery 回调中的 operationId 对应
     */
    public abstract int query(String channelId);

示例代码::

    mediaChannel.query("channelId");

查询操作发起后，UI 通过以下方法监听回调查询的结果：
::

    /**
     * 查询频道信息结果回调
     *
     * @param operationId 操作id，由 query 接口返回
     * @param result      查询结果，true 表示查询成功，false 表示查询失败
     * @param reason      查询失败原因，当 result 为 false 时该值有效
     * @param queryInfo   查询到的频道信息
     */
    public void onQuery(int operationId, boolean result, @JCMediaChannel.MediaChannelReason int reason, JCMediaChannelQueryInfo queryInfo);


示例代码::

    public void onQuery(int operationId, boolean result, @JCMediaChannel.MediaChannelReason int reason, JCMediaChannelQueryInfo queryInfo) {
       // 查询成功
       if (result) {
            // 频道标识
            String channelId = queryInfo.getChannelId();
            // 频道
            int number = queryInfo.getNumber();
            // 频道成员数
            int clientCount = queryInfo.getClientCount();
            // 频道成员列表
            List<String>  members = queryInfo.getMembers();
       } else {
            // 查询失败
       }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _获取成员(android):

获取成员对象
---------------------------

通过 userId 获取频道成员对象
::

    /**
     * 获取频道成员
     *
     * @param userId 用户唯一标识
     * @return 成员对象
     */
    public abstract JCMediaChannelParticipant getParticipant(String userId);

^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _踢出成员(android):

踢出成员
---------------------------

调用下面的方法将成员踢出会议
::

    /**
     * 将成员踢出会议
     *
     * @param participant 成员
     * @return true表示成功，false表示失败
     */
    public abstract boolean kick(JCMediaChannelParticipant participant);

示例代码::

    JCMediaChannelParticipant participant = mediaChannel.getParticipant("userId");
    if (participant != nil) {
        mediaChannel.kick(participant);
    }

^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _CDN 推流(android):

CDN 推流
----------------------

CDN 推流服务适用于各类音视频直播场景，如企业级音视频会议、赛事、游戏直播、在线教育、娱乐直播等。

CDN 推流集成简单高效，开发者只需调用相关 API 即可将 CDN 推流无缝对接到自己的业务应用中。

推流地址设置
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

如要开启 CDN 推流，需在 **加入频道前** 进行 CDN 推流地址的设置。具体为通过 JCMediaChannel.JoinParam 对象中的 cdn 属性进行设置

示例代码
::

    // 设置 CDN 推流地址
    JCMediaChannel.JoinParam joinParam = new JCMediaChannel.JoinParam();
    joinParam.cdn = "推流地址";
    // 加入频道
    mediaChannel.join("222", joinParam);


CDN 状态获取
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

开启 CDN 推流前需要判断 CDN 的状态，通过下面的方法获取 CDN 的状态
::

    /**
     * 获得cdn推流状态
     *
     * @return cdn推流状态
     * @see CdnState
     */
    @CdnState
    public abstract int getCdnState();

其中，CdnState（推流状态）有以下几种：
::

    // 无法进行CDN推流
    public static final int CDN_STATE_NONE = 0;
    // 可以开启CDN推流
    public static final int CDN_STATE_READY = 1;
    // CDN推流中
    public static final int CDN_STATE_RUNNING = 2;

只有 CDN 当前状态不为 JCMediaChannel.CDN_STATE_NONE 时才可以开启 CDN 推流。

CDN 状态的变化通过 onMediaChannelPropertyChange 回调上报
::

    /**
     * 属性变化回调，目前主要关注屏幕共享状态的更新
     *
     * @param propChangeParam 变化标识集合
     */
    void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam);


开启或关闭 CDN 推流
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

CDN 状态获取后，即可根据 CDN 的状态开启或关闭 CDN 推流，接口如下
::

    /**
     * 开关Cdn推流
     * 在收到 onMediaChannelPropertyChange 回调后检查是否开启
     *
     * @param enable       是否开启Cdn推流
     * @param keyInterval  推流关键帧间隔(毫秒)，当 enable 为 true 时有效，-1表示使用默认值(5000毫秒)，有效值需要>=1000
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean enableCdn(boolean enable, int keyInterval);


示例代码
::

    public onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam) {
        if (propChangeParam.cdn) { // CDN 状态变化
            // 根据CDN推流状态判断是否开启推流
            if (mediaChannel.getCdnState() = JCMediaChannel.CDN_STATE_NONE) {
                // 无法使用 CDN 推流
            } else if (mediaChannel.getCdnState() == JCMediaChannel.CDN_STATE_READY) {
                // 可以开启 CDN 推流
                mediaChannel.enableCdn(true, 0);
            } else if (mediaChannel.getCdnState() == JCMediaChannel. CDN_STATE_RUNNING) {
                // CDN 推流中，可以关关闭 CDN 推
                mediaChannel.enableCdn(false, 0);
            }
        }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _音频录制(android):

服务器音频录制
----------------------

服务器音频录制将录制的文件保存在七牛云上，因此，如果需要进行服务器音视频录制，需要在加入频道之前设置录制参数，然后在加入频道的时候传入录制参数。

设置录制参数
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

在七牛云注册账号并获取 AccessKey、SecretKey、BucketName、fileKey。

AccessKey、SecretKey、BucketName、fileKey 获取之后，利用 JCMediaChannel.JoinParam.RecordParam 对象中的 buildQiniuRecordParam 方法构造七牛录制参数
::

        /**
         * 七牛录制参数构造
         *
         * @param video 是否是视频录制
         * @param bucketName 七牛云 bucketName
         * @param secretKey 七牛云 secretKey
         * @param accessKey 七牛云 accessKey
         * @param fileName 录制文件名
         * @return 录制参数字符串
         */
        public static String buildQiniuRecordParam(boolean video, String bucketName, String secretKey, String accessKey, String fileName)


录制参数构造之后，**在加入频道之前** 通过 JCMediaChannel.JoinParam.RecordParam 对象传入录制参数。

其中，JCMediaChannel.JoinParam.RecordParam 对象有以下属性：

::

        /**
         * 录制字符串
         */
        public String recordString;


示例代码::

    // 设置录制参数
    JCMediaChannel.JoinParam joinParam = new JCMediaChannel.JoinParam();
    joinParam.record = new JCMediaChannel.RecordParam();
    joinParam.record.recordString = JCMediaChannel.RecordParam.buildQiniuRecordParam(false, bucketName, secretKey, accessKey, fileName);

.. note:: 

       音频录制时，需要将构造方法中的 video 值设为 false。


获取录制状态
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

录制参数设置好后，需要根据目前的录制状态来判断是否启音视频录制。获取频录制状态接口如下
::

    /**
     * 获取频录制状态
     *
     * @return 视频录制状态
     * @see RecordState
     */
    @RecordState
    public abstract int getRecordState();


其中，RecordState 有以下几种：
::

    /**
     * 无法进行视频录制
     */
    public static final int RECORD_STATE_NONE = 0;
    /**
     * 可以开启视频录制
     */
    public static final int RECORD_STATE_READY = 1;
    /**
     * 视频录制中
     */
    public static final int RECORD_STATE_RUNNING = 2;


录制状态的变化通过 onMediaChannelPropertyChange 回调上报
::

    /**
     * 属性变化回调，目前主要关注屏幕共享状态的更新
     *
     * @param propChangeParam 变化标识集合
     */
    void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam);


开启或关闭音视频录制
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

录制状态获取后，即可根据录制状态调用下面的接口开启或关闭音视频录制
::

    /**
     * 开关视频录制
     *
     * @param enable 是否开启视频录制
     * @param recordParam 录制参数，当 enable 为 true 时，可以更改由 join 时传入的录制参数，不需更改则填 null
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean enableRecord(boolean enable, RecordParam recordParam);


示例代码::


    public void onMediaChannelPropertyChange(JCMediaChannel.PropChangeParam propChangeParam) {
        if (changeParam.record) { // 录制状态变化
            // 根据音视频录制状态判断是否开启音视频录制
            if (mediaChannel.getRecordState() = JCMediaChannel.RECORD_STATE_NONE) {
                // 无法进行音视频录制
            } else if (mediaChannel.getRecordState() = JCMediaChannel.RECORD_STATE_READY) {
                // 可以开启音视频录制
                mediaChannel.enableRecord(true, null);
            } else if (mediaChannel.getRecordState() = JCMediaChannel.RECORD_STATE_RUNNING) {
                // 音视频录制中，可以关闭音视频录制
                mediaChannel.enableRecord(false, null);
            }
        }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _发送消息(android):

发送消息
----------------------

如果想在频道中给其他成员发送消息，可以调用下面的接口
::

    /**
     * 发送消息
     *
     * @param type     消息类型
     * @param content  消息内容，当 toUserId 不为 null 时，content 不能大于 4k
     * @param toUserId 接收者id，null则发给频道所有人员
     * @return true表示成功，false表示失败
     */
    public abstract boolean sendMessage(String type, String content, String toUserId);

其中，消息类型（type）为自定义类型。


示例代码::

    public void onJoin(boolean result, @JCMediaChannel.MediaChannelReason int reason, String channelId) {
        // 发送给所有成员
        mediaChannel.sendMessage("text", "content", null);
        // 发送给某个成员
        mediaChannel.sendMessage("text", "content", "userId");
    }

当频道中的其他成员收到消息时会收到 onMessageReceive 回调
::

    /**
     * 接收频道消息的回调
     *
     * @param type          消息类型
     * @param content       消息内容
     * @param fromUserId    消息发送成员的userId
     */
    public void onMessageReceive(String type, String content, String fromUserId);


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _Sip 邀请(android):

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

    /**
     * 邀请Sip用户，一般用于对接落地网关等
     *
     * @param userId    一般为号码
     * @param sipParam  sip参数对象
     * @return          成功返回值 >= 0，失败返回 -1
     */
    public abstract int inviteSipUser(String userId, SipParam sipParam);

其中，SipParam 对象有以下属性
::

        // SIP呼叫 主叫号码
        public String callerNumber = "";
        // 核心网ID
        public String coreNetwork = "";
        // 额外信息
        public Extra extra;

Extra 有以下属性和方法
::

    // JCMediaChannel.inviteSipUser 参数 userId 是号码还是 sipUri
    public boolean sipUri;
    // sipUri 为 true 才生效，决定 sip 信令是否路由到 userId 的 sip 域里
    public boolean route;
    // sip用户加入会议后的昵称
    public String displayName;
    // JCMediaChannel.inviteSipUser 参数 userId 是否为 Mcu 会议
    public boolean mcu;
    // 是否需要视频接入
    public boolean video;
    // dtmf 密码
    public String dtmfPassowrd;

    // 生成json字符串
    public String toParamString()

示例代码
::

    JCMediaChannel.SipParam sipParam = new JCMediaChannel.SipParam();
    sipParam.callerNumber = "ConfSipCallerNumber";
    sipParam.coreNetwork = "ConfSipCoreNetwork";
    sipParam.extra = new JCMediaChannel.SipParam.Extra();
    sipParam.extra.sipUri = "ConfSipSipUri";
    sipParam.extra.route = "ConfSipRoute";
    sipParam.extra.mcu = "ConfSipMcu";
    sipParam.extra.video = "ConfSipVideo";
    sipParam.extra.displayName = "ConfSipDisplayName";
    sipParam.extra.dtmfPassowrd = "ConfSipDtmfPassowrd";
    mediaChannel.inviteSipUser("userId", sipParam);

