# dump_fc_keyword_dispose
用于做垃圾站，自动选取上线聚合页面的关键词的程序，满足：
+ 1、有对应内容，且内容数量大于指定值的词，生成聚合页
+ 2、有对应内容，但内容数量很少的词，选取与该词相似性最高的详情页，修改title来覆盖该词

具体流程为：

第一步：凤巢关键词，排除包含空格，并保留searches>5且包含词根的关键词，输出到制定csv文件。凤巢关键词扩展处理视情况处理下，去除与词根完全没关联的词

第二步：连接mysql，抽取频道detail内容，已上线的聚合页面的关键词列表。另一方面，通过xunsearch给这些detail创建索引，指定全文索引接口，用于第4步

第三步：将凤巢关键词与上线关键词进行对比，完整匹配的存入pipei_word.txt待去重，未匹配存入nopipei_word.txt待分词判断相关性来挑选合适的着陆页页面

第四步：对完整匹配的关键词pipei_word.py进行去重，保留字数最多的那一个

第五步：剩余未完整匹配上的凤巢搜索词跑下站内搜索，提取并计算‘搜索结果数’，‘整词召回数’，‘主词召回数’，根据以上指标判定该词是否生成专题页还是匹配相似详情页title

index.py ：主程序，按指定顺序依次引用以上程序
