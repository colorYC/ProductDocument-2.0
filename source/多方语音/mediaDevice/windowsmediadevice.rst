.. _设备控制(windows):

音频管理
=======================

.. highlight:: csharp

音频设备管理
------------------------

当前音频输入输出设备
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
::

        /// <summary>
        /// 当前输入设备
        /// </summary>
        public JCMediaDeviceAudio audioInput

        /// <summary>
        /// 当前输出设备
        /// </summary>
        public JCMediaDeviceAudio audioOutput


获取音频输入设备列表
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

::

    /// <summary>
    /// 音频输入设备列表
    /// </summary>
    public List<JCMediaDeviceAudio> audioInputs

其中，JCMediaDeviceAudio 有以下几个变量
::

        /// <summary>
        /// 名称
        /// </summary>
        public string audioName
        /// <summary>
        /// id
        /// </summary>
        public string audioId


获取音频输出设备列表
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

::

    /// <summary>
    /// 音频输出设备列表
    /// </summary>
    public List<JCMediaDeviceAudio> audioOutputs

其中，JCMediaDeviceAudio 有以下几个变量
::

        /// <summary>
        /// 名称
        /// </summary>
        public string audioName
        /// <summary>
        /// id
        /// </summary>
        public string audioId


示例代码::


    // 获取音频输入设备列表
    List<JCMediaDeviceAudio> audioInputDevices = mediaDevice.audioInputs;

    // 获取音频输出设备列表
    List<JCMediaDeviceAudio> audioOutputDevices = mediaDevice.audioOutputs;


开启/关闭音频设备
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

::

    /// <summary>
    /// 启动音频，一般正式开启通话前需要调用此接口
    ///</summary>
    ///<returns>启动成功失败</returns>
    public bool startAudio()

    /// <summary>
    /// 停止音频，一般在通话结束时调用
    /// </summary>
    /// <returns>停止音频成功失败</returns>
    public bool stopAudio()


获取当前音频输出等级
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

::

        /// <summary>
        /// 获取当前输出等级
        /// </summary>
        /// <returns>音频输出等级</returns>
        public int getSpkLevel()

示例代码::

    // 打开音频
    mediaDevice.startAudio();

    // 关闭音频
    mediaDevice.stopAudio();

    // 获取当前输出等级
    int level = mediaDevice.getSpkLevel(); 
