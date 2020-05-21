Windows
==========================

.. highlight:: csharp

**集成屏幕共享功能前，请确保您已经集成了多方音视频通话。**

.. _屏幕共享(windows):

桌面或窗口共享可以让您和频道中的其他成员一起分享设备里的精彩内容，您可以在频道中利用桌面或窗口共享的功能进行文档演示、在线教育演示、视频会议以及游戏过程分享等。

设置要共享的桌面或窗口id
----------------------------

开启桌面或窗口共享前需要先设置要共享的桌面或窗口id

获取桌面列表或者窗口列表
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


桌面列表或者窗口列表获取后，通过 JCMediaDevice 类的 screenshareWindow 属性设置要共享的窗口或桌面id
::

        /// <summary>
        /// 屏幕分享的窗口
        /// </summary>
        public JCMediaDeviceWindow screenshareWindow

示例代码
::

    mediaDevice.screenshareWindow = desktops[0];
    string windowId = mediaDevice.screenshareWindow.windowId;


屏幕共享采集属性设置
-----------------------------

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
-----------------------------

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
-----------------------------

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


