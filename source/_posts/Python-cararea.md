---
title: 使用Python制作各省市车牌地区代码拼音检索
date: 2020-02-05 15:23:20
categories: Python
tags: [python, pypinyin, json]
---
开发过程中需要制作一个用于输入车牌的选择列表，为了方便输入加入了拼音检索功能，但网上的数据往往只有省份的拼音代码，在此基础上做一些内容的修整

<!--more-->

## 数据下载与预处理

先从网上下载一个已经制作好的json格式车牌地区数据，我用的是[全国省市车牌号json数据](https://download.csdn.net/download/bergtang/11058942)，其他资源其实大同小异。

检查了一下文件内容，还是有些地方需要处理的，如少数民族自治州等没有使用**简称**，而是使用了正式名的全称；还有一些车牌是属于直属系统、特殊系统的。这些都需要手动处理，将城市一级的名称统一化。

处理后的文件内容大致如此：
```json
[
    {
        "code": "冀A",
        "city": "石家庄",
        "province": "河北",
        "Pcode": "HB"
    },
    {
        "code": "冀B",
        "city": "唐山",
        "province": "河北",
        "Pcode": "HB"
    },
    ...
    {
        "code": "沪R",
        "city": "上海",
        "province": "上海",
        "Pcode": "SH"
    }
]
```

## python进行生成拼音处理

数据文件本身有 `Pcode` 字段，只要将它替换成我们要的值就好了。有现成的汉字转拼音库 `pypinyin` 可以利用，安装和使用都可以参考 [汉字拼音转换工具（Python 版）](https://github.com/mozillazg/python-pinyin)。这里用到两个功能，转换成**不含声调的拼音**和**首字母拼音**，例如：

```python
>>> from pypinyin import pinyin, Style
>>> pinyin('冀A 河北 石家庄',style=Style.NORMAL)
[['ji'], ['A '], ['he'], ['bei'], [' '], ['shi'], ['jia'], ['zhuang']]
>>> pinyin('冀A 河北 石家庄',style=Style.FIRST_LETTER)
[['j'], ['A '], ['h'], ['b'], [' '], ['s'], ['j'], ['z']]
```

由于文件中含有中文，还有几个地方要注意：
- 文件的读写需要指定编码 `encoding='UTF-8'`，否则报错 `'gbk' codec can't decode byte .... in position .. : illegal multibyte sequence`
- `dump` 操作时需要注意 `ensure_ascii=False` ，否则中文字符会被替换成ASCII字符


完整代码：

```python
from pypinyin import pinyin, Style
import json

# 读取json文件
file = 'car_area.json'
fs = open(file, 'r', encoding='UTF-8')
dlist = json.load(fs)
fs.close()

# 对内容进行处理
for dist in dlist:
    pcodestr = '%s %s %s'%(dist['code'],dist['city'],dist['province'])
    pcode = ''
    # 不要声调符号的拼音
    pystr = pinyin(pcodestr, style=Style.NORMAL)
    for ilist in pystr:
        pcode += ilist[0]
    pcode += ' '
    # 首字母拼音
    pystr = pinyin(pcodestr, style=Style.FIRST_LETTER)
    for ilist in pystr:
        pcode += ilist[0]
    # 转大写
    pcode=pcode.upper()
    dist['Pcode'] = pcode

# 输出到json文件
file = 'car_area_new.json'
fs = open(file, 'w', encoding='UTF-8')
json.dump(dlist, fs, ensure_ascii=False)
fs.close()
```

## 运行结果

```json
[
    {
        "code": "冀A",
        "city": "石家庄",
        "province": "河北",
        "Pcode": "JIA SHIJIAZHUANG HEBEI JA SJZ HB"
    },
    {
        "code": "冀B",
        "city": "唐山",
        "province": "河北",
        "Pcode": "JIB TANGSHAN HEBEI JB TS HB"
    },
    ...
    {
        "code": "沪R",
        "city": "上海",
        "province": "上海",
        "Pcode": "HUR SHANGHAI SHANGHAI HR SH SH"
    }
]
```
