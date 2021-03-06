# 灵魂手绘板

![](https://cos.56qq.com/fis/20200528113638122fc87f80610ce62b.png)

动画演示

![](https://cos.56qq.com/fis/20200528113201974b758341eaa19bae.gif)

## 配置项

![](https://cos.56qq.com/fis/202005281349331681d996e59af2d7db.png)

### 画笔颜色

设置画笔的颜色

### 笔触大小

设置画笔的大小

### 显示演示控制按钮

是否显示组件左下方的操作按钮，这些按钮并未经过精心设计，只作为组件内置方法的演示之用，用户可以调用这些方法来自己实现对画笔和画布的控制。

## 内置方法

![](https://cos.56qq.com/fis/20200528135603139f81b3bda7ee9ff7.png)

### 设置笔触大小

可选方法 “设置笔触大小”

代码调用 `$vm.setDotSize`

### 设置画笔颜色

可选方法 “设置画笔颜色”

代码调用 `$vm.setPenColor`

### 撤销一步

可选方法 “撤销一步”

代码调用 `$vm.undo`

### 清空画布

可选方法 “清空画布”

代码调用 `$vm.clear`

### 获取图片

此方法不在“可选方法”之列

代码调用 `$vm.save`

### 锁定画布

此方法不在“可选方法”之列

代码调用 `$vm.lock`
