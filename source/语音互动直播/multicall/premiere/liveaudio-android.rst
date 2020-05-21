Android
===========================

.. _多方通话-Android:

.. highlight:: java

前提条件
----------------------------------

- Android SDK API 等级 16 或以上

- 支持 Android 4.1 或以上版本的移动设备

- 有效的菊风开发者账号（`免费注册 <http://developer.juphoon.com/signup>`_ ）


准备工作
----------------------------

开始之前，请您先做好如下准备工作：

SDK 下载
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

点击 `Android SDK <http://developer.juphoon.com/document/cloud-communication-android-sdk#2>`_ 进行下载。

AppKey 获取
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

AppKey 是应用在菊风云平台中的唯一标识。需要在 SDK 初始化的时候使用，AppKey 获取请参考 :ref:`创建应用 <创建应用>` 。


SDK 配置
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

有两种方式集成 JC SDK：

**方法一：使用 JCenter 自动集成**

.. note:: 该方法仅适用于 2.0 及以上版本的 JC SDK 集成。
 
在项目的 /app/build.gradle 文件中，添加如下行：

::

    ...
    dependencies {
        ...
        // 可通过 SDK 发版说明取得最新版本号
        implementation 'com.JuphoonCloud:JC-SDK:2.0'
    }


**方法二：手动导入 JC SDK**

1. 下载 SDK，拷贝 libs 文件夹内的 armeabi-v7a、X86、mtc.jar 、JCSDK.jar 和 zmf.jar 到您工程目录中的 libs 目录下，并打开工程，如下图所示:

.. image:: images/android_sdklist.png

.. image:: images/quickstart_android1.png

2. 为能连接到我们的 so 库，在您工程 build.gradle 文件中确保增加以下配置，如图:

.. image:: images/set_sdk_android2.png

3. 修改您工程中 Application 配置文件 AndroidManifest.xml，**请确保已经加入以下特性和权限信息**。具体信息可以参考 :ref:`Android 权限说明<Android 权限说明>` 。
::

    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.VIBRATE"/>
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />

.. note::

    您在 AndroidManifest 中进行权限配置时，请确保您能够获得打开摄像头、音视频录制等相关权限。

4. 配置完成后编译运行，如果没有报错，恭喜您，您已经成功配置 SDK，可以进行 SDK 初始化了。

.. note:: SDK 不支持模拟器运行，请使用真机。


SDK 初始化
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

.. _Android SDK 初始化:

在使用 SDK 之前，需要进行 SDK 的初始化。在 App 的整个生命周期中，开发者只需要将 SDK 初始化一次。

首先在需要使用 JC SDK 的地方引入相关的类::

    import com.juphoon.cloud.JCClient;

在实现初始化的文件中实现 JCClientCallback 回调，用于接收 JCClient 相关通知。

在初始化的时候还可以设置 SDK 信息存储目录，日志路径以及日志打印的等级，具体通过 createParam 对象设置，如果不设置则使用默认值。

CreateParam 类有以下属性
::

    /**
     * SDK 相关信息存放目录，包括账号信息，日志信息等，默认为 sdcard/Android/data/包名/files 下
     */
    public String sdkInfoDir;
    /**
     * SDK 日志目录，如果不设置则默认在 sSdkInfoDir 下的 log 目录中
     */
    public String sdkLogDir;
    /**
     * 是否内部自动加载so库，默认为 true，如果上层需要自己加载则设置为 false
     */
    public boolean needLoadLibrary = true;
    /**
     * sdk 日志等级
     */
    public @LogLevel int sdkLogLevel;

日志等级 sdkLogLevel 有以下几种
::

    /**
     * disable日志等级
     */
    public static final int LOG_LEVEL_DISABLE = 0;
    /**
     * error日志等级
     */
    public static final int LOG_LEVEL_ERROR = 1;
    /**
     * info日志等级
     */
    public static final int LOG_LEVEL_INFO = 2;
    /**
     * debug日志等级
     */
    public static final int LOG_LEVEL_DEBUG = 3;

