

 - 统计学
 - 基于内容（Item）：内容之间的相似度
 - 协同过滤（User）：用户之间的相似度

基于文章标签过滤
---

```
"323"	"0"	"tech tools"	"45"	"165"	"3.66666666666667"	"1"	"每个程序员必知之:程序员差别的本质"
"63"	"0"	"programmer resume latex"	"24"	"66"	"2.75"	"1"	"程序员该如何去写自己的简历(草稿）"
"207"	"0"	"beageek geek anywherehtml"	"20"	"79"	"3.95"	"1"	"be a geek 1:无处不在的html"
"38"	"0"	"iot osiot laravel ajax RESTful"	"19"	"84"	"4.42105263157895"	"1"	"一个最小的物联网系统设计方案及源码"
"361"	"0"	"write type writer programmer"	"17"	"65"	"3.82352941176471"	"1"	"编程同写作，写代码只是在码字"
"259"	"0"	"gitbook"	"12"	"51"	"4.25"	"1"	"gitbook 制作书籍"
"37"	"0"	"iot ajax laravel RESTful serial"	"11"	"40"	"3.63636363636364"	"1"	"最小物联网系统（一）——系统组成"
"391"	"0"	"javascript anonymous encapsulation"	"11"	"50"	"4.54545454545455"	"1"	"Javascript 匿名函数与封装"
"512"	"0"	""	"9"	"36"	"4.0"	"1"	"如何通过github提升自己"
"508"	"0"	"emacs vim github"	"8"	"40"	"5.0"	"1"	"努力只是因为想去做想做的事"
"548"	"0"	"full stack mustache django rework"	"8"	"35"	"4.375"	"1"	"全栈工程师的思考"
"557"	"0"	"markdown javascript refactor"	"8"	"37"	"4.625"	"1"	"前端技能训练: 重构一"
"367"	"0"	"backbone jquery underscore mustache"	"7"	"29"	"4.14285714285714"	"1"	"构建基于Javascript的移动web CMS——Hello,World"
"380"	"0"	"blog why rethink"	"7"	"28"	"4.0"	"1"	"重新思考博客的意义"
"381"	"0"	"javascript higher order"	"7"	"35"	"5.0"	"1"	"Javascript 高阶函数"
"552"	"0"	"impact github weibo zhihu"	"7"	"30"	"4.28571428571429"	"1"	"如何提高影响力"
"32"	"0"	"iot iot system jquery highchart"	"6"	"21"	"3.5"	"1"	"最小物联网系统（六）——Ajax打造可视化"
"36"	"0"	"iot iot system Hardware RESTful laravel"	"6"	"23"	"3.83333333333333"	"1"	"最小物联网系统（二）——RESTful（一）Laravel安装与数据库设置"
"49"	"0"	"rework ruby readme markdown"	"6"	"28"	"4.66666666666667"	"1"	"《REWORK》启示录 招聘笔杆子——成为笔杆子"
"568"	"0"	"github work opensource"	"6"	"26"	"4.33333333333333"	"1"	"为什么你应该深入Github"
"220"	"0"	"sqlite3 nodejs javascriptdb"	"5"	"23"	"4.6"	"1"	"node sqlite3,网站重构一，使用nodejs sqlite3查询数据"
"290"	"0"	"jasper sphinxbase pocketsphinx arecord aplay"	"5"	"18"	"3.6"	"1"	"Raspberry PI Jasper安装,Raspberry PI语音控制"
"308"	"0"	"think writeblog tech"	"5"	"18"	"3.6"	"1"	"当我写博客时，我在想什么"
"313"	"0"	"SEO ux websocket nginx"	"5"	"21"	"4.2"	"1"	"每个程序员必知之SEO"
"366"	"0"	"backbone jquery underscore mustache"	"5"	"24"	"4.8"	"1"	"构建基于Javascript的移动web CMS入门——简介"
"418"	"0"	"thoughtworks salary"	"5"	"25"	"5.0"	"1"	"ThoughtWorks 待遇与面试"
"556"	"0"	"innovation copy think rework"	"5"	"20"	"4.0"	"1"	"软件抄袭与创新的思考"
"10"	"0"	"intellij refactor thoughtworks java"	"4"	"18"	"4.5"	"1"	"ThoughtWorks 实习记——重构与Intellij Idea初探"
"29"	"0"	"iot iot system mcu serial"	"4"	"16"	"4.0"	"1"	"最小物联网系统（八）——与单片机通讯"
"304"	"0"	""	"4"	"20"	"5.0"	"1"	"持续学习"
"368"	"0"	"jquery mustache backbone underscore"	"4"	"12"	"3.0"	"1"	"构建基于Javascript的移动web CMS——模板"
"395"	"0"	"CoAP iot"	"4"	"16"	"4.0"	"1"	"CoAP与物联网系统"
"475"	"0"	"learning technology rework"	"4"	"10"	"2.5"	"1"	"技术是最简单的"
```

