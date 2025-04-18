#### 历史行情数据

##### 历史行情数据-东财

接口: stock_zh_a_hist

目标地址: https://quote.eastmoney.com/concept/sh603777.html?from=classic(示例)

描述: 东方财富-沪深京 A 股日频率数据; 历史数据按日频率更新, 当日收盘价请在收盘后获取

限量: 单次返回指定沪深京 A 股上市公司、指定周期和指定日期间的历史行情日频率数据

输入参数

| 名称         | 类型    | 描述                                                       |
|------------|-------|----------------------------------------------------------|
| symbol     | str   | symbol='603777'; 股票代码可以在 **ak.stock_zh_a_spot_em()** 中获取 |
| period     | str   | period='daily'; choice of {'daily', 'weekly', 'monthly'} |
| start_date | str   | start_date='20210301'; 开始查询的日期                           |
| end_date   | str   | end_date='20210616'; 结束查询的日期                             |
| adjust     | str   | 默认返回不复权的数据; qfq: 返回前复权后的数据; hfq: 返回后复权后的数据               |
| timeout    | float | timeout=None; 默认不设置超时参数                                  |

**股票数据复权**

1. 为何要复权：由于股票存在配股、分拆、合并和发放股息等事件，会导致股价出现较大的缺口。
若使用不复权的价格处理数据、计算各种指标，将会导致它们失去连续性，且使用不复权价格计算收益也会出现错误。
为了保证数据连贯性，常通过前复权和后复权对价格序列进行调整。

2. 前复权：保持当前价格不变，将历史价格进行增减，从而使股价连续。
前复权用来看盘非常方便，能一眼看出股价的历史走势，叠加各种技术指标也比较顺畅，是各种行情软件默认的复权方式。
这种方法虽然很常见，但也有两个缺陷需要注意。

    2.1 为了保证当前价格不变，每次股票除权除息，均需要重新调整历史价格，因此其历史价格是时变的。
这会导致在不同时点看到的历史前复权价可能出现差异。

    2.2 对于有持续分红的公司来说，前复权价可能出现负值。

3. 后复权：保证历史价格不变，在每次股票权益事件发生后，调整当前的股票价格。
后复权价格和真实股票价格可能差别较大，不适合用来看盘。
其优点在于，可以被看作投资者的长期财富增长曲线，反映投资者的真实收益率情况。

4. 在量化投资研究中普遍采用后复权数据。

输出参数-历史行情数据

| 名称   | 类型      | 描述          |
|------|---------|-------------|
| 日期   | object  | 交易日         |
| 股票代码 | object  | 不带市场标识的股票代码 |
| 开盘   | float64 | 开盘价         |
| 收盘   | float64 | 收盘价         |
| 最高   | float64 | 最高价         |
| 最低   | float64 | 最低价         |
| 成交量  | int64   | 注意单位: 手     |
| 成交额  | float64 | 注意单位: 元     |
| 振幅   | float64 | 注意单位: %     |
| 涨跌幅  | float64 | 注意单位: %     |
| 涨跌额  | float64 | 注意单位: 元     |
| 换手率  | float64 | 注意单位: %     |

接口示例-历史行情数据-不复权

```python
import akshare as ak

stock_zh_a_hist_df = ak.stock_zh_a_hist(symbol="000001", period="daily", start_date="20170301", end_date='20240528', adjust="")
print(stock_zh_a_hist_df)
```

数据示例-历史行情数据-不复权

```
            日期    股票代码   开盘   收盘  ... 振幅  涨跌幅  涨跌额 换手率
0     2017-03-01  000001   9.49   9.49  ...  0.84  0.11  0.01  0.21
1     2017-03-02  000001   9.51   9.43  ...  1.26 -0.63 -0.06  0.24
2     2017-03-03  000001   9.41   9.40  ...  0.74 -0.32 -0.03  0.20
3     2017-03-06  000001   9.40   9.45  ...  0.74  0.53  0.05  0.24
4     2017-03-07  000001   9.44   9.45  ...  0.63  0.00  0.00  0.17
...          ...     ...    ...    ...  ...   ...   ...   ...   ...
1755  2024-05-22  000001  11.56  11.56  ...  2.42  0.09  0.01  1.09
1756  2024-05-23  000001  11.53  11.40  ...  1.90 -1.38 -0.16  0.95
1757  2024-05-24  000001  11.37  11.31  ...  1.67 -0.79 -0.09  0.72
1758  2024-05-27  000001  11.31  11.51  ...  1.95  1.77  0.20  0.75
1759  2024-05-28  000001  11.50  11.40  ...  1.91 -0.96 -0.11  0.62
[1760 rows x 12 columns]
```

接口示例-历史行情数据-前复权

```python
import akshare as ak

stock_zh_a_hist_df = ak.stock_zh_a_hist(symbol="000001", period="daily", start_date="20170301", end_date='20240528', adjust="qfq")
print(stock_zh_a_hist_df)
```

数据示例-历史行情数据-前复权

