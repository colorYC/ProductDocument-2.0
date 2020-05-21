Windows 进阶
==========================

.. highlight:: csharp

**集成进阶功能前，请确保您已经集成了基础的音视频通话功能**

.. _通话录音(windows):

通话录音
-----------------------------

可以在通话中进行录音，开启或关闭录音需要根据当前的录音状态（audioRecord）来决定。如果正在录制或者通话被挂起或者挂起的情况下，不能进行音频录制。录音状态可通过 `JCCallItem <http://developer.juphoon.com/portal/reference/ios/Classes/JCCallItem.html>`_ 对象获取。

开启或关闭录音接口如下
::

        /// <summary>
        /// 通话录音，通过JCCallItem对象中的呼叫保持状态来决定开启关闭呼叫保持
        /// </summary>
        /// <param name="item">JCCallItem对象</param>
        /// <param name="enable">开启关闭录音</param>
        /// <param name="filePath">录音文件路径</param>
        /// <returns>返回true表示正常执行调用流程，false表示调用异常</returns>
        public bool audioRecord(JCCallItem item, bool enable, string filePath)


示例代码::

        JCCallItem item = call.getActiveCallItem();
        if (item.audioRecord)
        {
            // 录音结束
            call.audioRecord(item, false, "your filePath");
        } else {
            // 创建录音文件保存路径
            String filePath; // 录音文件的绝对路径，SDK会自动创建录音文件
            if (filtPath.Length > 0)
            {
                // 开始录音
                call.audioRecord(item, true, filePath);
            }
        }
    }


开启或关闭录音时，录音状态会发生改变，并通过 onCallItemUpdate 回调上报 
::

        /// <summary>
        /// 通话状态更新回调
        /// 当上层收到此回调时，可以根据JCCallItem对象获得该通话所有信息及状态，从而更新通话相关UI
        /// </summary>
        /// <param name="item">JCCallItem对象</param>
        /// <param name="changeParam">更新标识类</param>
        void onCallItemUpdate(JCCallItem item, JCCallItem.ChangeParam changeParam);


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _视频通话录制(windows):

视频通话录制
----------------------------

视频通话录制提供 **本地视频源录制和远端视频源录制**。

开启或者关闭视频录制需要根据本端视频源录制状态（localVideoRecord）和远端视频源录制状态（remoteVideoRecord）来决定。视频录制状态可通过 `JCCallItem <http://developer.juphoon.com/portal/reference/V2.0/windows/html/0267696e-79ee-8d46-c086-3c071a2b2b3a.htm>`_ 对象获得。您可以对录制视频的高度和宽度以及存储路径进行设置，接口如下：
::

        /// <summary>
        /// 视频通话录制，通过JCCallItem对象中的localVideoRecord，remoteVideoRecord状态来决定开启关闭录制
        /// </summary>
        /// <param name="item">JCCallItem对象</param>
        /// <param name="enable">开启关闭录制</param>
        /// <param name="remote">是否为远端视频源</param>
        /// <param name="width">录制视频宽像素</param>
        /// <param name="height">录制视频高像素</param>
        /// <param name="filePath">录制视频文件存储路径</param>
        /// <param name="bothAudio">是否录制两端音频，false 表示只录制视频端音频</param>
        /// <returns>返回true表示正常执行调用流程，false表示调用异常</returns>
        public bool videoRecord(JCCallItem item, bool enable, bool remote, int width, int height, string filePath, bool bothAudio)

.. note:: 参数 remote 值为 true 时为远端视频源录制，值为 false 时为本地视频源录制。

示例代码::

        JCCallItem item = call.getActiveCallItem();
        if (item.audioRecord)
        {
            // 视频录制结束
            call.videoRecord(item, false, true, 0, 0, "your filePath", true);
        } else {
            // 创建视频录制文件保存路径
            String filePath; // 视频录制文件的绝对路径，SDK会自动创建视频录制文件
            if (filtPath.Length > 0)
            {
                // 开始视频录制
                call.videoRecord(item, true, true, 640, 360, filePath, true);
            }
        }
    }


开启或关闭视频录制时，录制状态会发生改变，并通过 onCallItemUpdate 回调上报 
::

        /// <summary>
        /// 通话状态更新回调
        /// 当上层收到此回调时，可以根据JCCallItem对象获得该通话所有信息及状态，从而更新通话相关UI
        /// </summary>
        /// <param name="item">JCCallItem对象</param>
        /// <param name="changeParam">更新标识类</param>
        void onCallItemUpdate(JCCallItem item, JCCallItem.ChangeParam changeParam);


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _截屏(windows):

截图
------------------------------

在视频通话中，如果想对当前的通话界面进行保存，可以使用截图功能，截图分为 **本端视频源截图和远端视频源截图**，接口如下：

::

        /// <summary>
        /// 视频通话截图
        /// </summary>
        /// <param name="width">截屏宽度像素，-1为视频源像素</param>
        /// <param name="height">截屏高度像素，-1为视频源像素</param>
        /// <param name="filePath">文件路径</param>
        /// <returns>是否成功</returns>
        public bool snapshot(int width, int height, string filePath)

示例代码::

    JCMediaDeviceVideoCanvas remoteCanvas = mediaDevice.startVideo(renderId, JCMediaDeviceRenderMode.FULLSCREEN);
    String filePath; // 截屏文件的绝对路径，SDK会自动创建截屏文件
    RemoteCanvas.snapshot(-1, -1, filePath);


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

通话人数设置
-----------------------------

发起通话前可以通过 maxCallNum 属性设置通话的最大人数，默认为 1。
::

    call.maxCallNum = 2;

当通话超过最大人数呼出会失败，收到来电会自动拒绝。

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

发送消息
-----------------------------

调用下面的接口在通话中实现发消息的功能
::

        /// <summary>
        /// 通过通话建立后的通道发送数据
        /// </summary>
        /// <param name="item">需要发送数据的JCCallItem对象</param>
        /// <param name="type">文本消息类型，用户可以自定义，例如text，xml等</param>
        /// <param name="content">文本内容</param>
        /// <returns>返回 true 表示正常执行调用流程，false 表示调用异常</returns>
        public bool sendMessage(JCCallItem item, string type, string content)

当通话中收到消息时，会收到 onMessageReceive 回调
::

        /// <summary>
        /// 通话中收到消息的回调
        /// </summary>
        /// <param name="type">消息类型</param>
        /// <param name="content">消息内容</param>
        /// <param name="item">JCCallItem对象</param>
       void onMessageReceive(string type, string content, JCCallItem item);

示例代码::

    JCCallItem item = call.getActiveCallItem();
    call.sendMessage(item, "text", "消息内容");