调用下面的接口初始化 SDK
::

    /**
     * 创建 JCClient 实例
     *
     * @param context   上下文句柄
     * @param appKey    用户从 Juphoon Cloud 平台上申请的 AppKey 字符串
     * @param callback    回调接口，用于接收 JCClient 相关通知
     * @param createParam 创建参数，null 则按默认值创建
     * @return JCClient 对象
     */
    public static JCClient create(@NonNull Context context, @NonNull String appKey, @NonNull JCClientCallback callback, CreateParam createParam) {

.. note::

       appKey 为准备工作中“获取 AppKey”步骤中取得的 AppKey。如果还未获取 AppKey，请参考 :ref:`创建应用 <创建应用>` 来获取。


示例代码::

    public boolean initialize(Context context) {
        // 初始化各模块，因为这些模块实例将被频繁使用，建议声明在单例中
        JCClient.CreateParam createParam = new JCClient.CreateParam(this);
        createParam.sdkLogLevel = LOG_LEVEL_INFO;
        createParam.sdkInfoDir = "SDK 信息存放路径";
        createParam.sdkLogDir = "日志存放路径";
        JCClient client = JCClient.create(Context, "your appkey", this, createParam);
        return true;
    }

SDK 初始化之后，可以调用下面的方法获取创建参数
::

    /**
     * 创建参数
     * @return 创建参数
     */
    public abstract CreateParam getCreateParam();

示例代码
::

    JCClient.CreateParam createParam = client.getCreateParam();

SDK 初始化之后，即可进行登录的集成。


登录
----------------------------------

.. _Android 登录:

登录涉及 JCClient 类及其回调 JCClientCallback，其主要作用是负责登录、登出管理及帐号信息存储。

登录之前，可以通过 loginParam 登录参数进行登录的相关配置，如服务器地址的设置或者使用代理服务器登录，如不设置则按照默认值登录，具体如下：

::

        JCClient.LoginParam loginParam = new JCClient.LoginParam();
        //默认国内环境 http:cn.router.justalkcloud.com:8080
        loginParam.serverAddress = "服务器地址";
        //如果使用代理服务器登录
        loginParam.httpsProxy = "代理服务器地址";

其中，服务器地址包括国际环境服务器地址和国内环境服务器地址：

**国际环境** 服务器地址默认为 ``http:intl.router.justalkcloud.com:8080`` 。

**国内环境** 服务器地址默认为 ``http:cn.router.justalkcloud.com:8080`` 。

开发者可以使用自定义服务器地址。

还可以通过 setDisplayName 接口设置昵称
::

    /**
     * 设置昵称，用于通话，消息等，可以更直观的表明身份
     * @param displayName 昵称
     */
    public abstract void setDisplayName(@NonNull String displayName);

示例代码::

    client.setDisplayName("小张");


发起登录
>>>>>>>>>>>>>>>>>>>>>>>>>>>

登录参数设置之后，即可调用 login 接口发起登录操作，userId 为英文、数字和'+' '-' '_' '.'，大小写不敏感，长度不要超过64字符，'-' '_' '.'不能作为第一个字符
::

    /**
     * 登陆 Juphoon Cloud 平台，只有登陆成功后才能进行平台上的各种业务<br>
     * 登陆结果通过 JCCallCallback 通知<br>
     * 注意:用户名为英文数字和'+' '-' '_' '.'，长度不要超过64字符，'-' '.' '_'字符不能处于第一位<br>
     *
     * @param userId   用户名
     * @param password 密码，但不能为空
     * @param loginParam 登录参数，null 则按默认值
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常，异常错误通过 JCClientCallback 通知
     */
    public abstract boolean login(@NonNull String userId, @NonNull String password, LoginParam loginParam);

其中，LoginParam 类有以下属性
::

        /**
         * 登录服务器地址
         */
        public String serverAddress = "http:cn.router.justalkcloud.com:8080";
        /**
         * https代理地址，例如 192.168.1.100:3128
         */
        public String httpsProxy;
        /**
         * 设备id，一般模拟器使用，因为模拟器可能获得的设备id都一样
         */
        public String deviceId;
        /**
         * 登录账号不存在的情况下是否内部自动创建该账号，默认为 true
         */
        public boolean autoCreateAccount = true;


示例代码：
::

        JCClient.LoginParam loginParam = new JCClient.LoginParam();
        //默认国内环境 http:cn.router.justalkcloud.com:8080
        loginParam.serverAddress = "http:cn.router.justalkcloud.com:8080";
        client.login("账号", "123", loginParam);

登录成功之后，首先会触发登录状态改变（onClientStateChange）回调
::

    /**
     * 登录状态变化通知
     *
     * @param state    当前状态值
     * @param oldState 之前状态值
     */
    void onClientStateChange(@JCClient.ClientState int state, @JCClient.ClientState int oldState);

ClientState 有::

    // 未初始化
    public static final int STATE_NOT_INIT = 0;
    // 未登录
    public static final int STATE_IDLE = 1;
    // 登录中
    public static final int STATE_LOGINING = 2;
    // 登录成功
    public static final int STATE_LOGINED = 3;
    // 登出中
    public static final int STATE_LOGOUTING = 4;


示例代码::

    public void onClientStateChange(@JCClient.ClientState int state, @JCClient.ClientState int oldState) {
         if (state == JCClient.STATE_IDLE) { // 未登录
           ...
        } else if (state == JCClient.STATE_LOGINING) { // 正在登录
           ...
        } else if (state == JCClient.STATE_LOGINED) { // 登录成功
           ... 
        } else if (state == JCClient.STATE_LOGOUTING) { // 登出中
           ...
        }
    }


之后通过 onLogin 回调上报登录结果
::

    /**
     * 登陆结果回调
     *
     * @param result true 表示登陆成功，false 表示登陆失败
     * @param reason 当 result 为 false 时该值有效
     */
    void onLogin(boolean result, @JCClient.ClientReason int reason);

其中，ClientReason 有
::

    /**
     * 正常
     */
    public static final int REASON_NONE = 0;
    /**
     * sdk 未初始化
     */
    public static final int REASON_SDK_NOT_INIT = 1;
    /**
     * 无效参数
     */
    public static final int REASON_INVALID_PARAM = 2;
    /**
     * 函数调用失败
     */
    public static final int REASON_CALL_FUNCTION_ERROR = 3;
    /**
     * 当前状态无法再次登录
     */
    public static final int REASON_STATE_CANNOT_LOGIN = 4;
    /**
     * 超时
     */
    public static final int REASON_TIMEOUT = 5;
    /**
     * 网络异常
     */
    public static final int REASON_NETWORK = 6;
    /**
     * appkey 错误
     */
    public static final int REASON_APPKEY = 7;
    /**
     * 账号密码错误
     */
    public static final int REASON_AUTH = 8;
    /**
     * 无该用户
     */
    public static final int REASON_NOUSER = 9;
    /**
     * 强制登出
     */
    public static final int REASON_SERVER_LOGOUT = 10;
    /**
     * 其他错误
     */
    public static final int REASON_OTHER = 100;

登录成功之后，SDK 会自动保持与服务器的连接状态，直到用户主动调用登出接口，或者因为帐号在其他设备登录导致该设备登出。

登录之后可以通过下面的方法获取昵称、用户标识以及登录参数
::

    /**
     * 获取昵称
     *
     * @return 昵称
     */
    public abstract String getDisplayName();

    /**
     * 获得用户标识
     *
     * @return 用户标识
     */
    public abstract String getUserId();

    /**
     * 登录参数，只有调用登录接口后会有值，登出后为 nil
     * @return 登录参数
     */
    public abstract LoginParam getLoginParam();

示例代码::

        JCClient.CreateParam createParam = client.getCreateParam();
        JCClient.LoginParam loginParam = client.getLoginParam();
        String displayName = client.getDisplayName();
        String userId = client.getUserId();


登出
>>>>>>>>>>>>>>>>>>>>>>>>>>>

登出调用下面的方法，登出后不能进行平台上的各种业务操作
::

    /**
     * 登出 Juphoon Cloud 平台，登出后不能进行平台上的各种业务
     *
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常，异常错误通过 JCClientCallback 通知
     */
    public abstract boolean logout();

登出同样会触发登录状态改变（onClientStateChange）回调

之后将通过 onlogout 回调上报登出结果
::

    /**
     * 登出回调
     *
     * @param reason 登出原因
     */
    void onLogout(@JCClient.ClientReason int reason);

集成登录后，即可进行相关业务的集成。


``SDK 支持前后台模式，可以在应用进入前台或者后台时调用 JCClient 类中的 setForeground 方法进行设置``

::

    /**
     * 设置是否为前台, 在有控制后台网络的手机上当进入前台时主动触发
     *
     * @param foreground 是否为前台
     */
    public abstract void setForeground(boolean foreground);

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

完成以上步骤，就做好了基础工作，您可以开始集成业务了。

业务集成
----------------------------------

**相关类说明**

语音互动直播涉及以下类：

.. list-table::
   :header-rows: 1

   * - 名称
     - 描述
   * - `JCMediaChannel <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannel.html>`_
     - 媒体频道模块，类似音视频房间的概念，可以通过频道号加入此频道，从而进行音视频通话
   * - `JCMediaChannelParticipant <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannelParticipant.html>`_
     - 媒体频道成员，主要用于成员基本信息以及状态等的管理
   * - `JCMediaChannelQueryInfo <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannelQueryInfo.html>`_
     - 媒体频道查询信息结果
   * - `JCMediaChannelCallback <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaChannelCallback.html>`_
     - 媒体频道模块回调代理
   * - `JCMediaDevice <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDevice.html>`_
     - 设备模块，主要用于视频、音频设备的管理
   * - `JCMediaDeviceCallback <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCMediaDeviceCallback.html>`_
     - 设备模块回调代理

更多关于类的详细信息请参考 `API 说明文档 <https://developer.juphoon.com/portal/reference/V2.0/android/>`_ 。

**开始集成语音互动直播功能前，请先实现 JCMediaDeviceCallback, JCMediaChannelCallback 回调，用于接收 JCMediaDevice 和 JCMediaChannel 的相关通知** 

之后进行 ``模块的初始化``

创建 JCMediaChannel 实例
::
    
    /**
     * 创建 JCMediaChannel 对象
     *
     * @param client      JCClient 对象
     * @param mediaDevice JCMediaDevice 对象
     * @param callback    JCMediaChannelCallback 回调接口，用于接收 JCMediaChannel 相关通知
     * @return            返回 JCMediaChannel 对象
     */
    public static JCMediaChannel create(JCClient client, JCMediaDevice mediaDevice, JCMediaChannelCallback callback);


创建 JCMediaDevice 实例
::

    /**
     * 创建 JCMediaDevice 对象
     *
     * @param client   JCClient 对象
     * @param callback JCMediaDeviceCallback 回调接口，用于接收 JCMediaDevice 相关通知
     * @return 返回 JCMediaDevice 对象
     */
    public static JCMediaDevice create(JCClient client, JCMediaDeviceCallback callback)

示例代码
::

    // 初始化各模块，因为这些模块实例将被频繁使用，建议声明在单例中
    JCMediaDevice mediaDevice = JCMediaDevice.create(client, this);
    JCMediaChannel mediaChannel = JCMediaChannel.create(client, mediaDevice, this);


**开始集成**

1. 角色设置
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

加入频道前要先进行角色的设置。其中角色设置包括主播和观众。

角色值可以根据 CustomRole 枚举值进行自定义，CustomRole 有以下几种
::

    /**
     * 无自定义角色
     */
    public static final int CUSTOM_ROLE_NONE = 0;
    /**
     * 自定义角色0
     */
    public static final int CUSTOM_ROLE_0 = 1<<12;
    /**
     * 自定义角色1
     */
    public static final int CUSTOM_ROLE_1 = 1<<13;
    /**
     * 自定义角色2
     */
    public static final int CUSTOM_ROLE_2 = 1<<14;
    /**
     * 自定义角色3
     */
    public static final int CUSTOM_ROLE_3 = 1<<15;

例如::

    //自定义主播角色，根据CustomState枚举值自定义角色
    int ROLE_BROASCASTER = JCMediaChannel.CUSTOM_ROLE_0;
    //自定义观众角色，根据CustomState枚举值自定义角色
    int ROLE_AUDIENCE = JCMediaChannel.CUSTOM_ROLE_1;

角色定义之后，调用下面的接口设置角色
::

    /**
     * 设置自定义角色
     *
     * @param customRole 自定义角色, 参看 CustomRole
     * @param participant 成员，null 则默认设置自己
     */
    public abstract void setCustomRole(@CustomRole int customRole, JCMediaChannelParticipant participant);


自定义角色设置后可以调用下面的方法获取自定义的角色值
::

    /**
     * 获得自定义角色
     *
     * @return
     */
    public abstract @CustomRole int getCustomRole();


2. 发送本地音频流
>>>>>>>>>>>>>>>>>>>>>>>>>>>>

**如果角色为主播，则需要在加入会议前打开音频，上传本地音频流。如果为观众，则不需要。**

在加入频道时，SDK 会 **自动打开音频设备**，因此可以在加入频道之前直接调用 enableUploadAudioStream 方法打开或关闭“上传音频”的标识，这样加入频道后其他成员就可以听到您的声音
::

    /**
     * 开启关闭发送本地音频流
     * 1.在频道中将会与服务器进行交互，服务器会更新状态并同步给其他用户
     * 2.未在频道中则标记是否上传音频流，在join时生效
     * 3.建议每次join前设置
     *
     * @param enable 是否开启本地音频流
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean enableUploadAudioStream(boolean enable);

.. note:: 

        该接口可以在加入频道之前调用，也可以在加入频道之后调用。两者区别具体如下：
         - 如果在加入频道前调用，**只是打开或关闭“上传音频流”的标识，但不会发送数据**，当加入频道成功时会根据 enableUploadAudioStream 设定的值来确定是否上传音频数据。同时，频道中的其他成员会收到该成员“是否上传音频“的状态变化回调（onParticipantUpdate）。
         - 如果在加入频道后调用，则会开启或者关闭发送本地音频流数据，服务器也会根据 enableUploadAudioStream 设定的值来确定是否上传音频数据。同时，频道中的其他成员会收到该成员“是否上传音频“的状态变化回调（onParticipantUpdate）。
        此外，此方法还可以实现开启或关闭静音的功能。当 enable 值为 false ，将会停止发送本地音频流，此时其他成员将听不到本端的声音，从而实现静音功能。


要实现语音直播，还需要在下面的接口中，将发送本地视频流(enableUploadVideoStream)设置为 false 
::

    /**
     * 开启关闭发送本地音频流
     * 1.在频道中将会与服务器进行交互，服务器会更新状态并同步给其他用户
     * 2.未在频道中则标记是否上传音频流，在join时生效
     * 3.建议每次join前设置
     *
     * @param enable 是否开启本地音频流
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean enableUploadAudioStream(boolean enable);


.. note:: 

    该接口可以在加入频道之前调用，也可以在加入频道之后调用。两者区别具体如下：
     - 如果在加入频道前调用，**只是打开或关闭“上传视频流”的标识，但不发送数据**，当加入频道后会根据 enableUploadVideoStream 设定的值来确定是否上传视频流数据。同时，频道中的其他成员会收到该成员”是否上传视频“的状态变化回调（onParticipantUpdate）。如果设定的值为 false，则在加入频道后自动开启语音通话模式。
     - 如果在加入频道后调用，则会开启或关闭发送本地视频流数据。服务器会根据 enableUploadVideoStream 设定的值来确定是否上传视频流数据。同时，频道中的其他成员会收到该成员”是否上传视频“的状态变化回调（onParticipantUpdate），从而进行语音通话和视频通话的切换。
    此外，调用该方法发送本地视频流数据还要依赖摄像头是否已经打开。


3. 加入频道
>>>>>>>>>>>>>>>>>>>>>>>

调用下面的接口加入频道
::

    /**
     * 加入频道
     *
     * @param channelIdOrUri 媒体频道标识或者频道Uri，当 params 中 JOIN_PARAM_URI_MODE 设置为 true 时表示频道 Uri，其他表示频道标识
     * @param joinParam 参数，没有则填null
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常
     * @see MaxResolution
     * @see JoinParam
     */
    public abstract boolean join(String channelIdOrUri, JoinParam joinParam);

其中，JoinParam 为频道参数类，加入频道之前可以通过此类设置频道属性，如频道允许加入的最大人数，推流参数、录制参数等。具体如下：
::

        // 会议最大人数
        public int capacity = 16;
        // 推流参数
        public String cdn = null;
        // 录制参数
        public RecordParam record = null;
        // 密码
        public String password = "123456";
        // uri 模式, join 函数的参数为会议 uri
        public boolean uriMode = false;
        // 心跳间隔
        public int heartbeatTime = 20;
        // 心跳超时
        public int heartbeatTimeout = 60;
        // 自定义属性
        public String customProperty = "";

.. note:: 加入频道会自动打开音频设备。

**示例代码**

::

    //自定义主播角色，根据CustomState枚举值自定义角色
    int ROLE_BROASCASTER = JCMediaChannel.CUSTOM_ROLE_0;
    //自定义观众角色，根据CustomState枚举值自定义角色
    int ROLE_AUDIENCE = JCMediaChannel.CUSTOM_ROLE_1;
    // 设置角色，participant值为nil代表设置自身的角色
    mediaChannel.setCustomRole(ROLE_BROASCASTER, null);
    public void join() {
        // 设置频道参数
        JCMediaChannel.JoinParam joinParam = new JCMediaChannel.JoinParam();
        joinParam.password = "ConfJoinPassword";
        joinParam.smooth = true;
        joinParam.maxResolution = MAX_RESOLUTION_720p;
        //主播可以上传本地音频流，语音直播时，不需要上传本地视频流
        mediaChannel.enableUploadAudioStream(true);
        mediaChannel.enableUploadVideoStream(false);
        // 加入直播
        mediaChannel.join("222", joinParam);
    }


加入频道结果回调
::

    /**
     * 加入频道结果回调
     *
     * @param result    true 表示成功，false 表示失败
     * @param reason    加入失败原因，当 result 为 false 时该值有效
     * @param channelId 频道标识符
     */
    void onJoin(boolean result, @JCMediaChannel.MediaChannelReason int reason, String channelId);


现在您可以开始语音直播了。

直播中如果有新成员加入，会收到 onParticipantJoin 回调，此时可以进行界面更新
::

    /**
     * 新成员加入回调
     *
     * @param participant 成员对象
     */
    void onParticipantJoin(JCMediaChannelParticipant participant);

直播中，还可以与观众互动连麦，请参考 :ref:`互动连麦<互动连麦(android)>`。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

4. 离开频道
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

.. image:: leavechannel.png

如果非主播成员想离开直播，可以调用下面的接口
::

    /**
     * 离开频道
     *
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean leave();

示例代码::

    // 离开频道
    mediaChannel.leave()


离开频道后，UI 监听回调离开的原因
::

    /**
     * 离开频道结果回调
     *
     * @param reason    离开原因
     * @param channelId 频道标识符
     */
    void onLeave(@JCMediaChannel.MediaChannelReason int reason, String channelId);

离开原因枚举值请参考 `JCMediaChannelReason <https://developer.juphoon.com/portal/reference/V2.0/android/>`_。

示例代码::

    // 离开频道回调
    void onLeave(@JCMediaChannel.MediaChannelReason int reason, String channelId)
    {
        // 界面处理
    }


5. 解散频道
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

.. image:: stopchannel.png

如果主播想解散频道，可以调用下面的接口关闭频道，此时所有成员都将被退出
::

    /**
     * 关闭频道，所有成员都将被退出
     *
     * @return 返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean stop();

示例代码
::

    // 结束频道
    mediaChannel.stop()


关闭频道的结果通过 onStop 回调
::

    /**
     * 解散频道结果回调
     *
     * @param result    true 表示成功，false 表示失败
     * @param reason    解散失败原因，当 result 为 false 时该值有效
     */
    void onStop(boolean result, @JCMediaChannel.MediaChannelReason int reason);


