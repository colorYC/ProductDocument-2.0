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