```
           日期    股票代码   开盘    收盘  ...  振幅  涨跌幅  涨跌额 换手率
0     2017-03-01  000001   8.14   8.14  ...  0.98  0.12  0.01  0.21
1     2017-03-02  000001   8.16   8.08  ...  1.47 -0.74 -0.06  0.24
2     2017-03-03  000001   8.06   8.05  ...  0.87 -0.37 -0.03  0.20
3     2017-03-06  000001   8.05   8.10  ...  0.87  0.62  0.05  0.24
4     2017-03-07  000001   8.09   8.10  ...  0.74  0.00  0.00  0.17
...          ...     ...    ...    ...  ...   ...   ...   ...   ...
1755  2024-05-22  000001  11.56  11.56  ...  2.42  0.09  0.01  1.09
1756  2024-05-23  000001  11.53  11.40  ...  1.90 -1.38 -0.16  0.95
1757  2024-05-24  000001  11.37  11.31  ...  1.67 -0.79 -0.09  0.72
1758  2024-05-27  000001  11.31  11.51  ...  1.95  1.77  0.20  0.75
1759  2024-05-28  000001  11.50  11.40  ...  1.91 -0.96 -0.11  0.62
[1760 rows x 12 columns]
```

接口示例-历史行情数据-后复权

```python
import akshare as ak

stock_zh_a_hist_df = ak.stock_zh_a_hist(symbol="000001", period="daily", start_date="20170301", end_date='20240528', adjust="hfq")
print(stock_zh_a_hist_df)
```

数据示例-历史行情数据-后复权

```
           日期    股票代码   开盘     收盘  ...    振幅   涨跌幅   涨跌额 换手率
0     2017-03-01  000001  1575.20  1575.20  ...  0.83  0.10   1.63  0.21
1     2017-03-02  000001  1578.45  1565.45  ...  1.24 -0.62  -9.75  0.24
2     2017-03-03  000001  1562.20  1560.57  ...  0.73 -0.31  -4.88  0.20
3     2017-03-06  000001  1560.57  1568.70  ...  0.73  0.52   8.13  0.24
4     2017-03-07  000001  1567.07  1568.70  ...  0.62  0.00   0.00  0.17
...          ...     ...      ...      ...  ...   ...   ...    ...   ...
1755  2024-05-22  000001  2131.04  2131.04  ...  2.14  0.08   1.62  1.09
1756  2024-05-23  000001  2126.17  2105.04  ...  1.68 -1.22 -26.00  0.95
1757  2024-05-24  000001  2100.16  2090.41  ...  1.47 -0.69 -14.63  0.72
1758  2024-05-27  000001  2090.41  2122.92  ...  1.71  1.56  32.51  0.75
1759  2024-05-28  000001  2121.29  2105.04  ...  1.68 -0.84 -17.88  0.62
[1760 rows x 12 columns]
```

##### 历史行情数据-新浪

接口: stock_zh_a_daily

P.S. 建议切换为 stock_zh_a_hist 接口使用(该接口数据质量高, 访问无限制)

目标地址: https://finance.sina.com.cn/realstock/company/sh600006/nc.shtml(示例)

描述: 新浪财经-沪深京 A 股的数据, 历史数据按日频率更新; 注意其中的 **sh689009** 为 CDR, 请 通过 **ak.stock_zh_a_cdr_daily** 接口获取

限量: 单次返回指定沪深京 A 股上市公司指定日期间的历史行情日频率数据, 多次获取容易封禁 IP

输入参数

| 名称         | 类型  | 描述                                                                                   |
|------------|-----|--------------------------------------------------------------------------------------|
| symbol     | str | symbol='sh600000'; 股票代码可以在 **ak.stock_zh_a_spot()** 中获取                              |
| start_date | str | start_date='20201103'; 开始查询的日期                                                       |
| end_date   | str | end_date='20201116'; 结束查询的日期                                                         |
| adjust     | str | 默认返回不复权的数据; qfq: 返回前复权后的数据; hfq: 返回后复权后的数据; hfq-factor: 返回后复权因子; qfq-factor: 返回前复权因子 |

**股票数据复权**

1.为何要复权：由于股票存在配股、分拆、合并和发放股息等事件，会导致股价出现较大的缺口。
若使用不复权的价格处理数据、计算各种指标，将会导致它们失去连续性，且使用不复权价格计算收益也会出现错误。
为了保证数据连贯性，常通过前复权和后复权对价格序列进行调整。

2.前复权：保持当前价格不变，将历史价格进行增减，从而使股价连续。
前复权用来看盘非常方便，能一眼看出股价的历史走势，叠加各种技术指标也比较顺畅，是各种行情软件默认的复权方式。
这种方法虽然很常见，但也有两个缺陷需要注意。

2.1 为了保证当前价格不变，每次股票除权除息，均需要重新调整历史价格，因此其历史价格是时变的。
这会导致在不同时点看到的历史前复权价可能出现差异。

2.2 对于有持续分红的公司来说，前复权价可能出现负值。

3.后复权：保证历史价格不变，在每次股票权益事件发生后，调整当前的股票价格。
后复权价格和真实股票价格可能差别较大，不适合用来看盘。
其优点在于，可以被看作投资者的长期财富增长曲线，反映投资者的真实收益率情况。

4.在量化投资研究中普遍采用后复权数据。

输出参数-历史行情数据

