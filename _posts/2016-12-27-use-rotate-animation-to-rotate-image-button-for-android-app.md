---
layout: post
title: "使用RotateAnimation实现图片按钮旋转"
date: "2016-12-27"
---

##### 定义ImageButton

```xml
<ImageButton
    android:id="@+id/btn"
    android:layout_width="48dp"
    android:layout_height="48dp"
    android:background="@null"
    android:scaleType="fitXY"
  android:src="@drawable/icon"/>
```

```java
ImageButton imageBtn = (ImageButton) findViewById(R.id.btn);
```

##### /res/anim文件夹下定义旋转动画rotate.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<set xmlns:android="http://schemas.android.com/apk/res/android">  
    <rotate  
        android:fromDegrees="0"  
        android:toDegrees="359"  
        android:duration="300"  
        android:repeatCount="-1"  
        android:pivotX="50%"  
        android:pivotY="50%"
        android:interpolator="@android:anim/linear_interpolator"
        />  
</set> 
```

设置补间动画变化率

```
Animation btnAnim = AnimationUtils.loadAnimation(this, R.anim.rotate);  
LinearInterpolator linear = new LinearInterpolator();  
btnAnim.setInterpolator(linear);  
```

+ android:fromDegrees 表示旋转其实位置
+ android:toDegrees 表示旋转结束为止
+ android:duration 动画持续时间，单位毫秒，与旋转速度有关的参数
+ android:repeatCount 动画重复次数，-1表示不停止
+ android:pivotX 旋转中心X坐标
+ android:pivotY 旋转中心Y坐标
+ android:startOffset 正式旋转钱等待数
+ android:zAdjustment 表示动画运行时在z轴上的表现，默认为normal, normal（保持内容当前的z轴顺序），top(运行时在最顶层显示)，bottom（运行时在最底层显示）
+ android:interpolator 设置动画变化率，可以理解为补间动画速率，有这种类型: LinearInterpolator匀速效果，Accelerateinterpolator加速效果，DecelerateInterpolator减速效果，对应xml的写法如下图:

![interpolator_list]({{site.IMG_PATH}}/interpolator_list.png)

根据参数：上面的动画即表示以按钮的中心位置由0的位置开始，顺时针进行359度的旋转。这里的旋转可以使用下面公式求得:

```
旋转速度 = android:duration/(android:toDegrees-android:fromDegrees)
```

##### 加载动画与清除动画
```java
# 开始旋转
if (btnAnim != null) {
  imageBtn.startAnimation(btnAnim); 
}

# 停止旋转
imageBtn.clearAnimation();
```

-完-

### 参考引用

+ [http://t.cn/zQOAvbF](http://t.cn/zQOAvbF)
+ [http://t.cn/RIR5ErI](http://t.cn/RIR5ErI)
+ [http://t.cn/RIR5gMl](http://t.cn/RIR5gMl)
