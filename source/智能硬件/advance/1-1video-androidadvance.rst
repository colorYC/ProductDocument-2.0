Android 进阶
=========================

.. highlight:: java

**集成进阶功能前，请确保您已经集成了基础的音视频通话功能**

.. note:: SDK 不支持模拟器运行，请使用真机。

.. _通话录音(android):

通话录音
-----------------------------

可以在通话中进行录音，开启或关闭录音需要根据当前的录音状态（audioRecord）来决定。如果正在录制或者通话被挂起或者挂起的情况下，不能进行音频录制。录音状态（audioRecord）可通过 `JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCallItem.html>`_ 对象中的 getAudioRecord() 方法获得。

开启或关闭录音接口如下
::

    /**
     * 语音通话录音，通过 JCCallItem 对象中的audioRecord状态来决定开启关闭录音
     *
     * @param item      JCCallItem 对象
     * @param enable    开启关闭录音
     * @param filePath  录音文件路径
     * @return          返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean audioRecord(JCCallItem item, boolean enable, String filePath);


示例代码::

        JCCallItem item = call.getCallItems().get(0);
        if (item.getAudioRecord()) {
            // 录音结束
            call.audioRecord(item, false, "your filePath");
        } else {
            // 创建录音保存文件路径
            String filePath; // 录音文件的绝对路径，SDK会自动创建录音文件
            if (!TextUtils.isEmpty(filePath)) {
                // 开始录音
                call.audioRecord(item, true, filePath);
            }
        }

开启或关闭录音时，录音状态会发生改变，并通过 onCallItemUpdate 回调上报 
::

    /**
     * 通话状态更新回调（当上层收到此回调时，可以根据 JCCallItem 对象获得该通话的所有信息及状态，从而更新该通话相关UI）
     *
     * @param item JCCallItem 对象
     * @param changeParam 更新标识类
     */
    void onCallItemUpdate(JCCallItem item, JCCallItem.ChangeParam changeParam);


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _视频通话录制(android):

视频通话录制
----------------------------

菊风一对一通话视频录制为您提供 **本地视频源录制和远端视频源录制**。

开启或者关闭视频录制需要根据本端视频源录制状态（localVideoRecord）和远端视频源录制状态（remoteVideoRecord）来决定。视频录制状态可通过 `JCCallItem <https://developer.juphoon.com/portal/reference/V2.0/android/com/juphoon/cloud/JCCallItem.html>`_ 对象中的 getLocalVideoRecord() 方法和 getRemoteVideoRecord() 方法获得。此外，您可以对录制视频的高度和宽度以及存储路径进行设置，接口如下：
::

    /**
     * 视频通话录制，通过 JCCallItem 对象中的localVideoRecord和remoteVideoRecord状态来决定开启关闭录制
     *
     * @param item      JCCallItem 对象
     * @param enable    开启关闭录制
     * @param remote    是否为远端视频源
     * @param width     录制视频宽像素
     * @param height    录制视频高像素
     * @param filePath  录制视频文件存储路径
     * @return          返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean videoRecord(JCCallItem item, boolean enable, boolean remote, int width, int height, String filePath);

示例代码::

    public void onVideoRecord() {
        JCCallItem item = call.getCallItems().get(0);
        if (item.getLocalVideoRecord()) {
            if (call.videoRecord(item, false, false, 0, 0, "your filePath")) { // 本端录制结束
                ...
            } else {
                // 本端录制结束失败
            }
        } else if (item.getRemoteVideoRecord()) {
            if (call.videoRecord(item, false, true, 0, 0, "your filePath")) { // 远端录制结束
                ...
            } else {
               // 远端录制结束失败
            }
        } else {
            // 创建视频录制文件保存路径
            String filePath; // 视频录制文件的绝对路径，SDK会自动创建视频录制文件
            if (!TextUtils.isEmpty(filePath)) {
                if (!call.videoRecord(item, true, false, 640, 360, filePath)) {
                    // 视频录制失败
                    File file = new File(filePath);
                    file.delete();
                }
            } else {
                // 创建视频录制文件保存路径失败
            }
        }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _截屏(android):

截屏
------------------------------

在视频通话中，如果想对当前的通话界面进行保存，可以使用截屏功能，截屏分为 **本端视频源截图和远端视频源截图**，接口如下：

::

    /**
     * 视频通话截图
     *
     * @param width     截屏宽度像素，-1为视频源像素
     * @param height    截屏高度像素，-1为视频源像素
     * @param filePath  文件路径
     */
    public boolean snapshot(int width, int height, String filePath)

