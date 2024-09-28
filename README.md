### 项目目录

1. 数据补全.py
2. 驶入_驶出_状态检测.py
3. geojson_data.py
4. 加减匀速_直线转弯_状态检测.py
5. 周围船舶检测.py
6. data

---

#### 1. 数据补全.py
下拉到最下方：
```
# 实例化类，并传入数据与补齐频率(自定义时间参数：例如：1h、30min、15min等等)
processor = ShipDataProcessor(df, frequency='30min')

# 调用main方法执行处理，并获得补齐后的数据
filled_data = processor.main()
```
输入：DataFrame格式的文件，补齐的频率（间隔1小时、30分钟、15分钟等）

输出：DataFrame格式的补齐后的数据

---

#### 2. 驶入_驶出_状态检测.py

此部分调用了3. geojson_data.py，其中存放了检测区域定义的信息

下拉到最下方：
```
# 选取感兴趣的船
    data_ship = data[data['name_code'] == 54]

    # 选择检测的时间段
    start_time = '2021-02-08 06:00:00'
    end_time = '2021-02-10 08:00:00'

    # 调用函数，选取数据子集
    data = select_subset_by_time(data_ship, start_time, end_time)

    # 判断驶入或驶出核心函数（输入为data）
    polygon_check_results = detection_entry_exit(data)
```
输入：感兴趣船的数据，轨迹截取的开始时间、结束时间

输出：驶入、驶出、不在区域内的信息

---

#### 4. 加减匀速_直线转弯_状态检测.py
下拉到最下方：
```
analyzer = ShipBehaviorAnalyzer(data)  # 实例化分析类

# 设置时间范围
start_time = pd.Timestamp('2021-01-03 09:26:05')
end_time = pd.Timestamp('2021-01-04 09:26:05')

# 调用类的方法进行行为分类
behavior_df = analyzer.classify_ship_behavior(start_time, end_time, slice_hours=4)
```
输入：检测片段开始时间、截止时间、数据切片时间（例如：2、4、6、8小时）

输出：DataFrame格式的数据，包含检测出的状态信息

---

#### 5. 周围船舶检测.py
下拉到最下方：
```
 # 创建ShipAnalyzer对象并分析54号船
analyzer = ShipAnalyzer("data/all_ships.csv")
analyzer.analyze_and_visualize(ship_code=54, num_within_500m=2, num_outside_500m=3, radius_meters=500)
```
输入：检测主体船只的ID、生成模拟数据的500米内船只数量、500米以外船只数量、检测半径（设定为500米）

输出：检测出的船只数量
