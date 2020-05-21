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