| 名称                | 类型      | 描述            |
|-------------------|---------|---------------|
| date              | object  | 交易日           |
| open              | float64 | 开盘价           |
| high              | float64 | 最高价           |
| low               | float64 | 最低价           |
| close             | float64 | 收盘价           |
| volume            | float64 | 成交量; 注意单位: 股  |
| amount            | float64 | 成交额; 注意单位: 元  |
| outstanding_share | float64 | 流动股本; 注意单位: 股 |
| turnover          | float64 | 换手率=成交量/流动股本  |

接口示例-历史行情数据(前复权)

```python
import akshare as ak

stock_zh_a_daily_qfq_df = ak.stock_zh_a_daily(symbol="sz000001", start_date="19910403", end_date="20231027", adjust="qfq")
print(stock_zh_a_daily_qfq_df)
```

数据示例-历史行情数据(前复权)

```
           date   open   high  ...        amount  outstanding_share  turnover
0    1991-04-03   0.39   0.39  ...  5.000000e+03       3.710000e+07  0.000003
1    1991-04-04   0.39   0.39  ...  1.500000e+04       3.710000e+07  0.000008
2    1991-04-05   0.38   0.38  ...  1.000000e+04       3.710000e+07  0.000005
3    1991-04-08   0.38   0.38  ...  1.000000e+04       3.710000e+07  0.000005
4    1991-04-09   0.38   0.38  ...  1.900000e+04       3.710000e+07  0.000011
         ...    ...    ...  ...           ...                ...       ...
7730 2023-10-23  10.59  10.60  ...  5.570322e+08       1.940555e+10  0.002733
7731 2023-10-24  10.54  10.61  ...  8.015284e+08       1.940555e+10  0.003920
7732 2023-10-25  10.51  10.54  ...  1.470972e+09       1.940555e+10  0.007273
7733 2023-10-26  10.31  10.42  ...  6.219153e+08       1.940555e+10  0.003092
7734 2023-10-27  10.38  10.48  ...  9.575875e+08       1.940555e+10  0.004740
[7735 rows x 9 columns]
```

接口示例-历史行情数据(后复权)

```python
import akshare as ak

stock_zh_a_daily_hfq_df = ak.stock_zh_a_daily(symbol="sz000001", start_date="19910403", end_date="20231027", adjust="hfq")
print(stock_zh_a_daily_hfq_df)
```

数据示例-历史行情数据(后复权)

```
           date     open     high  ...        amount  outstanding_share  turnover
0    1991-04-03    49.00    49.00  ...  5.000000e+03       3.710000e+07  0.000003
1    1991-04-04    48.76    48.76  ...  1.500000e+04       3.710000e+07  0.000008
2    1991-04-05    48.52    48.52  ...  1.000000e+04       3.710000e+07  0.000005
3    1991-04-08    48.04    48.04  ...  1.000000e+04       3.710000e+07  0.000005
4    1991-04-09    47.80    47.80  ...  1.900000e+04       3.710000e+07  0.000011
         ...      ...      ...  ...           ...                ...       ...
7730 2023-10-23  1341.02  1342.28  ...  5.570322e+08       1.940555e+10  0.002733
7731 2023-10-24  1334.69  1343.55  ...  8.015284e+08       1.940555e+10  0.003920
7732 2023-10-25  1330.89  1334.69  ...  1.470972e+09       1.940555e+10  0.007273
7733 2023-10-26  1305.56  1319.49  ...  6.219153e+08       1.940555e+10  0.003092
7734 2023-10-27  1314.42  1327.09  ...  9.575875e+08       1.940555e+10  0.004740
[7735 rows x 9 columns]
```

接口示例-前复权因子

```python
import akshare as ak

qfq_factor_df = ak.stock_zh_a_daily(symbol="sz000002", adjust="qfq-factor")
print(qfq_factor_df)
```

数据示例-前复权因子

```
         date            qfq_factor
0  2023-08-25    1.0000000000000000
1  2022-08-25    1.0517754291451000
2  2021-08-25    1.1168798473057000
3  2020-08-14    1.1810093745970000
4  2019-08-15    1.2245366427798000
5  2018-08-23    1.2735981991808000
6  2017-08-29    1.3233913520593000
7  2016-07-29    1.3684162947004000
8  2015-07-21    1.4256988372693000
9  2014-05-08    1.4737021651235000
10 2013-05-16    1.5546965200431000
11 2012-07-05    1.5798398599899000
12 2011-05-27    1.6027105929296000
13 2010-05-18    1.6232581646338000
14 2009-06-08    1.6398462042724000
15 2008-06-16    1.6475522484654000
16 2007-05-16    2.6521572780175000
17 2006-07-21    4.0043855834397000
18 2005-06-29    4.1056769557021000
19 2004-05-26    6.3462750504298000
20 2003-05-23    9.5848830885446000
21 2002-07-17   19.4514673699928000
22 2001-08-21   19.7687833140383020
23 2000-08-17   20.0160648287379000
24 2000-01-10   20.2241320306998030
25 1999-08-06   21.7007260053956000
26 1998-07-10   24.0549868667835000
27 1997-07-14   26.7769985385511000
28 1997-06-27   31.2296603923877000
29 1996-08-06   36.0689784010141000
30 1995-07-04   40.3371408451341000
31 1994-06-21   47.9136235499274000
32 1993-04-05   65.9178076663026900
33 1992-03-30   99.0712868582592000
34 1991-06-10  118.8855442299110900
35 1991-01-29  169.1453113717332000
36 1900-01-01  169.1453113717332000
```

