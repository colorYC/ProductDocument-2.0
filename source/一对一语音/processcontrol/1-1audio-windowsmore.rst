Windows
============================

.. highlight:: csharp

.. _通话状态更新(windows1-1):

本节内容提供通话中的状态更新以及通话过程控制的接口说明。

通话状态更新
-----------------------------

通话过程中，如果通话状态发生了改变，如开启关闭静音、开启关闭通话保持、活跃状态切换、开启关闭视频流发送、网络变化等，将会收到通话状态更新的回调
::

        /// <summary>
        /// 通话状态更新回调
        /// 当上层收到此回调时，可以根据JCCallItem对象获得该通话所有信息及状态，从而更新通话相关UI
        /// </summary>
        /// <param name="item">JCCallItem对象</param>
        /// <param name="changeParam">更新标识类</param>
        void onCallItemUpdate(JCCallItem item, JCCallItem.ChangeParam changeParam);

关于 ChangeParam 的说明请参考 JCCallItem.cs 文件。

.. note::

       静音状态、通话保持状态、活跃状态可通过 `JCCallItem <http://developer.juphoon.com/portal/reference/V2.0/windows/html/0267696e-79ee-8d46-c086-3c071a2b2b3a.htm>`_ 对象获得。

示例代码::

    public void onCallItemUpdate(JCCallItem item, JCCallItem.ChangeParam changeParam) {
        if (item.mute) { // 开启静音
            ...
        } else if (item.hold) { // 挂起通话变化
            ...
        } else if (item.held) { // 被挂起变化
            ...
        } else if (item.active) { // 激活状态变化
            ...
        } else if (item.netStatus) { // 网络状态变化
            ...
        }
    }


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _通话过程控制(windows1-1):

通话过程控制
-----------------------------

通话静音
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

通过下面的方法开启或关闭静音，开启关闭静音需要根据 JCCallItem 中的静音状态（`mute <http://developer.juphoon.com/portal/reference/V2.0/windows/html/bb1ed5b7-2f76-e89d-f964-328e2b746904.htm>`_）来决定，静音开启后，对方将听不到您的声音
::

        /// <summary>
        /// 静音，通过JCCallItem中的静音状态来决定开启关闭
        /// </summary>
        /// <param name="item">JCCallItem对象</param>
        /// <returns>返回true表示正常执行调用流程，false表示调用异常</returns>
        public bool mute(JCCallItem item)


开启关闭呼叫保持
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

调用下面的方法对通话对象进行呼叫保持或解除呼叫保持（当通话对象处于被保持状态（即状态为held）时不可以进行此操作），开启或关闭呼叫保持需要根据 JCCallItem 对象中（`hold <http://developer.juphoon.com/portal/reference/V2.0/windows/html/dc13e9d5-2842-1b22-5d6d-9a617d321458.htm>`_）的呼叫保持状态来决定
::

        /// <summary>
        /// 呼叫保持，通过JCCallItem对象中的呼叫保持状态来决定开启关闭
        /// </summary>
        /// <param name="item">JCCallItem对象</param>
        /// <returns>返回true表示正常执行调用流程，false表示调用异常</returns>
        public bool hold(JCCallItem item)


切换活跃通话
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

调用下面的方法对通话中被保持的对象和活跃的通话对象进行切换

::

        /// <summary>
        /// 切换活跃通话
        /// </summary>
        /// <param name="item">需要变活跃的JCCallItem对象</param>
        /// <returns>返回true表示正常执行调用流程，false表示调用异常</returns>
        public bool becomeActive(JCCallItem item)

示例代码
::

    //获取活跃通话对象
    JCCallItem item = call.getActiveCallItem();
    call.mute(item);
    call.hold(item);
    call.becomeActive(item);
