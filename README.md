# AGV系统仿真平台（Beta版本）
AGV system simulation platform for scheduling algorithm evaluation


## 文件结构：

```

|-- map.json
|-- mission.json
|-- setting.json
|-- SimulationPlatform  
|-- simulationResult.csv 
`-- log
    |-- log_date1
    |   |-- time1.log
    | 	`--time2.log
    | -- log_date2
    `-- log_date3

```



## 显示：

平台界面在部分较小尺寸屏幕上显示无法完全显示，在window显示设置中更改较小的屏幕缩放比例即可解决这一问题。（当前Beta版本的平台暂不支持自适应显示调节，更多功能将在后续开发完成后发布）

## 输入：

平台输入包括三个json文件，`map.json mission.json setting.json` 需要将三个json文件与平台程序放置在同一个路径下

## 输出：

平台完整运行完成后，会创建以日期-时间为区分的运行日志文件以及运行指标文件`simulationResult.csv`（如果已有csv文件，则追加填写，不覆盖原结果）

## 输入详细说明：

### `map.json`

地图结构只接受网格拓扑结构地图，如有其他需要，也可通过设置网络地图中某些节点与通道的行驶时间为"Infinity"，实现对其他地图结构的模拟。
地图输入包括以下字段：

"ROW": 行数

"ROW_CNV": 经过路径网络转换后的地图行数`ROW_CNV = 2 * ROW - 1`

"COL": 列数

"COL_CNV": 经过路径网络转换后的地图列数`COL_CNV = 2 * COL - 1`

"ROW_DIST": 平均行距（用于真实地图显示的比例尺缩放）

"COL_DIST": 平均列距（用于真实地图显示的比例尺缩放）

"nodeDlt":默认速度下，节点与弧的完整时间，使用二维数组表示，对应关系如下图

```
"""
node0_1			arc0_1-1_1		node1_1
arc0_0-0_1					    arc1_0-1_1
node0_0			arc0_0-1_0		node1_0

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

[
[[node0_1_dlt]		[arc0_1-1_1_dlt]	[node1_1_dlt]]
[[arc0_0-0_1_dlt]	[Infinity]		[arc1_0-1_1_dlt]]
[[node0_0_dlt]		[arc0_0-1_0_dlt]	[node1_0_dlt]]
]
"""
```



"nodeLoc"节点与弧的位置，使用三维数组表示，对应关系如下图

```
"""
node0_1			arc0_1-1_1		node1_1
arc0_0-0_1					    arc1_0-1_1
node0_0			arc0_0-1_0		node1_0

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

[
[[node0_1_loc]		[arc0_1-1_1_loc]	[node1_1_loc]]
[[arc0_0-0_1_loc]	[Infinity]		[arc1_0-1_1_loc]]
[[node0_0_loc]		[arc0_0-1_0_loc]	[node1_0_loc]]
]
"""
```

### `mission.json`

包括以下字段：

"rMissions":所有运输任务，使用三维数组表示。（目前运输任务的到来时间只由任务稀疏系数控制，运输任务中的时间暂不起作用，指定运输任务到来时间的功能将在后续开放）

```
[
[mission1Orig, mission1Dest, mission1Time]
[mission2Orig, mission2Dest, mission2Time]
...
]
```

"lengthList":对应的任务无冲突最短路时间（用于仿真指标计算，不影响仿真过程，可用欧式距离粗略代替）

"totalLength":所有任务无冲突最短路时间之和（用于仿真指标计算，不影响仿真过程，可用欧式距离粗略代替）

### `setting.json`

包括以下字段：

"assignStForMission": 任务驱动的任务指派策略选择，可设置为"NVF"、”LIVF“

 "assignStForCar": 车辆驱动的任务指派策略选择，可设置为"NMF"、”EMF“

 "routeAlgo": 路径规划算法选择，可设置为”TWRA“、ASA

 "conflictSt": 路径冲突应对策略选择，可设置为”CAS“、”CSS“、”MS“

 "carNum": AGV数量，大于0的整数

 "sparsePara": 任务稀疏参数，大于0的实数，参数越大，任务到来越稀疏

 "barrierPara": 障碍参数，0~1的实数，表示系统中障碍平均占所有位置的比例

 "randomSeed": 随机种子设置，任意整数，用于复现仿真结果
 
 "interfencePara": 误差项参数，0~1的实数，用于控制误差项大小，参数越大，路径时间误差越大
 
 "realMapFlag": 是否以真实地图显示，可设置为ture（真实地图）与false（拓扑地图）