接口示例-后复权因子

```python
import akshare as ak

hfq_factor_df = ak.stock_zh_a_daily(symbol="sz000002", adjust="hfq-factor")
print(hfq_factor_df)
```

数据示例-后复权因子

```
         date            hfq_factor
0  2023-08-25  169.1453113717332000
1  2022-08-25  160.8188465756604000
2  2021-08-25  151.4445011965874000
3  2020-08-14  143.2209726780933200
4  2019-08-15  138.1300530033661900
5  2018-08-23  132.8090063887726200
6  2017-08-29  127.8120120012352000
7  2016-07-29  123.6066188533395000
8  2015-07-21  118.6402814886964000
9  2014-05-08  114.7757772056770000
10 2013-05-16  108.7963529802224000
11 2012-07-05  107.0648460362401000
12 2011-05-27  105.5370271575670000
13 2010-05-18  104.2011154213954000
14 2009-06-08  103.1470578954563000
15 2008-06-16  102.6646114132513100
16 2007-05-16   63.7765010294439100
17 2006-07-21   42.2400160641971000
18 2005-06-29   41.1979104047185000
19 2004-05-26   26.6526915438809980
20 2003-05-23   17.6470917599285000
21 2002-07-17    8.6957610011813000
22 2001-08-21    8.5561821729119010
23 2000-08-17    8.4504777946604000
24 2000-01-10    8.3635387226989000
25 1999-08-06    7.7944540348408990
26 1998-07-10    7.0316110463273000
27 1997-07-14    6.3168137059205000
28 1997-06-27    5.4161751759863000
29 1996-08-06    4.6894954853220000
30 1995-07-04    4.1932895546844000
31 1994-06-21    3.5302133055222000
32 1993-04-05    2.5660032904614000
33 1992-03-30    1.7073091178651000
34 1991-06-10    1.4227575982209000
35 1991-01-29    1.0000000000000000
36 1900-01-01    1.0000000000000000
```

##### 历史行情数据-腾讯

接口: stock_zh_a_hist_tx

目标地址: https://gu.qq.com/sh000919/zs

描述: 腾讯证券-日频-股票历史数据; 历史数据按日频率更新, 当日收盘价请在收盘后获取

限量: 单次返回指定沪深京 A 股上市公司、指定周期和指定日期间的历史行情日频率数据

输入参数

| 名称         | 类型    | 描述                                         |
|------------|-------|--------------------------------------------|
| symbol     | str   | symbol='sz000001'; 带市场标识                   |
| start_date | str   | start_date='19000101'; 开始查询的日期             |
| end_date   | str   | end_date='20500101'; 结束查询的日期               |
| adjust     | str   | 默认返回不复权的数据; qfq: 返回前复权后的数据; hfq: 返回后复权后的数据 |
| timeout    | float | timeout=None; 默认不设置超时参数                    |

**股票数据复权**

1.为何要复权：由于股票存在配股、分拆、合并和发放股息等事件，会导致股价出现较大的缺口。
若使用不复权的价格处理数据、计算各种指标，将会导致它们失去连续性，且使用不复权价格计算收益也会出现错误。
为了保证数据连贯性，常通过前复权和后复权对价格序列进行调整。

2.前复权：保持当前价格不变，将历史价格进行增减，从而使股价连续。
前复权用来看盘非常方便，能一眼看出股价的历史走势，叠加各种技术指标也比较顺畅，是各种行情软件默认的复权方式。
这种方法虽然很常见，但也有两个缺陷需要注意。

2.1 为了保证当前价格不变，每次股票除权除息，均需要重新调整历史价格，因此其历史价格是时变的。
这会导致在不同时点看到的历史前复权价可能出现差异。

2.2 对于有持续分红的公司来说，前复权价可能出现负值。

3.后复权：保证历史价格不变，在每次股票权益事件发生后，调整当前的股票价格。
后复权价格和真实股票价格可能差别较大，不适合用来看盘。
其优点在于，可以被看作投资者的长期财富增长曲线，反映投资者的真实收益率情况。

4.在量化投资研究中普遍采用后复权数据。

输出参数-历史行情数据

| 名称     | 类型      | 描述      |
|--------|---------|---------|
| date   | object  | 交易日     |
| open   | float64 | 开盘价     |
| close  | float64 | 收盘价     |
| high   | float64 | 最高价     |
| low    | float64 | 最低价     |
| amount | int64   | 注意单位: 手 |

接口示例-不复权

```python
import akshare as ak

stock_zh_a_hist_tx_df = ak.stock_zh_a_hist_tx(symbol="sz000001", start_date="20200101", end_date="20231027", adjust="")
print(stock_zh_a_hist_tx_df)
```

数据示例-不复权

