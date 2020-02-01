---
date: 2020-02-01 16:15:01
categories:
   - 机器学习
tags:
   - 吴恩达视频笔记
---
# 机器学习的定义
Machine Learning，利用统计学技术，通过对数据的不断学习，获得其中的经验。

<!--more-->

# 一.监督学习
## 1.定义
每一组用于学习的数据都有对应的输出值（正确的值）。</br>
输出值可以是连续的值，也可以是离散的值。</br>
根据输出值的不同，监督学习分为回归问题和分类问题。
## 2.监督学习例子
1）房价的预测：面积与价格的关系（回归）。</br>
2）肿瘤的判断：肿瘤类型与大小的判断（分类）。
# 二.无监督学习
## 1.定义
根据给定的数据，通过学习将数据分成多类。</br>
将数据集分成不同簇的无监督学习也被称为聚类算法。
## 2.无监督学习例子
1) 谷歌新闻的分类：将相似内容的新闻链接放在一起成为新闻专题。
2) 根据基因分类：根据基因的相似性将人分群。
3) 鸡尾酒派对：将混合的声音分离开来。
4) 机器协同工作、社交网络分析、客户市场分析（针对性服务）、天文数据分析

## 无监督学习之dbscan笔记
### 算法原理：
（1）DBSCAN通过检查数据集中每点的Eps邻域来搜索簇，如果点p的Eps邻域包含的点多于MinPts个，则创建一个以p为核心对象的簇；

（2）然后，DBSCAN迭代地聚集从这些核心对象直接密度可达的对象，这个过程可能涉及一些密度可达簇的合并；

（3）当没有新的点添加到任何簇时，该过程结束。

### 伪代码
```python
输入：数据集D，给定点在邻域内成为核心对象的最小邻域点数：MinPts,邻域半径：Eps   
输出：簇集合

(1) 首先将数据集D中的所有对象标记为未处理状态
(2) for（数据集D中每个对象p） do
(3)    if （p已经归入某个簇或标记为噪声） then
(4)         continue;
(5)    else
(6)         检查对象p的Eps邻域 NEps(p) ；
(7)         if (NEps(p)包含的对象数小于MinPts) then
(8)                  标记对象p为边界点或噪声点；
(9)         else
(10)                 标记对象p为核心点，并建立新簇C, 并将p邻域内所有点加入C
(11)                 for (NEps(p)中所有尚未被处理的对象q)  do
(12)                       检查其Eps邻域NEps(q)，若NEps(q)包含至少MinPts个对象，则将NEps(q)中未归入任何一个簇的对象加入C；
(13)                 end for
(14)        end if
(15)    end if
(16) end for
```
### 实际实现代码
数据下载:[788points.txt](https://pan.baidu.com/s/17-55Q3p1dWwtX71jamxBuQ)

```
import numpy as np
import matplotlib.pyplot as plt
import math
import time

UNCLASSIFIED = False
NOISE = 0

def loadDataSet(fileName, splitChar='\t'):
    """
    输入：文件名
    输出：数据集
    描述：从文件读入数据集
    """
    dataSet = []
    with open(fileName) as fr:
        for line in fr.readlines():
            curline = line.strip().split(splitChar)
            fltline = list(map(float, curline))
            dataSet.append(fltline)
    return dataSet

def dist(a, b):
    """
    输入：向量A, 向量B
    输出：两个向量的欧式距离
    """
    return math.sqrt(np.power(a - b, 2).sum())

def eps_neighbor(a, b, eps):
    """
    输入：向量A, 向量B
    输出：是否在eps范围内
    """
    return dist(a, b) < eps

def region_query(data, pointId, eps):
    """
    输入：数据集, 查询点id, 半径大小
    输出：在eps范围内的点的id
    """
    nPoints = data.shape[1]
    seeds = []
    for i in range(nPoints):
        if eps_neighbor(data[:, pointId], data[:, i], eps):
            seeds.append(i)
    return seeds

def expand_cluster(data, clusterResult, pointId, clusterId, eps, minPts):
    """
    输入：数据集, 分类结果, 待分类点id, 簇id, 半径大小, 最小点个数
    输出：能否成功分类
    """
    seeds = region_query(data, pointId, eps)  # 查找 pointId 半径内符合要求的点
    if len(seeds) < minPts:  # 个数过少，不满足minPts条件，设置为噪声点
        clusterResult[pointId] = NOISE
        return False
    else:
        clusterResult[pointId] = clusterId  # 将pointId和半径内的点划分到该簇
        for seedId in seeds:
            clusterResult[seedId] = clusterId

        while len(seeds) > 0:  # 遍历seeds中的点，以每一个点为中心，扩大范围
            currentPoint = seeds[0]
            queryResults = region_query(data, currentPoint, eps)
            if len(queryResults) >= minPts:
                for i in range(len(queryResults)):
                    resultPoint = queryResults[i]
                    if clusterResult[resultPoint] == UNCLASSIFIED:
                        seeds.append(resultPoint)   # 该点没有被分类过，加到该簇中
                        clusterResult[resultPoint] = clusterId
                    elif clusterResult[resultPoint] == NOISE:   # 曾被设置为噪声的点
                        clusterResult[resultPoint] = clusterId
            seeds = seeds[1:]   # 约等于移除第一个数，实现遍历操作
        return True

def dbscan(data, eps, minPts):
    """
    输入：数据集, 半径大小, 最小点个数
    输出：分类簇id
    """
    clusterId = 1
    nPoints = data.shape[1]  # 数据个数
    clusterResult = [UNCLASSIFIED] * nPoints  # 初始状态全部设置为未处理
    for pointId in range(nPoints):
        point = data[:, pointId]
        if clusterResult[pointId] == UNCLASSIFIED:  # 存在没有被处理过的点
            if expand_cluster(data, clusterResult, pointId, clusterId, eps, minPts):   # 成功分类，增加一个簇,继续对未处理的点进行分类
                clusterId = clusterId + 1
    return clusterResult, clusterId - 1

def plotFeature(data, clusters, clusterNum):
    nPoints = data.shape[1]
    matClusters = np.mat(clusters).transpose()
    fig = plt.figure()
    scatterColors = ['black', 'blue', 'green', 'yellow', 'red', 'purple', 'orange', 'brown']
    ax = fig.add_subplot(111)
    for i in range(clusterNum + 1):
        colorSytle = scatterColors[i % len(scatterColors)]
        subCluster = data[:, np.nonzero(matClusters[:, 0].A == i)]
        ax.scatter(subCluster[0, :].flatten().A[0], subCluster[1, :].flatten().A[0], c=colorSytle, s=50)

def main():
    dataSet = loadDataSet('788points.txt', splitChar=',')
    dataSet = np.mat(dataSet).transpose()
    # print(dataSet)
    clusters, clusterNum = dbscan(dataSet, 2, 15)  # 半径为2 最少点个数15
    print("cluster Numbers = ", clusterNum)
    # print(clusters)
    plotFeature(dataSet, clusters, clusterNum)

if __name__ == '__main__':
    print('start! please wait...')
    start = time.clock()
    main()
    end = time.clock()
    print('finish all in %s' % str(end - start))
    plt.show()
```