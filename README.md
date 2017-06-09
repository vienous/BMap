入口类 BMap
主要使用到的BMap的子类：
	1.  Map
	2.  Point
	3.  Label
	4.  Marker
	5.  LocalSearch
	6.  Size
	7.  Icon
	8.  Overlay
	9.  InfoWindow

(1) Map类
	构造函数:
		var map = new BMap.Map("container");
		参数为页面元素id
	主要使用的方法:
		1:  map.centerAndZoom(center: Point, zoom: Number)
			注: 设初始化地图。 如果center类型为Point时，zoom必须赋值，
				范围3-19级，若调用高清底图（针对移动端开发）时，zoom可赋值
				范围为3-18级。如果center类型为字符串时，比如“北京”，zoom可
				以忽略，地图将自动根据center适配最佳zoom级别
		2:  map.enableScrollWheelZoom()
			注: 开启鼠标滚轮缩放功能。仅对PC上有效
		3:  map.addOverlay(Marker)
			注: 在地图上添加标注，参数为Marker对象
		4:  map.getDistance(start:Point,end:Point)
			注: 计算地图上两坐标的点的距离，参数为Point对象，单位是米
		5:  map.getOverlays()
			注: 返回地图上的所有覆盖物，返回值为Array< Overlay>	
		6:	map.removeOverlay(Overlay)
			注: 清除覆盖物
		7： map.clearOverlays();
		    注: 清除所有覆盖物
(2) Point类
	构造函数:
		var point =new BMap.Point(lng: Number, lat: Number);
		参数是经纬度
	主要方法:
		1:  point.equals(other: Point)	
			注: 返回值Boolean判断坐标点是否相等，当且仅当两点的经度和纬度均相等时返回true
(3) Label类
	构造函数:
		var label = new BMap.Label(content: String, opts: LabelOptions);
		创建一个文本标注实例。point参数指定了文本标注所在的地理位置
		LabelOptions:
		{
			offset:Szie,
        	position:Point,
        	enableMassClear:false//是否在调用map.clearOverlays清除此覆盖物，默认为true
		}
	主要方法:
		label.setStyle(styles: Object);
			注: 设置文本标注样式，该样式将作用于文本标注的容器元素上。其中
			styles为JavaScript对象	常量，比如： 
			setStyle({ color : "red", fontSize : "12px" }) 注意：如果css的
			属性名	中包含连字符，需要将连字符去掉并将其后的字母进行大写处理，
			例如：背景色属性要写成：backgroundColor
		label.setTitle("大搜家");
		    注: 鼠标移到label后显示内容
(4) Marker类:
	构造函数:
		var marker = new BMap.Marker(Point,MarkerOptions);
		创建一个图像标注实例。point参数指定了图像标注所在的地理位置
		MarkerOptions:
		{
			offset:Size	//标注的位置偏移值
			icon:Icon	//标注所用的图标对象
			enableMassClear:Boolean	//是否在调用map.clearOverlays清除此覆盖物，默认为true
			enableDragging:Boolean	//是否启用拖拽，默认为false
			enableClicking:Boolean	//是否响应点击事件。默认为true
			raiseOnDrag:Boolean	//拖拽标注时，标注是否开启离开地图表面效果。默认为false
			draggingCursor:String //拖拽标注时的鼠标指针样式。此属性值需遵循CSS的cursor属性规范
			rotation:Number	//旋转角度
			shadow:Icon	//阴影图标
			title:String//鼠标移到marker上的显示内容
		}
	主要方法:
		marker.setLabel(label: Label)		
			注: 为标注添加文本标注
		marker.openInfoWindow(infoWnd: InfoWindow)
			注: 打开信息窗口
		marker.addEventListener(event: String, handler: Function)//添加事件监听函数