```
     date   open  close   high    low      amount
0    2020-01-02  16.65  16.87  16.95  16.55  1530231.87
1    2020-01-03  16.94  17.18  17.31  16.92  1116194.81
2    2020-01-06  17.01  17.07  17.34  16.91   862083.50
3    2020-01-07  17.13  17.15  17.28  16.95   728607.56
4    2020-01-08  17.00  16.66  17.05  16.63   847824.12
..          ...    ...    ...    ...    ...         ...
920  2023-10-23  10.59  10.50  10.60  10.43   530404.00
921  2023-10-24  10.54  10.55  10.61  10.44   760653.00
922  2023-10-25  10.51  10.38  10.54  10.36  1411450.00
923  2023-10-26  10.31  10.41  10.42  10.30   599991.00
924  2023-10-27  10.38  10.45  10.48  10.33   919771.00
[925 rows x 6 columns]
```

接口示例-前复权

```python
import akshare as ak

stock_zh_a_hist_tx_df = ak.stock_zh_a_hist_tx(symbol="sz000001", start_date="20200101", end_date="20231027", adjust="qfq")
print(stock_zh_a_hist_tx_df)
```

数据示例-前复权

```
     date   open  close   high    low      amount
0    2020-01-02  15.74  15.96  16.04  15.64  1530231.87
1    2020-01-03  16.03  16.27  16.40  16.01  1116194.81
2    2020-01-06  16.10  16.16  16.43  16.00   862083.50
3    2020-01-07  16.22  16.24  16.37  16.04   728607.56
4    2020-01-08  16.09  15.75  16.14  15.72   847824.12
..          ...    ...    ...    ...    ...         ...
920  2023-10-23  10.59  10.50  10.60  10.43   530404.00
921  2023-10-24  10.54  10.55  10.61  10.44   760653.00
922  2023-10-25  10.51  10.38  10.54  10.36  1411450.00
923  2023-10-26  10.31  10.41  10.42  10.30   599991.00
924  2023-10-27  10.38  10.45  10.48  10.33   919771.00
[925 rows x 6 columns]
```

接口示例-后复权

```python
import akshare as ak

stock_zh_a_hist_tx_df = ak.stock_zh_a_hist_tx(symbol="sz000001", start_date="20200101", end_date="20231027", adjust="hfq")
print(stock_zh_a_hist_tx_df)
```

数据示例-后复权

```
     date     open    close     high      low      amount
0    2020-01-02  1880.18  1904.07  1912.76  1869.33  1530231.87
1    2020-01-03  1911.67  1937.73  1951.85  1909.50  1116194.81
2    2020-01-06  1919.27  1925.79  1955.11  1908.42   862083.50
3    2020-01-07  1932.30  1934.48  1948.59  1912.76   728607.56
4    2020-01-08  1918.19  1881.27  1923.62  1878.01   847824.12
..          ...      ...      ...      ...      ...         ...
920  2023-10-23  1321.10  1311.33  1322.19  1303.73   530404.00
921  2023-10-24  1315.67  1316.76  1323.27  1304.81   760653.00
922  2023-10-25  1312.41  1298.30  1315.67  1296.13  1411450.00
923  2023-10-26  1290.70  1301.56  1302.64  1289.61   599991.00
924  2023-10-27  1298.30  1305.90  1309.16  1292.87   919771.00
[925 rows x 6 columns]
```

##### 分时数据-新浪

接口: stock_zh_a_minute

目标地址: http://finance.sina.com.cn/realstock/company/sh600519/nc.shtml

描述: 新浪财经-沪深京 A 股股票或者指数的分时数据，目前可以获取 1, 5, 15, 30, 60 分钟的数据频率, 可以指定是否复权

限量: 单次返回指定股票或指数的指定频率的最近交易日的历史分时行情数据; 注意调用频率

输入参数

| 名称     | 类型  | 描述                                                         |
|--------|-----|------------------------------------------------------------|
| symbol | str | symbol='sh000300'; 同日频率数据接口                                |
| period | str | period='1'; 获取 1, 5, 15, 30, 60 分钟的数据频率                    |
| adjust | str | adjust=""; 默认为空: 返回不复权的数据; qfq: 返回前复权后的数据; hfq: 返回后复权后的数据; |

输出参数

| 名称     | 类型      | 描述  |
|--------|---------|-----|
| day    | object  | -   |
| open   | float64 | -   |
| high   | float64 | -   |
| low    | float64 | -   |
| close  | float64 | -   |
| volume | float64 | -   |

接口示例

```python
import akshare as ak

stock_zh_a_minute_df = ak.stock_zh_a_minute(symbol='sh600751', period='1', adjust="qfq")
print(stock_zh_a_minute_df)
```

数据示例

```
                       day  open  high   low  close  volume
0      2020-06-17 14:49:00  3.05  3.05  3.04   3.04  133200
1      2020-06-17 14:50:00  3.05  3.05  3.04   3.04  131900
2      2020-06-17 14:51:00  3.04  3.05  3.04   3.04  332700
3      2020-06-17 14:52:00  3.05  3.05  3.04   3.04   71100
4      2020-06-17 14:53:00  3.04  3.05  3.04   3.04   49200
                    ...   ...   ...   ...    ...     ...
19995  2020-10-23 14:54:00  3.52  3.52  3.51   3.52  234900
19996  2020-10-23 14:55:00  3.51  3.52  3.51   3.52  337300
19997  2020-10-23 14:56:00  3.52  3.53  3.51   3.52  198067
19998  2020-10-23 14:57:00  3.53  3.53  3.52   3.53  225800
19999  2020-10-23 15:00:00  3.52  3.52  3.52   3.52  259123
```

