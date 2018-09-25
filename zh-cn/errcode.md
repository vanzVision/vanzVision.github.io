# 智能结算平台Android SDK ErrorCode
## 1.1 ErrorCode
本文档用于说明智能结算平台SDK Android版本ErrorCode详细说明，持续完善中.....

```Java

public class ErrorCode {
    /**
     * 获取wifi设备地址失败
     */
    public static final int ERROR_GET_MAC = 100001;
    /**
     * 设备未注册
     * */
    public static final int ERROR_DEVICE_NOT_REGISTERED = 100002;

    /**
     * 请求注册中心失败
     * */
    public static final int ERROR_REQUEST_REGISTRY_FAILED = 100003;

    /**
     * 链接服务器失败
     * */
    public static final int ERROR_LINK_SERVER_FAILED = 100011;

    /**
     * 获取视频截图配置为空
     * */
    public static final int ERROR_GET_VIEDO_NULL = 100021;

    /**
     * 获取视频截图配置不正确
     * */
    public static final int ERROR_GET_VIEDO_NOT_CORRECT = 100022;

    /**
     * 上传图片网络异常
     *
     * */
    public static final int ERROR_UPLOAD_IMAGE_FAILED = 100031;
    /**
     * 服务异常
     *
     * */
    public static final int ERROR_UPLOAD_SERVER_FAILED = 100032;
    /**
     * 上传oss异常
     * */
    public static final int ERROR_UPLOAD_OSS_SERVER_FAILED = 100033;
}
```