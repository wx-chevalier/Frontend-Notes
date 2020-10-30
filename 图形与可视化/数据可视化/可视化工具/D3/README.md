# D3

D3 是一个 JavaScript 库，用于在网络上创建定制的交互式图表和地图。虽然大多数图表库（如 Chart.js 和 Highcharts）都提供了现成的图表，但 D3 由大量的构件组成，可以从这些构件中构建自定义的图表或地图。D3 的方法比其他图表库的层次要低得多。用 Chart.js 创建一个条形图只需要几行代码。

用 D3 创建同样的图表，你需要：创建 SVG 矩形元素并将它们连接到数据上；矩形元素定位；根据数据确定矩形元素的大小；最后添加坐标轴。您可能还想当图表首次加载时，对条形图进行动画处理；容器自适应处理；添加工具提示等。这都是额外的工作，但它能让你完全控制图表的外观和行为。

如果一个标准的条形图、线形图或饼形图就足够了，你应该考虑使用 Chart.js 这样的库。然而，如果你希望按照精确的规格创建一个定制的图表，那么 D3 是值得考虑的。D3 的功能包括：

- 数据驱动的 HTML/SVG 元素的修改
- 加载和转换数据（如 CSV 数据）。
- 生成复杂的图表，如树状图、包装圆和网络。
- 一个强大的过渡系统，用于在不同的视图之间制作动画
- 强大的用户交互支持，包括平移、缩放和拖动。

# Hello World

![D3 示例柱状图](https://s1.ax1x.com/2020/10/30/BtAZ4I.png)

```html
<!DOCTYPE html>
<meta charset="utf-8">
<head>
	<title>DOM Manipulation</title>
</head>

<style>
	body {
		font-family: "Helvetica Neue", Helvetica, sans-serif;
		font-size: 14px;
	}
	#wrapper {
		height: 220px;
	}

	.person {
		height: 20px;
		position: relative;
	}
	.person .label {
		width: 90px;
		text-align: right;
	}
	.person .bar {
		height: 19px;
		background-color: steelblue;
		position: absolute;
		left: 100px;
	}
	.person div {
		display: inline-block;
	}
	.data-view {
		padding: 10px;
		color: #777;
		font-size: 12px;
		width: 700px;
	}
</style>

<body>

	<div id="wrapper">
	</div>

	<div class="menu">
		<button onClick="updateScores();">Update scores</button>
		<button onClick="addPerson();">Add person</button>
		<button onClick="removePerson();">Remove person</button>
	</div>

	<!-- <div class="data-view"></div> -->

	<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/4.2.2/d3.min.js"></script>
	<script>

	var names = ['Andy', 'Beth', 'Craig', 'Diane', 'Evelyn', 'Fred', 'Georgia', 'Harry', 'Isabel', 'John'];
	var myData = [];
	var barWidth = 400;
	var barScale = d3.scaleLinear().domain([0, 100]).range([0, barWidth]);

	function randomInteger(n) {
		return Math.floor(10 * Math.random());
	}

	function initialiseData() {
		myData = [
		  {
		    "name": "Andy",
		    "score": 37
		  },
		  {
		    "name": "Beth",
		    "score": 39
		  },
		  {
		    "name": "Craig",
		    "score": 31
		  },
		  {
		    "name": "Diane",
		    "score": 35
		  },
		  {
		    "name": "Evelyn",
		    "score": 38
		  }
		];
	}

	function updateBars(data) {
		var u = d3.select('#wrapper')
			.selectAll('.person')
			.data(data, function(d) {
				return d.name;
			});

		var entering = u.enter()
			.append('div')
			.classed('person', true);

		entering.append('div')
			.classed('label', true)
			.text(function(d) {
				return d.name;
			});

		entering.append('div')
			.classed('bar', true);

		entering
			.merge(u)
			.select('.bar')
			.transition()
			.style('width', function(d) {
				return barScale(d.score) + 'px';
			});

		u.exit().remove();
	}

	function addPerson() {
		if(myData.length === 10)
			return;

		myData.push({
			name: names[myData.length],
			score: 30 + randomInteger(70)
		});

		update(myData);
	}

	function removePerson() {
		if(myData.length === 0)
			return;

		myData.pop();

		update(myData);
	}

	function updateScores() {
		for(var i = 0; i < myData.length; i++) {
			myData[i].score = 30 + randomInteger(70);
		}

		update(myData);
	}

	function updateDataView() {
		d3.select('.data-view').text('Array: ' + JSON.stringify(myData));
	}

	function update() {
		updateBars(myData);
		updateDataView(myData);
	}

	initialiseData();
	update(myData);


	</script>
</body>
</html>
```