##### 分时数据-东财

接口: stock_zh_a_hist_min_em

目标地址: https://quote.eastmoney.com/concept/sh603777.html

描述: 东方财富网-行情首页-沪深京 A 股-每日分时行情; 该接口只能获取近期的分时数据，注意时间周期的设置

限量: 单次返回指定股票、频率、复权调整和时间区间的分时数据, 其中 1 分钟数据只返回近 5 个交易日数据且不复权

输入参数

| 名称         | 类型  | 描述                                                                                                  |
|------------|-----|-----------------------------------------------------------------------------------------------------|
| symbol     | str | symbol='000300'; 股票代码                                                                               |
| start_date | str | start_date="1979-09-01 09:32:00"; 日期时间; 默认返回所有数据                                                    |
| end_date   | str | end_date="2222-01-01 09:32:00"; 日期时间; 默认返回所有数据                                                      |
| period     | str | period='5'; choice of {'1', '5', '15', '30', '60'}; 其中 1 分钟数据返回近 5 个交易日数据且不复权                       |
| adjust     | str | adjust=''; choice of {'', 'qfq', 'hfq'}; '': 不复权, 'qfq': 前复权, 'hfq': 后复权, 其中 1 分钟数据返回近 5 个交易日数据且不复权 |

输出参数-1分钟数据

| 名称  | 类型      | 描述      |
|-----|---------|---------|
| 时间  | object  | -       |
| 开盘  | float64 | -       |
| 收盘  | float64 | -       |
| 最高  | float64 | -       |
| 最低  | float64 | -       |
| 成交量 | float64 | 注意单位: 手 |
| 成交额 | float64 | -       |
| 均价  | float64 | -       |

接口示例-1分钟数据

```python
import akshare as ak

# 注意：该接口返回的数据只有最近一个交易日的有开盘价，其他日期开盘价为 0
stock_zh_a_hist_min_em_df = ak.stock_zh_a_hist_min_em(symbol="000001", start_date="2024-03-20 09:30:00", end_date="2024-03-20 15:00:00", period="1", adjust="")
print(stock_zh_a_hist_min_em_df)
```

数据示例-1分钟数据

```
               时间     开盘     收盘     最高     最低    成交量    成交额     均价
0    2024-03-20 09:30:00  10.38  10.38  10.38  10.38   7174   7446612.0  10.380
1    2024-03-20 09:31:00  10.38  10.40  10.40  10.38  12967  13471486.0  10.386
2    2024-03-20 09:32:00  10.40  10.41  10.41  10.39  10409  10824353.0  10.390
3    2024-03-20 09:33:00  10.41  10.41  10.41  10.40   4896   5095656.0  10.393
4    2024-03-20 09:34:00  10.40  10.39  10.41  10.39   7060   7341787.0  10.394
..                   ...    ...    ...    ...    ...    ...         ...     ...
236  2024-03-20 14:56:00  10.44  10.44  10.45  10.44   2646   2762914.0  10.431
237  2024-03-20 14:57:00  10.44  10.44  10.45  10.44   7613   7951367.0  10.431
238  2024-03-20 14:58:00  10.45  10.45  10.45  10.45    110    114850.0  10.431
239  2024-03-20 14:59:00  10.45  10.45  10.45  10.45      0         0.0  10.431
240  2024-03-20 15:00:00  10.45  10.45  10.45  10.45  13079  13667555.0  10.431
[241 rows x 8 columns]
```

输出参数-其他

| 名称  | 类型      | 描述      |
|-----|---------|---------|
| 时间  | object  | -       |
| 开盘  | float64 | -       |
| 收盘  | float64 | -       |
| 最高  | float64 | -       |
| 最低  | float64 | -       |
| 涨跌幅 | float64 | 注意单位: % |
| 涨跌额 | float64 | -       |
| 成交量 | float64 | 注意单位: 手 |
| 成交额 | float64 | -       |
| 振幅  | float64 | 注意单位: % |
| 换手率 | float64 | 注意单位: % |

接口示例-其他

```python
import akshare as ak

stock_zh_a_hist_min_em_df = ak.stock_zh_a_hist_min_em(symbol="000001", start_date="2024-03-20 09:30:00", end_date="2024-03-20 15:00:00", period="5", adjust="hfq")
print(stock_zh_a_hist_min_em_df)
```

数据示例-其他

