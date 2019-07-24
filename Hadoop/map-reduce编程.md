#Hadoop
##map-reduce编程
###代码层面

```
from mrjob.job import MRJob
class WordCounter(MRJob):
#'-'部分表示这是文本的第几行，不需要该内容所以不接收
    def mapper(self,_,line):
    #每次传入一行的内容，并根据空格切分成一个word list，使得每次运行返回一个word对应coun为1
        for word in line.split:
            yield word,1

    def reducer(self,word,count):
        yield word(sum(count))
if __name__=='__main__':
    WordCounter().run()

```

###框架角度

![   ](https://github.com/woshiellen117/Basic-Knowledge/raw/master/Hadoop/pic/1.png)

* 1、2、3都是HDFS文件形式存储的结果，存在一个hash映射把不同的单词分别映射到红、绿、蓝三块，每个单词都在同一个reduce处处理。
* shuffle过程，Hadoop使用物理存储，Spark使用内存，所以Spark快更多。
* 缺陷
  * 不支持流水线作业 ，需要所有数据map一系列操作完生成HDFS文件再放到reduce中
  * 不支持DAG计算（Directed Acyclic Graph，将计算任务在内部分解成为若干个子任务，将这些子任务之间的逻辑关系或顺序构建成DAG（有向无环图）结构），即该模式多个应用程序存在依赖关系，后一个应用程序的输入为前一个的输出。