---
title: 机器学习实践笔记
date: 2022-05-26 15:05:42
tags: 机器学习
categories: 机器学习
cover: /img/fantasy-2049567_960_720.jpg
---

机器学习笔记！
<!-- more -->   

# python基础实践

## 文件解压与遍历

```python
import zipfile
import os
def unzip_data(src_path,target_path):
    # 解压原始数据集，将src_path路径下的zip包解压至target_path目录下
    if(not os.path.isdir(target_path)):     
        z = zipfile.ZipFile(src_path, 'r')
        z.extractall(path=target_path)
        z.close()

unzip_data('data/data10954/cat_12_test.zip','data/data10954/cat_12_test')
unzip_data('data/data10954/cat_12_train.zip','data/data10954/cat_12_train')

```

> `files = os.listdir(path)` 
返回指定路径下的文件和文件夹列表

> `os.path.join(path, filename)`
把目录和文件名合成一个路径
`print( os.path.join('root','test','runoob.txt') ) `
输出：root/test/runoob.txt

参考：https://www.runoob.com/python3/python3-os-path.html


> `os.path.splitext(temp_path)[1]`
分割路径中的文件名与拓展名

`dict.setdefault(key, default=None)`如果键不存在于字典中，将会添加键并

在python中 % 操作符可以实现字符串格式化

```python
import os
 
"""
通过给定目录，统计所有的不同子文件类型及占用内存
"""
#字典size_dict记录文件大小和type_dict记录类型
size_dict = {}
type_dict = {}
def get_size_type(path):
    files = os.listdir(path)
    
    for filename in files:
        temp_path = os.path.join(path, filename)
        if os.path.isdir(temp_path):
            # 递归调用函数，实现深度文件名解析
            get_size_type(temp_path)     
        elif os.path.isfile(temp_path):
            # 获取文件后缀
            type_name=os.path.splitext(temp_path)[1]   
            #无后缀名的文件
            if not type_name:
                #dict.setdefault(key, default=None)如果键不存在于字典中，将会添加键并将值设为默认值
                type_dict.setdefault("None", 0)
                type_dict["None"] += 1
                size_dict.setdefault("None", 0)
                size_dict["None"] += os.path.getsize(temp_path)
            # 有后缀的文件
            else:
                type_dict.setdefault(type_name, 0)
                type_dict[type_name] += 1
                size_dict.setdefault(type_name, 0)
                # 获取文件大小
                size_dict[type_name] += os.path.getsize(temp_path)  
    

path= "data/"
get_size_type(path) #改变size_dict type_dict
for each_type in type_dict.keys():
    print ("%5s下共有【%5s】的文件【%5d】个,占用内存【%7.2f】MB" %     
            (path,each_type,type_dict[each_type],\
            size_dict[each_type]/(1024*1024)))
print("总文件数:  【%d】"%(sum(type_dict.values())))
print("总内存大小:【%.2f】GB"%(sum(size_dict.values())/(1024**3)))
```
## 简单计算器的实现
## 图像发布直方图
## 文本词频分析

```python
! pip install wordcloud
```
```py
import jieba # jieba中文分词库

with open('data/data131368/test.txt', 'r', encoding='UTF-8') as novelFile:
    novel = novelFile.read()
#print(novel)


stopwords = [line.strip() for line in open('data/data131368/stop.txt', 'r', encoding='UTF-8').readlines()]
novelList = list(jieba.lcut(novel))
novelDict = {}

# 统计出词频字典
for word in novelList:
    if word not in stopwords:
            # 不统计字数为一的词
            if len(word) == 1:
                continue
            else:
                novelDict[word] = novelDict.get(word, 0) + 1

# 对词频进行排序
novelListSorted = list(novelDict.items())
novelListSorted.sort(key=lambda e: e[1], reverse=True)

# 打印前10词频 
topWordNum = 0
for topWordTup in novelListSorted[:10]:#元组引索
    print(topWordTup)

from matplotlib import pyplot as plt
x = [c for c,v in novelListSorted]
y = [v for c,v in novelListSorted]
plt.plot(x[:10],y[:10],color='r')
plt.show()
```


# 数据爬取与分析

```py
import requests
import os
import urllib

class GetImage():
    def __init__(self,keyword='大雁',paginator=1):
        # self.url: 链接头
        self.url = 'http://image.baidu.com/search/acjson?'
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT\
             10.0; WOW64) AppleWebKit/537.36\
              (KHTML, like Gecko) Chrome/69.0.\
            3497.81 Safari/537.36'}
        self.headers_image = {
            'User-Agent': 'Mozilla/5.0 (Windows\
             NT 10.0; WOW64) AppleWebKit/537.36 \
             (KHTML, like Gecko) Chrome/69.0.\
            3497.81 Safari/537.36',
            'Referer': 'http://image.baidu.com/\
            search/index?tn=baiduimage&ipn=r&\
            ct=201326592&cl=2&lm=-1&st=-1&\
            fm=result&fr=&sf=1&fmq=1557124645631_R&\
            pv=&ic=&nc=1&z=&hd=1&latest=0&copyright\
            =0&se=1&showtab=0&fb=0&width=&height=\
            &face=0&istype=2&ie=utf-8&sid=&word=%\
            E8%83%A1%E6%AD%8C'}
        self.keyword = keyword      # 定义关键词
        self.paginator = paginator  # 定义要爬取的页数

    def get_param(self):
        # 将中文关键词转换为符合规则的编码
        keyword = urllib.parse.quote(self.keyword)
        params = []
        # 为爬取的每页链接定制参数
        for i in range(1, self.paginator + 1):
            params.append(
                'tn=resultjson_com&ipn=rj&ct=201326592&is=&\
                fp=result&queryWord={}&cl=2&lm=-1&ie=utf-8&o\
                e=utf-8&adpicid=&st=-1&z=&ic=&hd=1&latest=0&\
                copyright=0&word={}&s=&se=&tab=&width=&height\
                =&face=0&istype=2&qc=&nc=1&fr=&expermode=&for\
                ce=&cg=star&pn={}&rn=30&gsm=78&1557125391211\
                ='.format(keyword, keyword, 30 * i))
        return params   # 返回链接参数

    def get_urls(self, params):
        urls = []
        for param in params:
            # 拼接每页的链接
            urls.append(self.url + param)
        return urls   # 返回每页链接

    def get_image_url(self, urls):
        image_url = []
        for url in urls:
            json_data = requests.get(url, headers=self.headers).json()
            json_data = json_data.get('data')
            for i in json_data:
                if i:
                    image_url.append(i.get('thumbURL'))
        return image_url

    def get_image(self, image_url):
        """
        根据图片url，在本地目录下新建一个以搜索关键字命名的文件夹，然后将每一个图片存入。
        :param image_url:
        :return:
        """
        cwd = os.getcwd()
        file_name = os.path.join(cwd, self.keyword)
        if not os.path.exists(self.keyword):
            os.mkdir(file_name)
        for index, url in enumerate(image_url, start=1):
            with open(file_name+'/{}_0.jpg'.format(index), 'wb') as f:
                f.write(requests.get(url, headers=self.headers_image).content)
            if index != 0 and index % 30 == 0:
                print('第{}页下载完成'.format(index/30))

    def __call__(self, *args, **kwargs):
        params = self.get_param()  # 获取链接参数
        urls = self.get_urls(params)
        image_url = self.get_image_url(urls)
        self.get_image(image_url)

if __name__ == '__main__':
    spider = GetImage('二次元', 3)
    spider()
```
# 基于线性回归实现房价预测
# 基于逻辑回归模型实现手写数字识别