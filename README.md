# LoadingUtil
封装加载对话框，使其可以在项目中采用依赖的方式引用
详细博客文章详解，请移步 http://www.jianshu.com/p/c958004e6e27

## 补充：关于本例dialog的几种用法：
### 一、风格一，纯文本展示，支持自定义title和style
效果如下：
<img src="/screen/simple.gif" title="" width="270" height="486" /> <br>
1.1 basic：
```java
new LoadingDialog(context).show();
...
new LoadingDialog(context).dismiss();
```
1.2 自定义style：
```java
new LoadingDialog(this, R.style.Custom).show();
```
custom设置如下： 
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Custom" parent="android:Theme.DeviceDefault.Dialog">
        <item name="DialogTitleAppearance">@android:style/TextAppearance.Medium</item>
        <item name="DialogTitleText">Updating…</item>
        <item name="DialogSpotColor">@android:color/holo_orange_dark</item>
        <item name="DialogSpotCount">4</item>
    </style>
</resources>
```
1.3 自定义message：
```java 
new LoadingDialog(this, "Custom message").show();
```
1.4 自定义style 和 message：
```java
new LoadingDialog(this, "Custom message & style", R.style.Custom).show();
```

### 二、风格二，带有自定义逻辑：
显示Material进度样式
```java
    LoadingClickDialog pDialog = new LoadingClickDialog(this, LoadingClickDialog.PROGRESS_TYPE);
    pDialog.getProgressHelper().setBarColor(Color.parseColor("#A5DC86"));
    pDialog.setTitleText("Loading");
    pDialog.setCancelable(false);
    pDialog.show();
```	
<img src="/screen/loading.gif" title="" width="270" height="486" /> <br>
你可以通过**LoadingClickDialog.getProgressHelper()**调用materialish-progress中下面这些方法，来动态改变进度条的样式
```java
- resetCount()
- isSpinning()
- spin()
- stopSpinning()
- getProgress()
- setProgress(float progress)
- setInstantProgress(float progress)
- getCircleRadius()
- setCircleRadius(int circleRadius)
- getBarWidth()
- setBarWidth(int barWidth)
- getBarColor()
- setBarColor(int barColor)
- getRimWidth()
- setRimWidth(int rimWidth)
- getRimColor()
- setRimColor(int rimColor)
- getSpinSpeed()
- setSpinSpeed(float spinSpeed)
```
更多关于dialog的用法，请参见样例代码。

只显示标题：
```java
    new LoadingClickDialog(this)
        .setTitleText("Here's a message!")
        .show();
```
<img src="/screen/basic.gif" title="" width="270" height="486" /> <br>
显示标题和内容：
```java
    new LoadingClickDialog(this)
        .setTitleText("Here's a message!")
        .setContentText("It's pretty, isn't it?")
        .show();
```
显示异常样式：
```java
    new LoadingClickDialog(this, LoadingClickDialog.ERROR_TYPE)
        .setTitleText("Oops...")
        .setContentText("Something went wrong!")
        .show();
```
<img src="/screen/error.gif" title="" width="270" height="486" /> <br>
显示警告样式：
```java
    new LoadingClickDialog(this, LoadingClickDialog.WARNING_TYPE)
        .setTitleText("Are you sure?")
        .setContentText("Won't be able to recover this file!")
        .setConfirmText("Yes,delete it!")
        .show();
```
显示成功完成样式：
```java
    new LoadingClickDialog(this, LoadingClickDialog.SUCCESS_TYPE)
        .setTitleText("Good job!")
        .setContentText("You clicked the button!")
        .show();
```
自定义头部图像：
```java
    new LoadingClickDialog(this, LoadingClickDialog.CUSTOM_IMAGE_TYPE)
        .setTitleText("Sweet!")
        .setContentText("Here's a custom image.")
        .setCustomImage(R.drawable.custom_img)
        .show();
```
确认事件绑定：
```java
    new LoadingClickDialog(this, LoadingClickDialog.WARNING_TYPE)
        .setTitleText("Are you sure?")
        .setContentText("Won't be able to recover this file!")
        .setConfirmText("Yes,delete it!")
        .setConfirmClickListener(new LoadingClickDialog.OnSweetClickListener() {
            @Override
            public void onClick(LoadingClickDialog sDialog) {
                sDialog.dismissWithAnimation();
            }
        })
        .show();
```
显示取消按钮及事件绑定：
```java
    new LoadingClickDialog(this, LoadingClickDialog.WARNING_TYPE)
        .setTitleText("Are you sure?")
        .setContentText("Won't be able to recover this file!")
        .setCancelText("No,cancel plx!")
        .setConfirmText("Yes,delete it!")
        .showCancelButton(true)
        .setCancelClickListener(new LoadingClickDialog.OnSweetClickListener() {
            @Override
            public void onClick(LoadingClickDialog sDialog) {
                sDialog.cancel();
            }
        })
        .show();
```
确认后**切换**对话框样式：
```java
    new LoadingClickDialog(this, LoadingClickDialog.WARNING_TYPE)
        .setTitleText("Are you sure?")
        .setContentText("Won't be able to recover this file!")
        .setConfirmText("Yes,delete it!")
        .setConfirmClickListener(new LoadingClickDialog.OnSweetClickListener() {
            @Override
            public void onClick(LoadingClickDialog sDialog) {
                sDialog
                    .setTitleText("Deleted!")
                    .setContentText("Your imaginary file has been deleted!")
                    .setConfirmText("OK")
                    .setConfirmClickListener(null)
                    .changeAlertType(LoadingClickDialog.SUCCESS_TYPE);
            }
        })
        .show();
```