(5)LocalSearch类
	构造函数:
		var local = new BMap.LocalSearch(location: Map| Point| String, opts: LocalSearchOptions)
		创建一个搜索类实例，其中location表示检索区域，其类型可为地图实例、坐标点或城市名称的字符串。
		当参数为地图实例时，检索位置由当前地图中心点确定，且搜索结果的标注将自动加载到地图上，并
		支持调整地图视野层级；当参数为坐标时，检索位置由该点所在位置确定；当参数为城市名称时，
		检索会在该城市内进行
		LocalSearchOptions:
		{
			renderOptions:LocalRenderOptions//结果呈现设置

			onMarkersSet:Function//标注添加完成后的回调函数。 参数： pois: Array 
									通过marker属性可得到其对应的标注

			onInfoHtmlSet:Function//标注气泡内容创建后的回调函数。 
									参数： poi: LocalResultPoi，通过其marker属性可得到
									当前的标注。 html: HTMLElement，气泡内的Dom元素

			onResultsHtmlSet:Function	//结果列表添加完成后的回调函数。 
										参数： container: HTMLElement，结果列表所用的HTML元素

			pageCapacity:Number	//设置每页容量，取值范围：
								1 - 	100，对于多关键字检索，容量表示每个关键字的数量，
								如果有2个关键字，则实际检索结果数量范围为：2 - 200

			onSearchComplete:Function//检索完成后的回调函数。 
									参数：results: LocalResult或Array 如果是多关键字检索，
									回调函数参数返回一个LocalResult的数组，数组中的结果
									顺序和检索中多关键字数组中顺序一致
		}
		LocalRenderOptions:
		{
			map:Map//展现结果的地图实例。当指定此参数后，
					搜索结果的标注、线路等均会自动添加到此地图上
			panel:String | HTMLElement	//结果列表的HTML容器id或容器元素，
										提供此参数后，结果列表将在此容器中
										进行展示。此属性对LocalCity无效
			selectFirstResult:Boolean//是否选择第一个检索结果。
										此属性仅对LocalSearch有效
			autoViewport:Boolean//检索结束后是否自动调整地图视野。此属性对LocalCity无效
			highlightMode:HighlightModes//驾车结果展现中点击列表后的展现策略
		}
		LocalResult:
			类表示LocalSearch的检索结果，没有构造函数，通过LocalSearch.getResults()方法
			或LocalSearch的onSearchComplete回调函数的参数得到。
		主要属性:
			属性	           类型	              描述
			keyword	         String	         本次检索的关键词
			center	         LocalResultPoi	 周边检索的中心点（仅当周边检索时提供）
			radius	         Number	         周边检索的半径（仅当周边检索时提供）
			bounds	         Bounds	         范围检索的地理区域（仅当范围检索时提供）
			city	         String	         本次检索所在的城市
			moreResultsUrl	 String	         更多结果的链接，到百度地图进行搜索
			province	     String	         本次检索所在的省份
			suggestions	     Array<String>	 搜索建议列表。（当关键词是拼音或拼写错误时给出的搜索建议）
		主要方法:
		result.getPoi(i: Number)//返回值LocalResultPoi	返回索引指定的结果。索引0表示第1条结果
		result.getNumPois()//返回值Number	返回总结果数
		LocalResultPoi：
			此类表示位置检索或路线规划的一个结果点，没有构造函数，可通过对象字面量形式表示。
		主要属性：
			title	String	结果的名称标题
			point	Point	该结果所在的地理位置
			address	String	地址（根据数据部分提供）。注：当结果点类型为公交站或地铁站时，
							地址信息为经过该站点的所有车次
	主要方法:
		local.searchNearby(keyword: String | Array<String>, 
							center: LocalResultPoi| String | Point, 
							radius: Number, option: Object);
			注: 根据中心点、半径与检索词发起周边检索。当keyword为数组时
				将同时执行多关键字的检索，最多支持10个关键字，多关键字自 
				1.2 版本支持。当center为字符串时，半径参数将忽略。注意：
				Point类型的中心点自 1.1 版本支持。option:{customData:CustomData} 
				customData表示检索lbs云服务的数据
(6)Size类
	构造函数:
	var size = new BMap.Size(width: Number, height: Number)
		以指定的宽度和高度创建一个矩形区域大小对象
(7)Icon类
	构造函数:
	var icon = new BMap.Icon(url: String, size: Size, opts: IconOptions)	
		以给定的图像地址和大小创建图标对象实例
		IconOptions:
		{
			anchor:Size	//图标的定位锚点。此点用来决定图标与地理位置的关系，
						是相对于图标左上角的偏移值，默认等于图标宽度和高度的中间值
			imageOffset:Size//图片相对于可视区域的偏移值
			infoWindowAnchor:Size//信息窗口定位锚点。开启信息窗口时，信息窗口底
							部尖角相对于图标左上角的位置，默认等于图标的anchor
			printImageUrl:String//用于打印的图片，此属性只适用于IE6，
								为了解决IE6在包含滤镜的情况下打印样式不正确的问题
		}
(8)Overlay类
	覆盖物的抽象基类，所有覆盖物均继承基类的方法。此类不可实例化。
(9)InfoWindow类
	构造函数:
		var infoWindow = new InfoWindow(content: String | HTMLElement, opts: InfoWindowOptions)
			创建一个信息窗实例，其中content支持HTML内容。1.2版本开始content参数支持传入DOM结点
		InfoWindowOptions:{
			width:Number//信息窗宽度，单位像素。取值范围：0, 220 - 730。
						如果您指定宽度为0，则信息窗口的宽度将按照其内容自动调整
			height:Number//信息窗高度，单位像素。取值范围：0, 60 - 650。如果您指
						定高度为0，则信息窗口的高度将按照其内容自动调整
			maxWidth:Number//信息窗最大化时的宽度，单位像素。取值范围：220 - 730
			offset:Size	//信息窗位置偏移值。默认情况下在地图上打开的信息窗底端的尖角
						将指向其地理坐标，在标注上打开的信息窗底端尖角的位置取决于
						标注所用图标的infoWindowOffset属性值，您可以为信息窗添加偏
						移量来改变默认位置
			title:String//信息窗标题文字，支持HTML内容
			enableAutoPan:Boolean//是否开启信息窗口打开时地图自动移动（默认开启）
			enableCloseOnClick//Boolean	是否开启点击地图关闭信息窗口（默认开启）
			enableMessage:Boolean//是否在信息窗里显示短信发送按钮（默认开启）
			message:String//自定义部分的短信内容，可选项。完整的短信内容包括：自定义部分+位置链接，
							不设置时，显示默认短信内容。短信内容最长为140个字
		}