测试场景中从左到右的粒子分别为：
- ### color
显示效果为ColorOverLifetimeModule中color属性设置的颜色值，这是一个常量不会随着时间变化
- ### gradient
显示效果为ColorOverLifetimeModule中color属性设置的渐变色，会随时间线性变化
- ### twoColor
显示效果为ColorOverLifetimeModule中color属性设置的随机颜色，在粒子每次播放开始时随机生成两个设置颜色间的颜色
- ### twoGradient
显示效果为ColorOverLifetimeModule中color属性设置的随机渐变色，在粒子每次播放开始时会在两个设置好的渐变色中间随机生成一个渐变色
- ### randomColor
显示效果为ColorOverLifetimeModule中color属性设置的随机色，在粒子播放过程中会随机选择关键帧中的一个颜色