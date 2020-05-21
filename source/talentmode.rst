媒体参数设置
==================

.. highlight:: objective-c

.. _媒体参数设置:

针对不同设备类型，如手机、智能硬件等，需要设置不同的媒体参数以达到优质的音视频通话效果，菊风提供不同的媒体配置模式供开发者根据不同的场景选择，同时开放媒体设置参数供开发者灵活设置。具体如下：

发起通话前可以选择媒体配置模式，菊风提供如下三种模式供开发者选择
::

    /// 360P
    JCCallMediaConfigMode360P,
    /// 720P
    JCCallMediaConfigMode720P,
    /// 智能硬件场景，比如会和手表通话等
    JCCallMediaConfigModeIntelligentHardware,

其中，手机端可以选择三种模式中的一种，智能硬件一般选择 JCCallMediaConfigModeIntelligentHardware 模式。

在发起通话前，开发者可以根据设备类型选择一种媒体配置模式并通过下面的接口生成配置参数
::

    /// 根据模式生成配置参数
    /// @param mode 模式
    +(JCCallMediaConfig* __nonnull)generateByMode:(JCCallMediaConfigMode)mode;


除了选择提供的模式外，开发者还可以根据对应的属性对媒体参数进行自定义的设置，以满足不同场景的需求。媒体参数包括音频和视频参数。具体说明如下：

**音频参数**

.. list-table::
   :header-rows: 1

   * - 参数
     - 类型
     - 描述
   * - audioEnableCodecs
     - NSString
     - 音频编解码,协商优先级按顺序排列, 每个编解码用";"间隔, 注意大小写
   * - audioAecEnable
     - bool
     - 是否开启回声消除
   * - audioAecMode
     - JCCallAecMode
     - 回声消除模式，具体参考JCCallAecMode枚举值
   * - audioArsEnable
     - bool
     - 是否开启音频
   * - audioArsBitrateMin
     - int
     - 音频最小码率，kb
   * - audioArsBitrateMax
     - int
     - 音频最大码率，kb
   * - audioVad
     - bool
     - 静音检测，非语音以舒适噪声，节省码率从而降低功耗
   * - audioRed
     - bool
     - 
       - 语音red抗丢包, 打开会增加payload码率
       - 关闭增强抗丢包能力节省码率，从而降低功耗
       - 网络不稳定，一般选择打开
   * - audioRxAnr
     - bool
     - 一般接收端声音质量好可关闭噪声抑制，减少声音dsp处理，降低功耗
   * - audioRtx
     - bool
     - rtx重传, 同FEC、NACK一起用，降低功耗, 网络不稳定，一般选择打开
   * - audioRxAgc
     - bool
     - 接收端声音自动增益控制, 接收端声音过大过小时，可尝试打开
   * - audioQosAec
     - bool
     - 回声消除
   * - audioQosAnr
     - bool
     - 
       - 发送端噪声抑制, 接收端声音噪声大，提高抑制等级；
       - 接收端声音小，可在不影响声音质量情况下降低抑制等级或者关闭抑制
   * - audioQosAgc
     - bool
     - 发送端声音自动增益控制
   * - audioQosVad
     - bool
     - 静音检测


**视频参数**

.. list-table::
   :header-rows: 1

   * - videoEnableCodecs
     - NSString
     - 视频编解码，协商优先级按顺序排列
   * - videoResolutionRecvWidth
     - int
     - 视频接收宽
   * - videoResolutionRecvHeight
     - int
     - 视频接收高
   * - videoResolutionSendWidth
     - int
     - 视频发送宽
   * - videoResolutionSendHeight
     - int
     - 视频发送高
   * - videoBitrate
     - int
     - 视频初始码率 kb
   * - videoSendFramerate
     - int
     - 发送帧率
   * - videoArsEnable
     - bool
     - 是否开启视频 ars
   * - videoArsBitrateMin
     - int
     - 视频最小码率，kb
   * - videoArsBitrateMax
     - int
     - 视频最大码率，kb
   * - videoArsFramerateMin
     - int
     - 视频最小帧率，kb
   * - videoArsFramerateMax
     - int
     - 视频最大帧率，kb
   * - videoRedFec
     - bool
     - 
       - 支持rfc 2198 语音fec-red，增强抗丢包能力，会增加一倍的payload码率，不会增加包头。
       - 比如Opus 55kbps增加一倍码率后，最终码率达到90kbps=55+35；
       - Opus 10kbps增加一倍码率后，最终码率达到16kbps=10+6。
   * - videoRecvFullScreen
     - bool
     - 影响本端视频请求分辨率，默认设置为true
       
       假设条件
         - 1. 本端默认请求是640*360的分辨率，通过 Mtc_CallDbSetAnVideoRecvResolution 设置
         - 2. 本端屏幕分辨率为360*360
         - true：请求分辨率则会被调整为360*360
         - false: 请求还是以640*360进行请求
   * - videoSmallNalu
     - bool
     - 视频数据以SmallNalu方式打包, 一个包打包的数据多，减少包头的码率，从而降低功耗
   * - videoResolutionControl
     - bool
     - 分辨率控制, 开启则分辨率随网络波动而变化, 关闭则固定分辨率


开发者可根据具体的需求自行配置。

**常用的场景配置**

手机拨打手机

手机拨打智能硬件

智能硬件拨打智能硬件