示例代码::
  
        JCMediaDeviceVideoCanvas canvas = mediaDevice.startVideo(renderId, JCMediaDevice.RENDER_FULL_CONTENT);
        String filePath; // 截屏文件的绝对路径，SDK会自动创建截屏文件
        canvas.snapshot(-1, -1, filePath);


^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _文件视频源(android):

使用文件作为视频输入源
------------------------------

在视频通话中，可以将文件作为视频输入源，典型的场景如在线课堂的文稿显示，文件和摄像头作为视频输入源 **只能存在一种**，如果将文件作为视频输入源，而当前摄像头已开启的话，则会关闭摄像头。

开启视频文件作为视频输入源之前调用下面的方法获取文件视频源是否开启
::

    /**
     * 文件视频源是否开启
     *
     * @return 是否开启文件视频源
     */
    public abstract boolean isVideoFileOpen();

如果未开启，调用下面的接口开启视频文件作为视频输入源
::

    /**
     * 开启视频文件作为视频输入源，文件和摄像头作为视频输入源只能存在一种，当前摄像头开启的话会关闭摄像头
     *
     * @return 是否成功
     */
    public abstract boolean startVideoFile();

向文件视频源逐帧添加视频数据
::

    /**
     * 逐帧采集视频画面
     *
     * 当为 H264 格式时
     * 1. 如果是关键帧需要将 0x67 0x68 0x41 的数据作为一帧传入
     * 2. 关键帧要以固定间隔传入，例如5秒，否则一开始可能有几秒对端无法显示视频
     *
     * @param data 画面二进制数据
     * @param format   视频像素格式
     * @param width    宽
     * @param height   高
     * @param angle    90 的倍数
     * @param mirror   0 不镜像，1进行左右镜像
     * @param keyFrame 是否为关键帧，针对 format 为 H264
     */
    public abstract void setVideoFileFrame(byte[] data, @VideoPixelFormat int format, int width, int height, int angle, int mirror, boolean keyFrame);


开启文件作为视频输入源之后，对端通过调用下面的接口获得文件视频源渲染id
::

    /**
     * 获得文件视频源渲染id
     *
     * @return 视频源渲染id
     */
    public abstract String getVideoFileId();

之后通过 startVideo 方法渲染远端视频画面
::

    /**
     * 获得视频对象，通过此对象能获得视图用于UI显示
     *
     * @param videoSource   渲染标识串，比如 JCMediaChannelParticipant JCCallItem 中的 renderId，当videoSource 为 videoFileId 时，内部会调用 startVideoFile
     * @param renderType    渲染模式
     * @return              JCMediaDeviceVideoCanvas 对象
     * @see RenderType
     */
    public abstract JCMediaDeviceVideoCanvas startVideo(String videoSource, @RenderType int renderType);


如果想关闭文件视频源，调用下面的接口
::

    /**
     * 关闭逐帧采集画面
     *
     * @return ture表示关闭成功，false表示关闭失败
     */
    public abstract boolean stopVideoFile();

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

通话人数设置
-----------------------------

发起通话前可以通过 maxCallNum 属性设置通话的最大人数，默认为 1。
::

    call.maxCallNum = 2;

当通话超过最大数呼出会失败，收到来电会自动拒绝。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

发送消息
-----------------------------

调用下面的接口在通话中实现发消息的功能
::

    /**
     * 通过通话建立的通道发送数据
     *
     * @param item      需要发送数据的 JCCallItem 对象
     * @param type      文本消息类型，用户可以自定义，例如text，xml等
     * @param content   文本内容
     * @return          返回 true 表示正常执行调用流程，false 表示调用异常
     */
    public abstract boolean sendMessage(JCCallItem item, String type, String content);

当通话中收到消息时，会收到 onMessageReceive 回调
::

    /**
     * 通话中收到消息的回调
     *
     * @param type    消息类型
     * @param content 消息内容
     * @param item    JCCallItem 对象
     */
    void onMessageReceive(String type, String content, JCCallItem item);

示例代码::

    JCCallItem item = call.getActiveCallItem();
    call.sendMessage("text", "消息内容", item);

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _推送(android):

推送
-----------------------------

通过集成推送，可以将通话信息即时告知用户，从而高通话的接通率。推送分为 Android 端的小米推送、华为推送以及谷歌的 FCM 推送，详细集成步骤请参考 :ref:`推送<推送>` 模块。