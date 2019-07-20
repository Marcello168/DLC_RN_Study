##    <u>新建 RN 项目</u>

### DLCRNProject 框架地址 [git@192.168.1.248:DLCRNProject](git@192.168.1.248:DLCRNProject)

1. 先用 react-native init 命令初始化项目 记得要在后后面添加 --version 0.58.6 指定一下版本 目前我们使用的是这个版本

```sh
   react-native init DLCRNProject --version 0.58.6
```

2. 项目初始化完毕之后，在项目的根目录下,找到package.js文件 往里面添加常用的第三方库 常用的第三方库 如下

```js
    "@ant-design/react-native": "^3.1.8",
    "prop-types": "^15.7.2",
    "react-native-actionsheet": "^2.4.2",
    "react-native-camera": "^1.12.0",
"react-native-elements": "^1.1.0",
"react-native-gesture-handler": "^1.1.0",
"react-native-image-crop-picker": "^0.24.1",
"react-native-image-picker": "^0.28.0",
"react-native-keyboard-aware-scroll-view": "^0.8.0",
"react-native-permissions": "^1.1.1",
"react-native-shadow-cards": "^1.0.2",
"react-native-storage": "^1.0.1",
"react-native-vector-icons": "^6.3.0",
"react-navigation": "^3.5.1",
 "rn-countdown": "^1.0.0",
 "react-native-webview": "^5.12.0"



```
---

 添加前
![a0f3687d4fc0b0d5b740398e907413b8.png](https://ae01.alicdn.com/kf/HTB1YdM9aG61gK0jSZFl760DKFXaN.png)

---

 添加后
![0d1ed417dc71cabbe4e1b8a888fe1b57.png](https://ae01.alicdn.com/kf/HTB1Jrg8aRv0gK0jSZKb762K2FXa3.png)

1.  cd 到项目根目录下 执行 如下  install 命令 拉取第三方库

```sh

npm install 
# ---or ---
yarn install

```

4. 为 iOS 项目 添加 cocoapods 管理  cd 到 项目根目录的ios文件夹下 创建 Podfile 文件 文件内容如下 要记得修改<u>**项目的工程名字**</u>
```sh
platform :ios, '9.0'

# The target name is most likely the name of your project.
target 'RNCloudApp' do
 # Your 'node_modules' directory is probably in the root of your project,
  # but if not, adjust the `:path` accordingly
  pod 'React', :path => '../node_modules/react-native', :subspecs => [
    'Core',
    'CxxBridge', # Include this for RN >= 0.47
    'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', # needed for debugging
    'RCTLinkingIOS',
    'RCTImage' # <- Add this line
    # Add any other subspecs you want to use in your project
  ]
  # Explicitly include Yoga if you are using RN >= 0.42.0
  pod 'yoga', :path => '../node_modules/react-native/ReactCommon/yoga'

  # Third party deps podspec link
  pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
  pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
  pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'
  end
  
  post_install do |installer|
  installer.pods_project.targets.each do |target|
    if target.name == "React"
      target.remove_from_project
    end
  end
end

```


5. iOS 项目 添加 cocoapods 管理后 执行 link 命令 把一些需要用到原生的库，链接到 原生的工程上 也可以链接指定的库 


```sh
react-native link 
# ---or ---
react-native link Your-Project
```
6. 链接完第三方库之后 还需要链接[蚂蚁金服](https://mobile.ant.design/index-cn)的资源文件 使用如下命令

```sh
react-native link @ant-design/icons-react-native
```

7. 上面的工作都完成之后  cd 到 ios 文件夹下 执行 pod install 命令 初始化 iOS 项目  目前为止 我们为一个新的RN 工程添加了一些第三方库 下面 我们再把我们项目中常用的 文件 拖进去项目里面 在DLCRNProject 工程下的 App 文件夹 都可以直接拖到新的项目里面去  再把app.js的文件直接copy 过去 就可运行起来哦

### 为Android 增加 签名文件 

1. 将 my-release-key.keystore 文件放在 android/app 的目录下
2. 在 app目录下的 build.gradle 文件  增加如下代码 

```groovy
    signingConfigs {
        release {
            storeFile file(MYAPP_RELEASE_STORE_FILE)
            storePassword MYAPP_RELEASE_STORE_PASSWORD
            keyAlias MYAPP_RELEASE_KEY_ALIAS
            keyPassword MYAPP_RELEASE_KEY_PASSWORD
        }
    }
    buildTypes {
        release {
            minifyEnabled enableProguardInReleaseBuilds
            proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"
            signingConfig signingConfigs.release
        }
        debug {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }

    }
    
```

### 为Android 适配Android 9

适配Android 9 [https://www.jianshu.com/p/fc86e2b7d3ac](https://www.jianshu.com/p/fc86e2b7d3ac)

1. 在 res 下新增一个 xml 目录
2. 然后创建一个名为：network_security_config.xml 文件（也可以自定义文件名）文件内容如下
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" />
        </trust-anchors>
    </base-config>
</network-security-config>
```
文件目录如下图 在app/src/main/res 目录下新建一个xml文件夹 <u>**这里请注意是文件夹 而不是文件**</u> 然后再在xml 文件夹下新建 network_security_config.xml 文件
![a9ca7555540d853dd682d663e264b9d0.png](https://ae01.alicdn.com/kf/HTB1LcE8aRv0gK0jSZKb762K2FXaI.png)

3.然后在APP的AndroidManifest.xml文件下的application标签增加以下属性

```xml
<application
...
 android:networkSecurityConfig="@xml/network_security_config"
...
/>
```
如下图
![5b5a9ccb08a8c50978946c1d0f03bbbc.png](https://ae01.alicdn.com/kf/HTB1ZBE6aUY1gK0jSZFM761WcVXad.png)







### Android 容易出现的错误

* Failed to resolve: com.github.yalantis:ucrop:2.2.2-native

解决方法 在主Project 加上


```js
maven { url “https://jitpack.io” }
```

![e9afc77d59b9925f1324b056bf79a4bd.png](https://ae01.alicdn.com/kf/HTB1ycI7aKT2gK0jSZFv760nFXXa6.png)



