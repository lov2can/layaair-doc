#Set Matte

> LayaAir's mask can set up an object (supporting bitmaps and vector maps), and then display the mask according to the shape of the object.
>



##Introduction of Mask API

The masking property is located at[laya.display.Sprite](http://layaair.ldc.layabox.com/api/index.html?category=Core&class=laya.display.Sprite%3Ch1%3Emask)Within the API, the description of this property is shown in Figure 1.

![1](img/1.jpg)<br/> (Fig. 1)



##2. Simple mask example

###2.1 Let's first display a bitmap with the LayaAir engine

Create a MaskDemo.js entry program and write the following code:


```javascript

(function()
{
  	var Sprite = Laya.Sprite;
	var Texture = Laya.Texture;
  	var Handler = Laya.Handler;

  	var Res;
	var img;
	(function()
	{
			Laya.init(1136,640);
			//设置舞台背景色
			Laya.stage.bgColor = "#ffffff"      
			//资源路径
			Res = "res/img/monkey1.png";		
			//先加载图片资源，在图片资源加载成功后，通过回调方法绘制图片并添加到舞台
			Laya.loader.load(Res,Handler.create(this,graphicsImg));   
		})();
		
		function graphicsImg()
		{
			img = new Sprite();
			//获取图片资源，绘制到画布
			img.graphics.drawTexture(Laya.loader.getRes(Res),150,50);
			//添加到舞台
			Laya.stage.addChild(img);
		}
})();
```


The operation effect is shown in Figure 2.

![图2](img/2.jpg)<br/> (Figure 2)

###2.2 Create a circular mask area

Create a circular mask area with code. The masking effect can be achieved by mask attributes. Continuing with the code and comments, we changed the 2.1 sample code to the following code:


```javascript

(function()
{
  	var Sprite = Laya.Sprite;
	var Texture = Laya.Texture;
  	var Handler = Laya.Handler;

  	var Res;
	var img;
	(function()
	{
			Laya.init(1136,640);
			//设置舞台背景色
			Laya.stage.bgColor = "#ffffff"      
			//资源路径
			Res = "res/img/monkey1.png";		
			//先加载图片资源，在图片资源加载成功后，通过回调方法绘制图片并添加到舞台
			Laya.loader.load(Res,Handler.create(this,graphicsImg));   
		})();
		
		function graphicsImg()
		{
			img = new Sprite();
			//获取图片资源，绘制到画布
			img.graphics.drawTexture(Laya.loader.getRes(Res),150,50);
			//添加到舞台
			Laya.stage.addChild(img);
			
			//创建遮罩对象
			var cMask = new Sprite();
			//画一个圆形的遮罩区域
			cMask.graphics.drawCircle(80,80,50,"#ff0000");
          	//圆形所在的位置坐标
			cMask.pos(120,50);
         
         	//实现img显示对象的遮罩效果
			img.mask = cMask;
			
		}
})();
```


The operation effect is shown in Figure 3.

![图3](img/3.jpg)<br/> (Figure 3)

By comparing the code, we find that the implementation of mask is very simple. The mask effect of img display object is achieved by assigning the created display object cmack as mask object to the mask attribute of img object.





##3. Setting up masks in Laya AirIDE

> In addition to setting masks directly in the code, you can also easily set masks for objects through LayaAirIDE. Now let's follow the lead step by step.

Step 1: Create a UI page`maskDemo.ui`Import resources.*(If you are not familiar with this step, please go to the IDE section to see the documents related to UI creation and resource import.)*



Step 2: Drag in a resource panel`Image`Components to the scene editing area, as shown in Figure 4

![图4](img/4.png) <br /> （图4）




Step 3: Double-click Enter`Image`Inside the component, then drag a`Sprite`Components, as shown in Figure 5.

![图5](img/5.png)<br/> (Fig. 5)





Step 4: Select`Sprite`Component, in the property panel on the right, common properties`renderType`Set to`mask`As shown in Figure 6.

![图6](img/6.png)<br/> (Fig. 6)



Step 5: Double-click Enter`Sprite`Inside the component, then drag one into the component panel`Graphics`Round components, adjust the position and size. The hierarchical relationship is shown in Figure 7.

![图7](img/7.png)<br/> (Figure 7)



Step 6: Continuously double-click the blank area of the editing area to exit`Image`Inside the component, you can see the effect of the mask, as shown in Figure 8.

![图8](img/8.png)<br/> (Figure 8)





##4. Applying the masks set by LayaAirIDE in the project

###4.1 release UI

On the IDE interface, press`F12`Publish UI pages for masking effects, which will`src/ui`Generate UI classes under the directory, as shown in Figure 9.

![图9](img/9.png) <br />（图9）







###4.2 Use class and Atlas generated by IDE to achieve masking effect

Press F9 to set the startup scenario as maskDemoUI in engine preview.![图9](img/10.png)





The main class code is as follows


```javascript

import GameConfig from "./GameConfig";
class Main {
	constructor() {
		//根据IDE设置初始化引擎		
		if (window["Laya3D"]) Laya3D.init(GameConfig.width, GameConfig.height);
		else Laya.init(GameConfig.width, GameConfig.height, Laya["WebGL"]);
		Laya.stage.scaleMode = GameConfig.scaleMode;
		Laya.stage.screenMode = GameConfig.screenMode;
		Laya.stage.alignV = GameConfig.alignV;
		Laya.stage.alignH = GameConfig.alignH;

		//打开调试面板（通过IDE设置调试模式，或者url地址增加debug=true参数，均可打开调试面板）
		if (GameConfig.debug || Laya.Utils.getQueryString("debug") == "true") Laya.enableDebugPanel();
		if (GameConfig.stat) Laya.Stat.show();
		Laya.alertGlobalError = true;

		//激活资源版本控制，version.json由IDE发布功能自动生成，如果没有也不影响后续流程
		Laya.ResourceVersion.enable("version.json", Laya.Handler.create(this, this.onVersionLoaded), Laya.ResourceVersion.FILENAME_VERSION);
	}

	onVersionLoaded() {
		//激活大小图映射，加载小图的时候，如果发现小图在大图合集里面，则优先加载大图合集，而不是小图
		Laya.AtlasInfoManager.enable("fileconfig.json", Laya.Handler.create(this, this.onConfigLoaded));
	}

	onConfigLoaded() {
		//加载IDE指定的场景
		GameConfig.startScene && Laya.Scene.open(GameConfig.startScene);
	}
}
//激活启动类
new Main();

```


The operation effect is shown in Figure 10. We quickly realized the effect of the mask.

![图10](img/10.jpg)<br/> (Fig. 10)