```
             时间       开盘       收盘  ...         成交额    振幅   换手率
0   2024-03-20 09:35:00  1939.26  1940.89  ...  53652034.0  0.25  0.03
1   2024-03-20 09:40:00  1939.26  1940.89  ...  27557310.0  0.17  0.01
2   2024-03-20 09:45:00  1939.26  1939.26  ...  11556035.0  0.08  0.01
3   2024-03-20 09:50:00  1940.89  1945.76  ...  27377765.0  0.42  0.01
4   2024-03-20 09:55:00  1947.39  1944.14  ...  37867949.0  0.17  0.02
5   2024-03-20 10:00:00  1945.76  1947.39  ...  27334901.0  0.25  0.01
6   2024-03-20 10:05:00  1945.76  1945.76  ...  14407157.0  0.17  0.01
7   2024-03-20 10:10:00  1944.14  1950.64  ...  46100711.0  0.33  0.02
8   2024-03-20 10:15:00  1950.64  1949.01  ...  35531670.0  0.25  0.02
9   2024-03-20 10:20:00  1949.01  1945.76  ...  20072949.0  0.17  0.01
10  2024-03-20 10:25:00  1947.39  1945.76  ...  11039368.0  0.17  0.01
11  2024-03-20 10:30:00  1945.76  1942.51  ...  29766381.0  0.17  0.01
12  2024-03-20 10:35:00  1942.51  1942.51  ...   9093103.0  0.08  0.00
13  2024-03-20 10:40:00  1944.14  1944.14  ...   9585217.0  0.17  0.00
14  2024-03-20 10:45:00  1944.14  1944.14  ...   9546537.0  0.17  0.00
15  2024-03-20 10:50:00  1944.14  1945.76  ...   9917328.0  0.08  0.00
16  2024-03-20 10:55:00  1945.76  1942.51  ...  10891142.0  0.17  0.01
17  2024-03-20 11:00:00  1944.14  1944.14  ...   7600144.0  0.08  0.00
18  2024-03-20 11:05:00  1944.14  1944.14  ...   7900327.0  0.17  0.00
19  2024-03-20 11:10:00  1942.51  1944.14  ...   7612710.0  0.08  0.00
20  2024-03-20 11:15:00  1942.51  1949.01  ...  47061728.0  0.42  0.02
21  2024-03-20 11:20:00  1950.64  1947.39  ...  19120650.0  0.25  0.01
22  2024-03-20 11:25:00  1947.39  1952.26  ...  58101226.0  0.33  0.03
23  2024-03-20 11:30:00  1953.89  1952.26  ...  36299314.0  0.17  0.02
24  2024-03-20 13:05:00  1952.26  1950.64  ...  49781778.0  0.33  0.02
25  2024-03-20 13:10:00  1950.64  1949.01  ...  25451905.0  0.25  0.01
26  2024-03-20 13:15:00  1949.01  1949.01  ...  17763129.0  0.17  0.01
27  2024-03-20 13:20:00  1949.01  1950.64  ...  17839258.0  0.17  0.01
28  2024-03-20 13:25:00  1949.01  1950.64  ...  13335325.0  0.17  0.01
29  2024-03-20 13:30:00  1950.64  1949.01  ...   5756958.0  0.08  0.00
30  2024-03-20 13:35:00  1949.01  1950.64  ...   4852792.0  0.08  0.00
31  2024-03-20 13:40:00  1950.64  1950.64  ...  16137370.0  0.17  0.01
32  2024-03-20 13:45:00  1949.01  1949.01  ...   4450010.0  0.08  0.00
33  2024-03-20 13:50:00  1949.01  1950.64  ...   4778321.0  0.08  0.00
34  2024-03-20 13:55:00  1949.01  1949.01  ...  12025904.0  0.17  0.01
35  2024-03-20 14:00:00  1947.39  1949.01  ...   3459610.0  0.08  0.00
36  2024-03-20 14:05:00  1947.39  1949.01  ...   7610675.0  0.17  0.00
37  2024-03-20 14:10:00  1950.64  1950.64  ...  13966439.0  0.17  0.01
38  2024-03-20 14:15:00  1950.64  1952.26  ...   5955098.0  0.08  0.00
39  2024-03-20 14:20:00  1950.64  1950.64  ...  17385018.0  0.17  0.01
40  2024-03-20 14:25:00  1949.01  1950.64  ...   4410047.0  0.08  0.00
41  2024-03-20 14:30:00  1950.64  1947.39  ...  14639483.0  0.17  0.01
42  2024-03-20 14:35:00  1949.01  1947.39  ...  13156283.0  0.08  0.01
43  2024-03-20 14:40:00  1949.01  1949.01  ...  12261331.0  0.08  0.01
44  2024-03-20 14:45:00  1949.01  1950.64  ...  10861743.0  0.08  0.01
45  2024-03-20 14:50:00  1949.01  1950.64  ...  22807259.0  0.17  0.01
46  2024-03-20 14:55:00  1950.64  1949.01  ...  12102477.0  0.08  0.01
47  2024-03-20 15:00:00  1949.01  1950.64  ...  24496686.0  0.08  0.01
[48 rows x 11 columns]
```

##### 日内分时数据-东财

接口: stock_intraday_em

目标地址: https://quote.eastmoney.com/f1.html?newcode=0.000001

描述: 东方财富-分时数据

限量: 单次返回指定股票最近一个交易日的分时数据, 包含盘前数据

输入参数

| 名称         | 类型  | 描述                                  |
|------------|-----|-------------------------------------|
| symbol     | str | symbol="000001"; 股票代码               |

输出参数

| 名称    | 类型      | 描述 |
|-------|---------|----|
| 时间    | object  | -  |
| 成交价   | float64 | -  |
| 手数    | int64   | -  |
| 买卖盘性质 | object  | -  |

接口示例

```python
import akshare as ak

stock_intraday_em_df = ak.stock_intraday_em(symbol="000001")
print(stock_intraday_em_df)
```

