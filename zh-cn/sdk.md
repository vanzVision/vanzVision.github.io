# SDK 接入
## 智能结算台sdk包
### 1、简介
本文档用于说明凡知开放平台SDK Android版本接口之间的关系以及接口调用顺序，对开放平台SDK Android版本各接口都有详细的说明。

### 2、功能介绍
vanz: 初始化

### 3、工程配置
环境准备
支持 Android 平板系统 5.1版本

### 4、安装SDK
SDK的安装方式

### 5、使用gradle获得
工程build.gradle
```Java
allprojects {
    repositories {
        ...
        maven { url "https://github.com/vanzVision/vanzSdk/raw/master" }
        maven { url 'http://raw.github.com/saki4510t/libcommon/master/repository/' }
        maven { url 'https://jitpack.io' }
    }
}
```
app build.gradle
```Java
dependencies{
      compile('com.vanz:vanzSdk:1.0.13')
}
最小sdk版本为19
minSdkVersion 19
```

### 6、SDK初始化
- 显示的activity xml中加入视频加载控件，高宽度暂时支持640*480
```Java
    <!--添加视频播放FrameLayout-->
    <com.vanz.librarysdk.Modules.CamerView
        android:id="@+id/camera"
        android:background="@color/BLACK"
        android:layout_width="640px"
        android:layout_height="480px">
    </com.vanz.librarysdk.Modules.CamerView>
```
- 在onCreated之后进行VanzInterface初始化，以及vanz单例初始化

```Java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        VanzInterface view = (VanzInterface) findViewById(R.id.camera);
        view.setCallback(vanzCallback);

        Vanz vanz = Vanz.getInstance();
        vanz.init(this);
    }
```
- 监听回调
