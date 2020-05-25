# vue-echarts-demo

> vue echarts animation demo  
> 用Echarts实现的动画例子

## 构建步骤

``` bash
# 安装依赖
npm install

# 启动本地服务 localhost:8585
npm run dev

# 生产环境构建
npm run build

# 构建并查看分析报告
npm run build --report
```

## 功能
### 随机点
点击刷新散点图按钮，重新生成一张新的画布，随机生成任意个3D坐标点
![](https://gitee.com/taojunnan/blog-resources/raw/master/static/20200525234104.jpg)

### 播放
点击播放按钮，遍历图中每个3D坐标点并高亮成红色，只有在遍历到该点的时候高亮成玫红色，其余点保持原来的颜色。设置好了播放动画的频率，即帧幅，动画每跳一个点的间隔时间可设置成一秒
![](https://gitee.com/taojunnan/blog-resources/raw/master/static/20200525234757.png)

## 暂停
点击暂停按钮，动画效果暂停。再点击播放按钮，动画效果从该点开始继续往下遍历
