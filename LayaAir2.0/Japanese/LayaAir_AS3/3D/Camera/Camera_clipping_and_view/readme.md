#カメラの裁断と視野

###### *version :2.0.1beta   Update:2019-3-19*

####遠近距離の裁断

カメラはまた、遠近距離の裁断を設定し、遠近距離の間のシーンモデルのみを表示し、他のモデルはレンダリング表示しない。ゲームの性能を高めることが最大の強みです。

カメラを作る時、カメラの構造関数は標準的にカットされます。近距離は0.3メートル、遠距離は1000メートルです。開発者はコンストラクション関数に設定したり、カメラのプロパティによって設定したりすることができます。

！[](img/1.png)<br/>(図1)


```typescript

    //创建摄像机时初始化裁剪(横纵比，近距裁剪，远距裁剪)
    var camera:Camera = new Camera( 0, 0.1, 100);
    //近距裁剪
    camera.nearPlane=0;
    //远距裁剪
    camera.farPlane=100;
```


**tips**一般的にゲーム中に霧の効果をカメラと同時にカットして使います。霧の効果は遠いところ以外はほとんど見えません。この時に遠距離カットを設置して、ゲームのレンダリング性能を高めます。

####カメラ視野

カメラの視野は焦点距離に類似しており、視野パラメータの調整により、ビュー中のシーン範囲、透視の近距離変化が見られます。角度値によって調整され、角度が大きいほど、視野範囲が大きくなり、開発者は自分のニーズに応じて設定できます。


```typescript

//设置相机的视野范围90度
camera.fieldOfView = 90;
```