1. 获取文章的所有标签
2. 对所有文章的标签进行统计，计数
3. 获取文章标签中计数最多的 tag，查找相同标签的博客
4. 在剩余的博客中，选择第二多 tag，再过滤剩余的博客

```
keywords_name = model.get_keywordsfield_name()
assigned = getattr(model, keywords_name).all()
all_keywords = Keyword.objects.filter(assignments__in=assigned)
keywords = all_keywords.annotate(item_count=Count("assignments")).order_by('-item_count')

# TODO: filter most popular tag
first_keyword = keywords.first()
if first_keyword:
    first_filtered_blogposts = BlogPost.objects.published().filter(keywords__keyword__title__contains=first_keyword.title)
    first_filtered_blogposts = first_filtered_blogposts.filter(~Q(id=post_id))
    second_keyword = keywords[1]

    if second_keyword:
        blog_posts = first_filtered_blogposts.filter(keywords__keyword__title__contains=second_keyword.title)
        return blog_posts[:3]
    else:
        return []
```                

基于统计学推荐
---

 - 博客的访问量
 - 用户的点评数据

https://www.biaodianfu.com/imdb-rank.html

贝叶斯统计的算法得出的加权分(Weighted Rank-WR)，公式如下：

 WR， 加权得分（weighted rating）。
 R，该电影的用户投票的平均得分（Rating）。
 v，该电影的投票人数（votes）。
 m，排名前 250 名的电影的最低投票数（现在为 3000）。
 C， 所有电影的平均得分（现在为6.9）。

协同过滤
---


基于内容的推荐（Content-based Recommendations）
--

1. Item Representation：为每个item抽取出一些特征（也就是item的content了）来表示此item；
2. Profile Learning：利用一个用户过去喜欢（及不喜欢）的item的特征数据，来学习出此用户的喜好特征（profile）；
3. Recommendation Generation：通过比较上一步得到的用户profile与候选item的特征，为此用户推荐一组相关性最大的item。

步骤一：文章的标签

步骤二：用户行为 

步骤三：分析用户可能性

### 特征提取

分词库：[jieba](https://github.com/fxsjy/jieba) 功能：

 - 基于前缀词典实现高效的词图扫描，生成句子中汉字所有可能成词情况所构成的有向无环图 (DAG)
 - 采用了动态规划查找最大概率路径, 找出基于词频的最大切分组合
 - 对于未登录词，采用了基于汉字成词能力的 HMM 模型，使用了 Viterbi 算法

分词功能

 - 基于 TF-IDF 算法的关键词抽取
 - 基于 TextRank 算法的关键词抽取

**TextRank**

> TextRank算法可以用来从文本中提取关键词和摘要（重要的句子）

 - 关键词提取
 - 关键短语提取
 - 摘要生成

中文库：[https://github.com/letiantian/TextRank4ZH](https://github.com/letiantian/TextRank4ZH)

Content Engine: https://github.com/groveco/content-engine

User Filtering: https://github.com/fcurella/django-recommends

**LDA**

> 隐含狄利克雷分布简称LDA(Latent Dirichlet allocation)，是一种主题模型，它可以将文档集中每篇文档的主题按照概率分布的形式给出。

### 用户喜好

**最近邻方法（k-Nearest Neighbor，简称kNN）**


**Rocchio算法**

**决策树算法（Decision Tree，简称DT）**

**线性分类算法（Linear Classifer，简称LC）**

**朴素贝叶斯算法（Naive Bayes，简称NB）**

### 生成推荐



协同过滤
---


技术实现原型
---

 - hitcounts
 - rating
 
 
django + celery + redis 调度/定期/afterpost ?

ajax 生成


参考内容：

 - 《驾驭文本》
 - 《推荐系统》
 - 《机器学习实战》

文章：[http://www.cnblogs.com/breezedeus/archive/2012/04/10/2440488.html](http://www.cnblogs.com/breezedeus/archive/2012/04/10/2440488.html)

