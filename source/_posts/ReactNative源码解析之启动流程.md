---
title: ReactNative源码解析之启动流程
date: 2019-11-07 18:27:02
tags: 'ReactNative'
---
接触RN开发也快两年的时间了，期间也开发了5、6个APP了，ReactNative的版本也在快速的迭代着，今天重新出发，从源码解析一下App的启动流程，此次解析基于RN 0.60.5版本。
### 开始之前
开始分析之前，新建一个名为RnDemo的空项目,RN版本选择0.60.5，通过查看项目的目录结构中Android部分会自动为我们生成MainActivity.java和MainApplication.java文件，我们的分析就从这两个文件入手。
### Java部分，开始上传
 1.首先看一下MainApplication文件，继承Application并实现了ReactApplication接口，主要做一写RN的初始化操作。
```
public class MainApplication extends Application implements ReactApplication {
  // 实现ReactApplication接口，创建ReactNativeHost成员变量，持有ReactInstanceManager实例，做一些初始化操作。
  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
  // 是否开启dev调试,及一些调试工具，比如redbox(红盒)，有时我们看到的报错
    @Override
    public boolean getUseDeveloperSupport() {
      return BuildConfig.DEBUG;
    }
    // 返回app需要的ReactPackage，添加需要加载的模块，这个地方就是我们在项目中添加依赖包时需要添加第三方package的地方
    @Override
    protected List<ReactPackage> getPackages() {
      @SuppressWarnings("UnnecessaryLocalVariable")
      List<ReactPackage> packages = new PackageList(this).getPackages();
      // Packages that cannot be autolinked yet can be added manually here, for example:
      // packages.add(new MyReactNativePackage());
      return packages;
    }

    @Override
    protected String getJSMainModuleName() {
      return "index";
    }
  };

  @Override
  public ReactNativeHost getReactNativeHost() {
    return mReactNativeHost;
  }

  @Override
  public void onCreate() {
    super.onCreate();
   //SoLoader：加载C++底层库，准备解析JS。
    SoLoader.init(this, /* native exopackage */ false);
  }
}
```
2.接下来看一下MainActivity文件,继承自ReactActivity,ReactActivity作为JS页面的真正容器
```
public class MainActivity extends ReactActivity {

    /**
     * Returns the name of the main component registered from JavaScript.
     * This is used to schedule rendering of the component.
     */
    @Override
    protected String getMainComponentName() {
       // 返回组件名，和js入口注册名字一致
        return "RnDemo";
    }
}
对应的js模块注册名字中：
AppRegistry.registerComponent("RnDemo", () => App);
```
3.继续，来看一下ReactActivity来，
```
public abstract class ReactActivity extends AppCompatActivity
    implements DefaultHardwareBackBtnHandler, PermissionAwareActivity {

  private final ReactActivityDelegate mDelegate;

  protected ReactActivity() {
    mDelegate = createReactActivityDelegate();
  }
  /**
   * Called at construction time, override if you have a custom delegate implementation.
   */
  protected ReactActivityDelegate createReactActivityDelegate() {
    return new ReactActivityDelegate(this, getMainComponentName());
  }
  ...
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mDelegate.onCreate(savedInstanceState);
  }
  protected final ReactNativeHost getReactNativeHost() {
    return mDelegate.getReactNativeHost();
  }

  protected final ReactInstanceManager getReactInstanceManager() {
    return mDelegate.getReactInstanceManager();
  }

  protected final void loadApp(String appKey) {
    mDelegate.loadApp(appKey);
  }
}
```
从以上代码可以看到，真正实现是在ReactActivityDelegate类中进行的。
4.继续，我们重点看一下ReactActivityDelegate中的内容
```
public class ReactActivityDelegate {
  protected void onCreate(Bundle savedInstanceState) {
//mMainComponentName就是上面ReactActivity.getMainComponentName()返回的组件名
    String mainComponentName = getMainComponentName();
    if (mainComponentName != null) {
// 加载app页面
      loadApp(mainComponentName);
    }
// 双击判断工具类
    mDoubleTapReloadRecognizer = new DoubleTapReloadRecognizer();
  }

  protected void loadApp(String appKey) {
// 非空判断
    if (mReactRootView != null) {
      throw new IllegalStateException("Cannot loadApp while app is already running.");
    }
//创建ReactRootView作为根视图,它本质上是一个FrameLayout
    mReactRootView = createRootView();
// 启动RN应用，并完成一些初始化设置
    mReactRootView.startReactApplication(
      getReactNativeHost().getReactInstanceManager(),
      appKey,
      getLaunchOptions());
// 将ReactRootView作为Activity的显示view
    getPlainActivity().setContentView(mReactRootView);
  }
```
看看ReactActivityDelegate做了那些工作：
```
1.创建ReactRootView作为根视图
2.startReactApplication启动RN流程
3.将ReactRootView作为ReactActivity的内容显示view
```
由此看来ReactRootView是个关键，进入ReactRootView类继续看一下启动RN的startReactApplication方法，它接受三个参数:ReactInstanceManager,appName,启动的设置参数launchOptions,
```
/**
   * Schedule rendering of the react component rendered by the JS application from the given JS
   * module (@{param moduleName}) using provided {@param reactInstanceManager} to attach to the
   * JS context of that manager. Extra parameter {@param launchOptions} can be used to pass initial
   * properties for the react component.
   */
  public void startReactApplication(
      ReactInstanceManager reactInstanceManager,
      String moduleName,
      @Nullable Bundle initialProperties,
      @Nullable String initialUITemplate) {
    Systrace.beginSection(TRACE_TAG_REACT_JAVA_BRIDGE, "startReactApplication");
    try {
      UiThreadUtil.assertOnUiThread();

      // TODO(6788889): Use POJO instead of bundle here, apparently we can't just use WritableMap
      // here as it may be deallocated in native after passing via JNI bridge, but we want to reuse
      // it in the case of re-creating the catalyst instance
      Assertions.assertCondition(
        mReactInstanceManager == null,
        "This root view has already been attached to a catalyst instance manager");
    // reactInstanceManage实例,管理React实例
      mReactInstanceManager = reactInstanceManager;
   // js注册的name,同ReactActivity.getMainComponentName()与AppRegistry.registerComponent()放回一致
      mJSModuleName = moduleName;
   // 是Native向JS传递的数据，以后可能由POJO代替，默认是null，需要的话要重写createReactActivityDelegate ，并重写其中getLaunchOptions方法
      mAppProperties = initialProperties;
      mInitialUITemplate = initialUITemplate;

      if (mUseSurface) {
        // TODO initialize surface here
      }
    // 创建RN的上下文ReactContext
      if (!mReactInstanceManager.hasStartedCreatingInitialContext()) {
        mReactInstanceManager.createReactContextInBackground();
      }
    //宽高计算完成后添加布局监听
      attachToReactInstanceManager();

    } finally {
      Systrace.endSection(TRACE_TAG_REACT_JAVA_BRIDGE);
    }
  }
```
接下来，进入ReactInstanceManger类看一下createReactContextInBackground方法,
```
/**
   * Trigger react context initialization asynchronously in a background async task. This enables
   * applications to pre-load the application JS, and execute global code before
   * {@link ReactRootView} is available and measured. This should only be called the first time the
   * application is set up, which is enforced to keep developers from accidentally creating their
   * application multiple times without realizing it.
   *
   * Called from UI thread.
   */
  @ThreadConfined(UI)
  public void createReactContextInBackground() {
    Log.d(ReactConstants.TAG, "ReactInstanceManager.createReactContextInBackground()");
    Assertions.assertCondition(
        !mHasStartedCreatingInitialContext,
        "createReactContextInBackground should only be called when creating the react " +
            "application for the first time. When reloading JS, e.g. from a new file, explicitly" +
            "use recreateReactContextInBackground");
    // 仅在应用首次启动是调用，防止开发人员意外的创建其他应用
    mHasStartedCreatingInitialContext = true;
    recreateReactContextInBackgroundInner();
  }
```
createReactContextInBackground方法仅会在首次启动时调用，重新加载(reloaded)app时，会调用recreateReactContextInBackground(),两个方法都会调用recreateReactContextInBackgroundInner(),
```
@ThreadConfined(UI)
  private void recreateReactContextInBackgroundInner() {
    Log.d(ReactConstants.TAG, "ReactInstanceManager.recreateReactContextInBackgroundInner()");
    PrinterHolder.getPrinter()
        .logMessage(ReactDebugOverlayTags.RN_CORE, "RNCore: recreateReactContextInBackground");
  //UI线程
    UiThreadUtil.assertOnUiThread();
//开发模式，实现在线更新Bundle，晃动弹出调试菜单等功能，这一部分属于调试功能流程。
    if (mUseDeveloperSupport && mJSMainModulePath != null) {
      final DeveloperSettings devSettings = mDevSupportManager.getDevSettings();

      // If remote JS debugging is enabled, load from dev server.
      if (mDevSupportManager.hasUpToDateJSBundleInCache() &&
          !devSettings.isRemoteJSDebugEnabled()) {
        // If there is a up-to-date bundle downloaded from server,
        // with remote JS debugging disabled, always use that.
// 调试模式，从服务器加载jsBundle
        onJSBundleLoadedFromServer(null);
        return;
      }

      if (!Systrace.isTracing(TRACE_TAG_REACT_APPS | TRACE_TAG_REACT_JS_VM_CALLS)) {
// 加载服务bundle
        if (mBundleLoader == null) {
          mDevSupportManager.handleReloadJS();
        } else {
          mDevSupportManager.isPackagerRunning(
              new PackagerStatusCallback() {
                @Override
                public void onPackagerStatusFetched(final boolean packagerIsRunning) {
                  UiThreadUtil.runOnUiThread(
                      new Runnable() {
                        @Override
                        public void run() {
                          if (packagerIsRunning) {
                            mDevSupportManager.handleReloadJS();
                          } else {
                            // If dev server is down, disable the remote JS debugging.
                            devSettings.setRemoteJSDebugEnabled(false);
                            recreateReactContextInBackgroundFromBundleLoader();
                          }
                        }
                      });
                }
              });
        }
        return;
      }
    }
    // 加载本地bundle
    recreateReactContextInBackgroundFromBundleLoader();
  }
```
recreateReactContextInBackgroundFromBundleLoader方法向下调用recreateReactContextInBackground方法
```
@ThreadConfined(UI)
  private void recreateReactContextInBackground(
//C++和JS双向通信的中转站
    JavaScriptExecutorFactory jsExecutorFactory,
// bundle加载器，根据ReactNativeHost中的配置决定从哪里加载bundle文件
    JSBundleLoader jsBundleLoader) {
    Log.d(ReactConstants.TAG, "ReactInstanceManager.recreateReactContextInBackground()");
    UiThreadUtil.assertOnUiThread();
    //创建ReactContextInitParams对象
    final ReactContextInitParams initParams = new ReactContextInitParams(
      jsExecutorFactory,
      jsBundleLoader);
    if (mCreateReactContextThread == null) {
  // 在newThread实例化ReactContext
      runCreateReactContextOnNewThread(initParams);
    } else {
      mPendingReactContextInitParams = initParams;
    }
  }

 //runCreateReactContextOnNewThread()方法中内容
final ReactApplicationContext reactApplicationContext =
                      createReactContext(
                          initParams.getJsExecutorFactory().create(),
                          initParams.getJsBundleLoader());
```
在runCreateReactContextOnNewThread方法中，我们看到是ReactInstanceManager.createReactContext方法最终创建了ReactApplicationContext,我们继续看createReactContext()方法,有关此方法的2个参数：
> JSCJavaScriptExecutor jsExecutor：JSCJavaScriptExecutor继承于JavaScriptExecutor，当该类被加载时，它会自动去加载"reactnativejnifb.so"库，并会调用Native方
法initHybrid()初始化C++层RN与JSC通信的框架。
JSBundleLoader jsBundleLoader：缓存了JSBundle的信息，封装了上层加载JSBundle的相关接口，CatalystInstance通过其简介调用ReactBridge去加载JS文件，不同的场景会创建
不同的加载器，具体可以查看类JSBundleLoader。
```
private ReactApplicationContext createReactContext(
      JavaScriptExecutor jsExecutor,
      JSBundleLoader jsBundleLoader) {
    Log.d(ReactConstants.TAG, "ReactInstanceManager.createReactContext()");
    ReactMarker.logMarker(CREATE_REACT_CONTEXT_START, jsExecutor.getName());
// ReactApplicationContext 是reactContext的包装类
    final ReactApplicationContext reactContext = new ReactApplicationContext(mApplicationContext);

    NativeModuleCallExceptionHandler exceptionHandler = mNativeModuleCallExceptionHandler != null
        ? mNativeModuleCallExceptionHandler
        : mDevSupportManager;
    reactContext.setNativeModuleCallExceptionHandler(exceptionHandler);
//创建JavaModule注册表Builder，用来创建JavaModule注册表，JavaModule注册表将所有的JavaModule注册到CatalystInstance中。
    NativeModuleRegistry nativeModuleRegistry = processPackages(reactContext, mPackages, false);
//jsExecutor、nativeModuleRegistry、nativeModuleRegistry等各种参数处理好之后，开始构建CatalystInstanceImpl实例。
    CatalystInstanceImpl.Builder catalystInstanceBuilder = new CatalystInstanceImpl.Builder()
      .setReactQueueConfigurationSpec(ReactQueueConfigurationSpec.createDefault())
      .setJSExecutor(jsExecutor)// js执行通信类
      .setRegistry(nativeModuleRegistry)//java模块注册表
      .setJSBundleLoader(jsBundleLoader)// bundle加载器
      .setNativeModuleCallExceptionHandler(exceptionHandler); // 异常处理器

    ReactMarker.logMarker(CREATE_CATALYST_INSTANCE_START);
    // CREATE_CATALYST_INSTANCE_END is in JSCExecutor.cpp
    Systrace.beginSection(TRACE_TAG_REACT_JAVA_BRIDGE, "createCatalystInstance");
    final CatalystInstance catalystInstance;
    try {
      catalystInstance = catalystInstanceBuilder.build();
    } finally {
      Systrace.endSection(TRACE_TAG_REACT_JAVA_BRIDGE);
      ReactMarker.logMarker(CREATE_CATALYST_INSTANCE_END);
    }
    if (mJSIModulePackage != null) {
      catalystInstance.addJSIModules(mJSIModulePackage
        .getJSIModules(reactContext, catalystInstance.getJavaScriptContextHolder()));
    }

    if (mBridgeIdleDebugListener != null) {
      catalystInstance.addBridgeIdleDebugListener(mBridgeIdleDebugListener);
    }
    if (Systrace.isTracing(TRACE_TAG_REACT_APPS | TRACE_TAG_REACT_JS_VM_CALLS)) {
//调用CatalystInstanceImpl的Native方法把Java Registry转换为Json，再由C++层传送到JS层。
      catalystInstance.setGlobalVariable("__RCTProfileIsProfiling", "true");
    }
    ReactMarker.logMarker(ReactMarkerConstants.PRE_RUN_JS_BUNDLE_START);
    Systrace.beginSection(TRACE_TAG_REACT_JAVA_BRIDGE, "runJSBundle");
//通过CatalystInstance开始加载JS Bundle
    catalystInstance.runJSBundle();
    Systrace.endSection(TRACE_TAG_REACT_JAVA_BRIDGE);
  //关联ReacContext与CatalystInstance
    reactContext.initializeWithInstance(catalystInstance);


    return reactContext;
  }
```
createReactContext方法中用catalystInstance.runJSBundle() 来加载 JS bundle
```
@Override
  public void runJSBundle() {
   ...省略代码
    mJSBundleLoader.loadScript(CatalystInstanceImpl.this);
}
```
查看loadScript方法，参数JSBundleLoaderDelegate接口的实现类CatalystInstanceImpl,我们假设调用了loadScriptFromAssets方法，
```
@Override
  public void loadScriptFromAssets(AssetManager assetManager, String assetURL, boolean loadSynchronously) {
    mSourceURL = assetURL;
    jniLoadScriptFromAssets(assetManager, assetURL, loadSynchronously);
  }
private native void jniLoadScriptFromAssets(AssetManager assetManager, String assetURL, boolean loadSynchronously);
```
CatalystInstanceImpl.java最终还是调用C++层的CatalystInstanceImpl.cpp去加载JS Bundle。
接下来看一下CatalystInstance的实现类CatalystInstanceImpl的构造方法：
```
private CatalystInstanceImpl(
      final ReactQueueConfigurationSpec reactQueueConfigurationSpec,
      final JavaScriptExecutor jsExecutor,
      final NativeModuleRegistry nativeModuleRegistry,
      final JSBundleLoader jsBundleLoader,
      NativeModuleCallExceptionHandler nativeModuleCallExceptionHandler) {
    Log.d(ReactConstants.TAG, "Initializing React Xplat Bridge.");
    Systrace.beginSection(TRACE_TAG_REACT_JAVA_BRIDGE, "createCatalystInstanceImpl");
  //Native方法，用来创建JNI相关状态，并返回mHybridData
    mHybridData = initHybrid();
 //RN中的三个线程：Native Modules Thread、JS Thread、UI Thread，都是通过Handler来管理的。
    mReactQueueConfiguration = ReactQueueConfigurationImpl.create(
        reactQueueConfigurationSpec,
        new NativeExceptionHandler());
    mBridgeIdleListeners = new CopyOnWriteArrayList<>();
    mNativeModuleRegistry = nativeModuleRegistry;
    mJSModuleRegistry = new JavaScriptModuleRegistry();
    mJSBundleLoader = jsBundleLoader;
    mNativeModuleCallExceptionHandler = nativeModuleCallExceptionHandler;
    mNativeModulesQueueThread = mReactQueueConfiguration.getNativeModulesQueueThread();
    mTraceListener = new JSProfilerTraceListener(this);
    Systrace.endSection(TRACE_TAG_REACT_JAVA_BRIDGE);

    Log.d(ReactConstants.TAG, "Initializing React Xplat Bridge before initializeBridge");
    Systrace.beginSection(TRACE_TAG_REACT_JAVA_BRIDGE, "initializeCxxBridge");
//Native方法，调用initializeBridge()方法，并创建BridgeCallback实例，初始化Bridge。
    initializeBridge(
      new BridgeCallback(this),
      jsExecutor,
      mReactQueueConfiguration.getJSQueueThread(),
      mNativeModulesQueueThread,
      mNativeModuleRegistry.getJavaModules(this),
      mNativeModuleRegistry.getCxxModules());
    Log.d(ReactConstants.TAG, "Initializing React Xplat Bridge after initializeBridge");
    Systrace.endSection(TRACE_TAG_REACT_JAVA_BRIDGE);

    mJavaScriptContextHolder = new JavaScriptContextHolder(getJavaScriptContext());
  }

//在C++层初始化通信桥ReactBridge
 private native void initializeBridge(
      ReactCallback callback,
      JavaScriptExecutor jsExecutor,
      MessageQueueThread jsQueue,
      MessageQueueThread moduleQueue,
      Collection<JavaModuleWrapper> javaModules,
      Collection<ModuleHolder> cxxModules);
```
参数解读：
- ReactCallback:CatalystInstanceImpl的静态内部类ReactCallback，负责接口回调
-  JavaScriptExecutor: js执行器，将js的调用传给c++层
- MessageQueueThread jsQueue：js线程
- MessageQueueThread moduleQueue： java线程
- javaModules： java module
- cxxModules: c++ module
好累，😀，继续，我们去c++层看一下，在项目的node_modules/react-native/ReactAndroid/src/main/jni/react/jni可以找到CatalystInstanceImpl.cpp
看一下CatalystInstanceImpl::jniLoadScriptFromAssets
```
void CatalystInstanceImpl::jniLoadScriptFromAssets(
    jni::alias_ref<JAssetManager::javaobject> assetManager,
    const std::string& assetURL,
    bool loadSynchronously) {
  const int kAssetsLength = 9;  // strlen("assets://");
//获取source js Bundle的路径名，这里默认的就是index.android.bundle
  auto sourceURL = assetURL.substr(kAssetsLength);
//assetManager是Java层传递过来的AssetManager，调用JSLoade.cpo里的extractAssetManager()方法，extractAssetManager()再
  //调用android/asset_manager_jni.h里的AssetManager_fromJava()方法获取AssetManager对象。
  auto manager = extractAssetManager(assetManager);
// 调用JSloader.cpp的loadScriptFromAssets方法，读取js Bundle里面的内容
  auto script = loadScriptFromAssets(manager, sourceURL);
// unbundle命令打包判断，build.gradle默认里是bundle打包方式。
  if (JniJSModulesUnbundle::isUnbundle(manager, sourceURL)) {
    auto bundle = JniJSModulesUnbundle::fromEntryFile(manager, sourceURL);
    auto registry = RAMBundleRegistry::singleBundleRegistry(std::move(bundle));
    instance_->loadRAMBundle(
      std::move(registry),
      std::move(script),
      sourceURL,
      loadSynchronously);
    return;
  } else if (Instance::isIndexedRAMBundle(&script)) {
    instance_->loadRAMBundleFromString(std::move(script), sourceURL);
  } else {
//bundle命令打包走此流程，instance_是Instan.h中类的实例
    instance_->loadScriptFromString(std::move(script), sourceURL, loadSynchronously);
  }
}
```
在项目node_modules/react-native/ReactCommon的cxxReact的NativeToJsBridge.cpp文件：
```
void NativeToJsBridge::loadApplication(
    std::unique_ptr<JSModulesUnbundle> unbundle,
    std::unique_ptr<const JSBigString> startupScript,
    std::string startupScriptSourceURL) {

  //获取一个MessageQueueThread，探后在线程中执行一个Task。
  runOnExecutorQueue(
      m_mainExecutorToken,
      [unbundleWrap=folly::makeMoveWrapper(std::move(unbundle)),
       startupScript=folly::makeMoveWrapper(std::move(startupScript)),
       startupScriptSourceURL=std::move(startupScriptSourceURL)]
        (JSExecutor* executor) mutable {

    auto unbundle = unbundleWrap.move();
    if (unbundle) {
      executor->setJSModulesUnbundle(std::move(unbundle));
    }

    //executor从runOnExecutorQueue()返回的map中取得，与OnLoad中的JSCJavaScriptExecutorHolder对应，也与
    //Java中的JSCJavaScriptExecutor对应。它的实例在JSIExecutor.cpp中实现。
    executor->loadApplicationScript(std::move(*startupScript),
                                    std::move(startupScriptSourceURL));
  });
}
关于unbundle命令

<unbundle命令，使用方式和bundle命令完全相同。unbundle命令是在bundle命令的基础上增加了一项功能，除了生成整合JS文件index.android.bundle外，还会
生成各个单独的未整合JS文件（但会被优化），全部放在js-modules目录下，同时会生成一个名为UNBUNDLE的标识文件，一并放在其中。UNBUNDLE标识文件的前4个字节
固定为0xFB0BD1E5，用于加载前的校验。
```
进入项目node_modules/react-native/ReactCommon/jsiexecutor/jsireact/JSIExecutor.cpp进一步调用JSIExecutor.cpp的loadApplicationScript()方法。
```
//解释执行JS
runtime_->evaluateJavaScript(
      std::make_unique<BigStringBuffer>(std::move(script)), sourceURL);
  flush();
```
flushedQueueJS支线的是MessageQueue.js的flushedQueue()方法，此时JS已经被加载到队列中，等待Java层来驱动它。加载完JS后，返回reactApplicationContext，我们继续跟进它的实现。
我们回到ReactInstanceManager类的runCreateReactContextOnNewThread方法中，看到setupReactContext()方法，进入之后可以看到attachRootViewToInstance(reactRoot)方法，进入后
```
private void attachRootViewToInstance(final ReactRoot reactRoot) {
    Log.d(ReactConstants.TAG, "ReactInstanceManager.attachRootViewToInstance()");
    Systrace.beginSection(TRACE_TAG_REACT_JAVA_BRIDGE, "attachRootViewToInstance");
    UIManager uiManagerModule = UIManagerHelper.getUIManager(mCurrentReactContext, reactRoot.getUIManagerType());

    @Nullable Bundle initialProperties = reactRoot.getAppProperties();
    final int rootTag = uiManagerModule.addRootView(
      reactRoot.getRootViewGroup(),
      initialProperties == null ?
            new WritableNativeMap() : Arguments.fromBundle(initialProperties),
        reactRoot.getInitialUITemplate());
    reactRoot.setRootViewTag(rootTag);
  // 启动入口
    reactRoot.runApplication();
    Systrace.beginAsyncSection(
      TRACE_TAG_REACT_JAVA_BRIDGE,
      "pre_rootView.onAttachedToReactInstance",
      rootTag);
    UiThreadUtil.runOnUiThread(
        new Runnable() {
          @Override
          public void run() {
            Systrace.endAsyncSection(
                TRACE_TAG_REACT_JAVA_BRIDGE, "pre_rootView.onAttachedToReactInstance", rootTag);
            reactRoot.onStage(ReactStage.ON_ATTACH_TO_INSTANCE);
          }
        });
    Systrace.endSection(TRACE_TAG_REACT_JAVA_BRIDGE);
  }

```
catalystInstance.getJSModule(AppRegistry.class)AppRegistry.class是JS层暴露给Java层的接口方法，它的真正实现在AppRegistry.js里，AppRegistry.js是运行所有RN应用的JS层入口，
```
runApplication(appKey: string, appParameters: any): void {
    const msg =
      'Running application "' +
      appKey +
      '" with appParams: ' +
      JSON.stringify(appParameters) +
      '. ' +
      '__DEV__ === ' +
      String(__DEV__) +
      ', development-level warning are ' +
      (__DEV__ ? 'ON' : 'OFF') +
      ', performance optimizations are ' +
      (__DEV__ ? 'OFF' : 'ON');
    infoLog(msg);
    BugReporting.addSource(
      'AppRegistry.runApplication' + runCount++,
      () => msg,
    );
    invariant(
      runnables[appKey] && runnables[appKey].run,
      'Application ' +
        appKey +
        ' has not been registered.\n\n' +
        "Hint: This error often happens when you're running the packager " +
        '(local dev server) from a wrong folder. For example you have ' +
        'multiple apps and the packager is still running for the app you ' +
        'were working on before.\nIf this is the case, simply kill the old ' +
        'packager instance (e.g. close the packager terminal window) ' +
        'and start the packager in the correct app folder (e.g. cd into app ' +
        "folder and run 'npm start').\n\n" +
        'This error can also happen due to a require() error during ' +
        'initialization or failure to call AppRegistry.registerComponent.\n\n',
    );

    SceneTracker.setActiveScene({name: appKey});
    runnables[appKey].run(appParameters);
  },
```
😀，基本到这，就会去调用JS进行组件渲染，再通过Java层的UIManagerModule将JS组件转换为Android组件，最终显示在ReactRootView上，即完成启动过程。😀
阅读源代码还是挺耗时的事情，哈哈。

同步更新至个人公众号及博客。
CSDN:https://blog.csdn.net/wayne214
公众号：君伟说。
![个人微信公众号.jpg](/images/个人微信公众号.jpg)