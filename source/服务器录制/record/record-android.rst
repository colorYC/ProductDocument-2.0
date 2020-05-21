Android
=========================

.. _音视频录制(android):

.. highlight:: java

**集成服务器音视频录制功能前，请确保您已经集成了基础的音视频通话功能。**

.. note:: SDK 不支持模拟器运行，请使用真机。

服务器音频视频录制将录制的文件保存在七牛云上，因此，如果需要进行服务器音视频录制，需要在加入频道之前设置录制参数，然后在加入频道的时候传入录制参数。

设置录制参数
----------------------

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
    joinParam.record.recordString = JCMediaChannel.RecordParam.buildQiniuRecordParam(true, bucketName, secretKey, accessKey, fileName);

.. note:: 

       如果进行音频录制，需要将构造方法中的 video 值设为 false。


获取录制状态
----------------------

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
----------------------

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
