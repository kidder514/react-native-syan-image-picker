
# fork from react-native-syan-image-picker
### this is a fork, and is not maintained by me, please contact the original author if you need more information.


## 功能介绍

 多图片选择器

 Android 基于 [PictureSelector 2.2.0](https://github.com/LuckSiege/PictureSelector)

 iOS 基于 [TZImagePickerController 2.0.0.4](https://github.com/banchichen/TZImagePickerController)

![](http://oy5rz3rfs.bkt.clouddn.com/github/syan_001.png?imageView/2/w/268)
![](http://oy5rz3rfs.bkt.clouddn.com/github/syan_002.png?imageView/2/w/268)
![](http://oy5rz3rfs.bkt.clouddn.com/github/syan_003.png?imageView/2/w/268)

## 安装使用

`$ npm install react-native-syan-image-picker --save`

`$ react-native link react-native-syan-image-picker`

### iOS

- TARGETS -> Build Phases -> Copy Bundle Resources
点击"+"按钮，在弹出的窗口中点击“Add Other”按钮，选择
    ```
    node_modules/react-native-syan-image-picker/ios/TZImagePickerController/TZImagePickerController.bundle
    ```

- 项目目录->Info.plist->增加3项
    ```
    "Privacy - Camera Usage Description
    "Privacy - Location When In Use Usage Description"
    "Privacy - Photo Library Usage Description"
    ```
- 记得添加描述
    ```
    Privacy - Camera Usage Description 是否允许此App使用你的相机？
    Privacy - Photo Library Usage Description 是否允许此App访问你的媒体资料库？
    Privacy - Location When In Use Usage Description 我们需要通过您的地理位置信息获取您周边的相关数据
    ```
    
- 添加中文 PROJECT -> Info -> Localizations 点击"+"按钮，选择Chinese(Simplified)

### Android

- android 下build.gradle文件添加  maven { url "https://jitpack.io" }
    ```
     allprojects {
         repositories {
             mavenLocal()
             jcenter()
             maven { url "https://jitpack.io" }
             maven {
                 // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
                 url "$rootDir/../node_modules/react-native/android"
             }
         }
     }
    ```
 - 添加权限
 ```
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.CAMERA" />
  ```
  
 - 更新到PictureSelector 2.2.0
 ```
    在安卓文件app下修改build.gradle
    compileSdkVersion 26
    buildToolsVersion "26.0.3"
 ```
 
- 注意 安装运行报错
1. 检查自动link是否成功 
2. 使用Android Studio 查看MainApplication文件是否添加new RNSyanImagePickerPackage()
3. 使用Android Studio 打开项目检查Gradle是否同步完成
4. 可以运行[ImagePickerExample](https://github.com/syanbo/ImagePickerExample) 该demo，测试Android7.0，6.0拍照选图都为正常

## link失败手动添加
#### iOS

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-syan-image-picker` and add `RNSyanImagePicker.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libRNSyanImagePicker.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Run your project (`Cmd+R`)<



#### Android

1. Open up `android/app/src/main/java/[...]/MainApplication.java`
  - Add `import com.reactlibrary.RNSyanImagePickerPackage;` to the imports at the top of the file
  - Add `new RNSyanImagePickerPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-syan-image-picker'
  	project(':react-native-syan-image-picker').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-syan-image-picker/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      compile project(':react-native-syan-image-picker')
  	```

## 运行示例

[ImagePickerExample](https://github.com/syanbo/ImagePickerExample)

```
import SYImagePicker from 'react-native-syan-image-picker'

  /**
   * 默认参数
   */
  const options = {
      imageCount: 6,             // 最大选择图片数目，默认6
      isCamera: true,            // 是否允许用户在内部拍照，默认true
      isCrop: false,             // 是否允许裁剪，默认false
      CropW: ~~(width * 0.6),    // 裁剪宽度，默认屏幕宽度60%
      CropH: ~~(width * 0.6),    // 裁剪高度，默认屏幕宽度60%
      isGif: false,              // 是否允许选择GIF，默认false，暂无回调GIF数据
      showCropCircle: false,     // 是否显示圆形裁剪区域，默认false
      circleCropRadius: width/2  // 圆形裁剪半径，默认屏幕宽度一半
      showCropFrame: true,       // 是否显示裁剪区域，默认true
      showCropGrid: false,       // 是否隐藏裁剪区域网格，默认false
      quality: 90                // 压缩质量
  };

  /**
     * 以Callback形式调用
     * 1、相册参数暂时只支持默认参数中罗列的属性；
     * 2、回调形式：showImagePicker(options, (err, selectedPhotos) => {})
     *  1）选择图片成功，err为null，selectedPhotos为选中的图片数组
     *  2）取消时，err返回"取消"，selectedPhotos将为undefined
     *  按需判断各参数值，确保调用正常，示例使用方式：
     *      SYImagePicker.showImagePicker(options, (err, selectedPhotos) => {
     *          if (err) {
     *              // 取消选择
     *              return;
     *          }
     *          // 选择成功
     *      })
     *
     * @param {Object} options 相册参数
     * @param {Function} callback 成功，或失败回调
    */

     /**
      * 以Promise形式调用
      * 1、相册参数暂时只支持默认参数中罗列的属性；
      * 2、使用方式
      *  1）async/await
      *  handleSelectPhoto = async () => {
      *      try {
      *          const photos = await SYImagePicker.asyncShowImagePicker(options);
      *          // 选择成功
      *      } catch (err) {
      *          // 取消选择，err.message为"取消"
      *      }
      *  }
      *  2）promise.then形式
      *  handleSelectPhoto = () => {
      *      SYImagePicker.asyncShowImagePicker(options)
      *      .then(photos => {
      *          // 选择成功
      *      })
      *      .catch(err => {
      *          // 取消选择，err.message为"取消"
      *      })
      *  }
      * @param {Object} options 相册参数
      * @return {Promise} 返回一个Promise对象
     */
     
    /**
     * 打开相机
     * @param options
     * @param callback
     */
     *      SYImagePicker.openCamera(options, (err, selectedPhotos) => {
     *          if (err) {
     *              // 取消选择
     *              return;
     *          }
     *          // 选择成功
     *      })    

```
## 帮助
加入React-Native QQ群 397885169
## 非常感谢

[LuckSiege](https://github.com/LuckSiege/PictureSelector)

[banchichen](https://github.com/banchichen/TZImagePickerController)

[ljunb](https://github.com/ljunb)
