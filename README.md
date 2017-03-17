# webgl-

《webgl编程指南》

## ch03

- 绘制流程

```
gl.createBuffer();
gl.bindBuffer();
gl.bufferData();
gl.getAttribLocation();
gl.vertexAttribPointer();
gl.enableVertexAttribArray();
gl.drawArrays();
```

- 旋转、位移
```
gl.getUniformLocation();
gl.uniform[1-4]f();
```

- 旋转、位移矩阵
```
//利用矩阵工具Matrix4()计算矩阵

new Matrix4();

.setRotate(angle, 0, 0, 1);
```

## ch04

```
//动画
requestAnimationFrame();
```

## ch05

- 点、颜色、大小绘制
```
let vertices = new Float32Array([
    -0.5, 0.5, 5,  1.0, 0.0, 0.0, 1.0,
    -0,   0,   10, 0.0, 1.0, 0.0, 1.0,
    0.5,  0.5, 20, 0.0, 0.0, 1.0, 1.0,
    0.3,  0,   10, 1.0, 0.0, 0.0, 1.0,
    0.5,  0.3, 10, 1.0, 0.0, 0.0, 1.0,
]);

//通过gl.vertexAttribPointer()的第5、6个参数来区分

let fSize = vertices.BYTES_PER_ELEMENT; //获取每个元素所占字节

//以大小为例：
// @fSize * 7 相邻顶点的间距
// @fSize * 2 缓冲区中偏移量
gl.vertexAttribPointer(a_size, 1, gl.FLOAT, false, fSize * 7, fSize * 2);

```

- 纹理

```
//顶点着色器

...
varying vec2 v_texture;
...

//片元着色器
precision mediump float; // 声明所有浮点数默认为中精度 必写 精度限定字

uniform sampler2D u_sampler; //纹理数据类型。这里是2d的所以定义sampler2D 取样器只能用uniform来定义
varying vec2 v_texture;

void main () {
    gl_FragColor = texture2D(u_sampler, v_texture);
}
```

>  在fragment里定义与vertex相同，填充操作由内部完成

1、顶点着色器接收顶点纹理坐标，光栅化后传入片元着色器

2、片元着色器根据片元的纹理坐标， 从图像中抽取出纹理颜色， 赋值当前片元

```
1、与绘制流程类似
2、
    gl.createTexture();
    gl.getUniformLocation();

    //图片加载成功后
    gl.pixelStorei();
    gl.activeTexture();
    gl.bindTexture();
    gl.texParameteri();
    gl.texImage2D();
    gl.uniform1i();
    gl.drawArrays();

```

## ch07

深度信息：在哪里、看哪里、视野宽、能看多远。

观察者 (观察方向、可是距离)

视点：观察者的位置。

视线：从视点出发沿观察方向的射线。

视图矩阵: 表示观察者的状态。
```
             [观察点位置]      (0,0,0)
viewMatrix = [观察点目标位置]  (0,0,-z)
             [上方向]          (0,1,0)
```
模型矩阵：图形经过旋转、位移、缩放变化得到的矩阵。

> 模型视图矩阵 = 视图矩阵 * 模型矩阵

可视空间（正射投影orthographic、透视投影perspective）/

投影矩阵。

> 模型视图投影矩阵 = 投影矩阵 * 模型视图矩阵

投影遮挡问题: 后绘制图形会覆盖先绘制图形。

解决：隐藏面消除

```
/*
 * @param gl.DEPTH_TEST  隐藏面消除
 *        gl.BLEND       混合
 *        gl.POLYGON_OFFSET_FILL 多边形位移
 *        ...
 */
gl.enable(gl.DEPTH_TEST); //开启某个功能

gl.disable()//关闭某个功能

gl.clear(gl.DEPTH_BUFFER_BIT); //清楚深度缓冲区

```
深度冲突
```
    gl.enable(gl.POLYGON_OFFSET_FILL);
    gl.polygonOffset(1.0, 1.0); //设置偏移量

```