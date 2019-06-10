# 相机

游戏中的相机是用来捕捉场景画面的主要工具。我们通过调节相机相关参数来控制可视范围的大小，在 Cocos 3D 编辑器中相机呈如下表示：

![camera](camera/camera.jpg)

相机的可视范围是通过6的平面组成一个 **视锥体（Frustum）** 构成， **近裁剪面（Near Plane）** 和 **远裁剪面（Far Plane）** 用于控制近处和远处的可视距离与范围，同时它们也构成了视口的大小。

![camera view](camera/camera-view.gif)

---

继续前往 [光源](light.md) 说明文档。