# SDK 接入
## 智能结算台sdk包
### 1、简介
本文档用于说明凡知开放平台SDK Android版本接口之间的关系以及接口调用顺序，对开放平台SDK Android版本各接口都有详细的说明。

### 2、功能介绍
vanz: 初始化

### 3、工程配置
环境准备
支持 Android 平板系统 5.1版本

### 4、名词介绍
样本组:由各种标签商品组成一个样本组，所有需要识别的商品在同一样本组中进行识别
资源服务器:为了减轻服务器压力，可以通过资源服务器来分配解析任务

### 5、安装SDK
SDK的安装方式

### 6、使用gradle获得
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
      compile('com.vanz:vanzSdk:1.0.15')
}
最小sdk版本为19
minSdkVersion 19
```

### 7、SDK初始化
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
        //vanzCallback 回调消息类 见8-1
        createHandler();

        Vanz vanz = Vanz.getInstance();
        vanz.init(this);
    }
    public void onResume() {
        super.onResume();
        view.OpenvcInit();
    }
```

### 8、初始化回调
- 8-1 视频注册完毕后监听回调
```Java
    private VanzInterface.VanzCallback vanzCallback = new VanzInterface.VanzCallback() {
        @Override
        public void onViewReady(String deviceName) {    
            //摄像头链接准备成功
        }

        @Override
        public void onViewPasue(String deviceName) {
            //切换界面时视频播放暂停
        }

        @Override
        public void onViewStart(String deviceName) {
            //切换到主界面，视频开始播放
        }

        @Override
        public void onGetConfig() {
            //获取视频截取信息配置
        }

        @Override
        public void onViewError(int err) {
            
        }
    };
```

- 8-2 资源服务器 样本组选择
```Java
 private void createHandler() {
        Looper mainLooper = Looper.getMainLooper();
        mainHandler = new Handler(mainLooper) {
            @Override
            public void handleMessage(Message msg) {
                super.handleMessage(msg);
                switch (msg.what) {
                    case 0:
                        vanz.getGruopData(new OnVanzCallBack.OnSpinner() {
                            @Override
                            public void onSuccess(final List<String> list, final int num) {
                                //样本组
                                runOnUiThread(new Runnable() {
                                    @Override
                                    public void run() {
                                        Spinner spinner = (Spinner) findViewById(R.id.spinner);
                                        loadSpinner(spinner, list, num);
                                        mainHandler.sendEmptyMessage(1);
                                    }
                                });
                            }

                            @Override
                            public void onError(int msg) {

                            }
                        });
                        break;
                    case 1:
                        vanz.getRescSerData(new OnVanzCallBack.OnSpinner() {
                            @Override
                            public void onSuccess(final List<String> list, final int first) {
                                //资源服务器
                                runOnUiThread(new Runnable() {
                                    @Override
                                    public void run() {
                                        Spinner spinner = (Spinner) findViewById(R.id.spinnerS);
                                        loadSpinner(spinner, list, first);

                                    }
                                });
                            }

                            @Override
                            public void onError(int msg) {

                            }
                        });
                        break;
                    default:
                        break;
                }
            }
        };
    }
        //样本组、资源服务器信息选择获取
        vanz.setOnBasicLib(new OnVanzCallBack.OnBasicLib() {
            @Override
            public void onRegistered() {
                mainHandler.sendEmptyMessage(0);
            }

            @Override
            public void onRegistErr(int err) {
                Log.i("===>", "onRegistErr: " + err);
            }
        });
```
- 8-3 识别初始化监听回调

```Java

        vanz.setOnPortDevice(new OnVanzCallBack.OnPortDevice() {
            @Override
            public void Initialize() {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        desc.setText("初始化成功");
                    }
                });
            }

            @Override
            public void onWeighing(int state) {
                Log.i("===>", "onWeighing: "+state);
                //1 开始变化 2 稳定有东西 3 稳定没东西
                if (state == 2) {
                    view.TaskIdentify(new VanzInterface.Analyer() {
                        @Override
                        public void onSuccess(JSONObject jsonObject, Bitmap bitmap) {
                            Log.i("===>", "onSuccess: " + jsonObject);
                        }

                        @Override
                        public void onFaileMsg(String msg) {
                            Log.i("===>", "onFaileMsg: "+msg);
                        }

                        @Override
                        public void onError(int msg) {

                        }
                    });
                }
            }
        });
```