数据示例

```
         时间    成交价    手数 买卖盘性质
0     09:15:00  10.60    11   中性盘
1     09:15:09  10.57  1547   中性盘
2     09:15:18  10.57  1549   中性盘
3     09:15:27  10.57  1549   中性盘
4     09:15:36  10.57  1552   中性盘
        ...    ...   ...   ...
4396  14:56:51  10.50    30    买盘
4397  14:56:54  10.49   109    卖盘
4398  14:56:57  10.49    27    卖盘
4399  14:57:00  10.50    68    买盘
4400  15:00:00  10.50  8338    买盘
[4401 rows x 4 columns]
```

##### 日内分时数据-新浪

接口: stock_intraday_sina

目标地址: https://vip.stock.finance.sina.com.cn/quotes_service/view/cn_bill.php?symbol=sz000001

描述: 新浪财经-日内分时数据

限量: 单次返回指定交易日的分时数据；只能获取近期的数据

输入参数

| 名称     | 类型  | 描述                            |
|--------|-----|-------------------------------|
| symbol | str | symbol="sz000001"; 带市场标识的股票代码 |
| date   | str | date="20240321"; 交易日          |

输出参数

| 名称         | 类型      | 描述            |
|------------|---------|---------------|
| symbol     | object  | -             |
| name       | object  | -             |
| ticktime   | object  | -             |
| price      | float64 | -             |
| volume     | int64   | 注意单位: 股       |
| prev_price | float64 | -             |
| kind       | object  | D 表示卖盘，表示 是买盘 |

接口示例

```python
import akshare as ak

stock_intraday_sina_df = ak.stock_intraday_sina(symbol="sz000001", date="20240321")
print(stock_intraday_sina_df)
```

数据示例

```
        symbol  name  ticktime  price   volume  prev_price kind
0    sz000001  平安银行  09:25:00  10.45   437400        0.00    U
1    sz000001  平安银行  09:30:00  10.44    29100       10.45    D
2    sz000001  平安银行  09:30:03  10.45   356400       10.44    U
3    sz000001  平安银行  09:30:06  10.45    65500       10.45    D
4    sz000001  平安银行  09:30:09  10.46    35800       10.45    U
..        ...   ...       ...    ...      ...         ...  ...
818  sz000001  平安银行  14:56:03  10.46    22100       10.46    D
819  sz000001  平安银行  14:56:18  10.47    20700       10.46    U
820  sz000001  平安银行  14:56:24  10.47   156000       10.47    U
821  sz000001  平安银行  14:56:45  10.46    78900       10.47    D
822  sz000001  平安银行  15:00:00  10.47  1472200       10.46    E
[823 rows x 7 columns]
```

##### 盘前数据

接口: stock_zh_a_hist_pre_min_em

目标地址: https://quote.eastmoney.com/concept/sh603777.html

描述: 东方财富-股票行情-盘前数据

限量: 单次返回指定 symbol 的最近一个交易日的股票分钟数据, 包含盘前分钟数据

输入参数

| 名称         | 类型  | 描述                                  |
|------------|-----|-------------------------------------|
| symbol     | str | symbol="000001"; 股票代码               |
| start_time | str | start_time="09:00:00"; 时间; 默认返回所有数据 |
| end_time   | str | end_time="15:40:00"; 时间; 默认返回所有数据   |

输出参数

| 名称  | 类型      | 描述      |
|-----|---------|---------|
| 时间  | object  | -       |
| 开盘  | float64 | -       |
| 收盘  | float64 | -       |
| 最高  | float64 | -       |
| 最低  | float64 | -       |
| 成交量 | float64 | 注意单位: 手 |
| 成交额 | float64 | -       |
| 最新价 | float64 | -       |

接口示例

```python
import akshare as ak

stock_zh_a_hist_pre_min_em_df = ak.stock_zh_a_hist_pre_min_em(symbol="000001", start_time="09:00:00", end_time="15:40:00")
print(stock_zh_a_hist_pre_min_em_df)
```

数据示例

```
          时间             开盘  收盘   最高  最低    成交量    成交额  最新价
0    2023-12-11 09:15:00  9.30  9.30  9.30  9.30     0        0.0  9.300
1    2023-12-11 09:16:00  9.25  9.28  9.28  9.25     0        0.0  9.300
2    2023-12-11 09:17:00  9.28  9.28  9.28  9.28     0        0.0  9.300
3    2023-12-11 09:18:00  9.28  9.28  9.28  9.28     0        0.0  9.300
4    2023-12-11 09:19:00  9.28  9.27  9.28  9.27     0        0.0  9.300
..                   ...   ...   ...   ...   ...   ...        ...    ...
251  2023-12-11 14:56:00  9.35  9.35  9.35  9.34  7793  7283721.0  9.214
252  2023-12-11 14:57:00  9.34  9.36  9.36  9.34  5169  4833563.0  9.215
253  2023-12-11 14:58:00  9.36  9.36  9.36  9.36    22    20592.0  9.215
254  2023-12-11 14:59:00  9.36  9.36  9.36  9.36     0        0.0  9.215
255  2023-12-11 15:00:00  9.35  9.35  9.35  9.35  7138  6674030.0  9.216
[256 rows x 8 columns]
```
