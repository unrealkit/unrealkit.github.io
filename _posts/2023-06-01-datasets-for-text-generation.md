---
layout: post
title: Datasets for Text Generation
date: 2023-06-01 01:55 +0000
categories: [科研, 文本生成]
tags: [nlp, datasets, 调研]
---

先放几个链接：
1. https://paperswithcode.com/datasets?task=text-generation
2. https://aclweb.org/aclwiki/Data_sets_for_NLG

## GLUE

GLUE [官网](https://gluebenchmark.com/)

GLUE (General Language Understanding Evaluation benchmark) 是 9 个任务的集合，其中包括：
1. 单句任务
   - CoLA (Corpus of Linguistic Acceptability)
   - SST-2 (Stanford Sentiment Treebank)
2. 相似性
   - MRPC (Microsoft Research Paraphrase Corpus)
   - STS-B (Semantic Textual Similarity Benchmark)
   - QQP (Quora Question Pairs)
3. 释义任务
   - MNLI (Multi-Genre Natural Language Inference)
   - QNLI (Question NLI)
   - RTE (Recognizing Textual Entailment)
   - WNLI (Winograd Natural Language Inference)


接下来我们分别看一下数据集的内容。

### CoLA

CoLA 和 SST-2 都是 single-sentence tasks 的数据集。

- CoLA dataset: Training and validation sets for CoLA are available under [acceptability_corpus/raw](acceptability_corpus/raw) with a tokenized version available under [tokenized](acceptability_corpus/tokenized). Test data (unlabeled) is available here: [in domain](https://www.kaggle.com/c/cola-in-domain-open-evaluation) [out of domain](https://www.kaggle.com/c/cola-out-of-domain-open-evaluation). All models require tokenized data (we use the default NLTK tokenizer).
- CoLA [baseline](https://github.com/nyu-mll/CoLA-baselines): 可通过这个仓库下载所有的（9 个）数据集

是单句子分类任务，语料来自语言理论的书籍和期刊，每个句子被标注为是否合乎语法的单词序列。本任务是一个二分类任务，标签共两个，分别是 0 和 1，其中 0 表示不合乎语法，1 表示合乎语法。

```
gj04	1		Our friends won't buy this analysis, let alone the next one we propose.
gj04	1		One more pseudo generalization and I'm giving up.
gj04	1		One more pseudo generalization or I'm giving up.
gj04	1		The more we study verbs, the crazier they get.
gj04	1		Day by day the facts are getting murkier.
gj04	1		I'll fix you a drink.
gj04	1		Fred watered the plants flat.
gj04	1		Bill coughed his way out of the restaurant.
gj04	1		We're dancing the night away.
gj04	1		Herman hammered the metal flat.
gj04	1		The critics laughed the play off the stage.
gj04	1		The pond froze solid.
gj04	1		Bill rolled out of the room.
gj04	1		The gardener watered the flowers flat.
gj04	1		The gardener watered the flowers.
gj04	1		Bill broke the bathtub into pieces.
gj04	1		Bill broke the bathtub.
gj04	1		They drank the pub dry.
gj04	0	*	They drank the pub.
gj04	1		The professor talked us into a stupor.
gj04	0	*	The professor talked us.
gj04	1		We yelled ourselves hoarse.
gj04	0	*	We yelled ourselves.
gj04	0	*	We yelled Harry hoarse.
gj04	1		Harry coughed himself into a fit.
gj04	0	*	Harry coughed himself.
gj04	0	*	Harry coughed us into a fit.
gj04	1		Bill followed the road into the forest.
gj04	1		We drove Highway 5 from SD to SF.
```

样本个数：train (8551)，dev (1043)，test (1063)。

任务：可接受程度，合乎语法与不合乎语法二分类。

评价准则：Matthews correlation coefficient。

标签为 1（合乎语法）的样例：
```
She is proud.
she is the mother.
John thinks Mary left.
Yes, she did.
Will John not go to school?
Mary noticed John’s excessive appreciation of himself.
```
标签为 0（不合语法）的样例：
```
Mary sent.
Yes, she used.
Mary wonders for Bill to come.
They are intense of Bill.
Mary thinks whether Bill will come.
Mary noticed John’s excessive appreciation of herself.
```
### SST-2

数据集内容：

```
sentence	label
it 's a charming and often affecting journey . 	1
unflinchingly bleak and desperate 	0
allows us to hope that nolan is poised to embark a major career as a commercial yet inventive filmmaker . 	1
the acting , costumes , music , cinematography and sound are all astounding given the production 's austere locales . 	1
it 's slow -- very , very slow . 	0
although laced with humor and a few fanciful touches , the film is a refreshingly serious look at young women . 	1
a sometimes tedious film . 	0
or doing last year 's taxes with your ex-wife . 	0
you do n't have to know about music to appreciate the film 's easygoing blend of comedy and romance . 	1
```

单句子分类任务，包含电影评论中的句子和它们情感极性。本任务也是一个二分类任务，是句子级别的情感极性分类，分为正面和负面情感。

样本个数：train (67350)，dev (873)，test (1821)。

任务：情感分类，正面情感和负面情感二分类。

评价准则：accuracy。

标签为 1（正面情感，positive）的样例：
```
two central performances
against shimmering cinematography that lends the setting the ethereal beauty of an asian landscape painting
the situation in a well-balanced fashion
a better movie
at achieving the modest , crowd-pleasing goals it sets for itself
a patient viewer
```
标签为 0（负面情感，negative）的样例：
```
a transparently hypocritical work that feels as though it 's trying to set the women 's liberation movement back 20 years
so pat it makes your teeth hurt
blood work is laughable in the solemnity with which it tries to pump life into overworked elements from eastwood 's dirty harry period .
faced with the possibility that her life is meaningless , vapid and devoid of substance , in a movie that is definitely meaningless , vapid and devoid of substance
monotone
this new jangle of noise , mayhem and stupidity must be a serious contender for the title
```

### MRPC

数据集内容：

```
Quality	#1 ID	#2 ID	#1 String	#2 String
1	702876	702977	Amrozi accused his brother , whom he called " the witness " , of deliberately distorting his evidence .	Referring to him as only " the witness " , Amrozi accused his brother of deliberately distorting his evidence .
0	2108705	2108831	Yucaipa owned Dominick 's before selling the chain to Safeway in 1998 for $ 2.5 billion .	Yucaipa bought Dominick 's in 1995 for $ 693 million and sold it to Safeway for $ 1.8 billion in 1998 .
1	1330381	1330521	They had published an advertisement on the Internet on June 10 , offering the cargo for sale , he added .	On June 10 , the ship 's owners had published an advertisement on the Internet , offering the explosives for sale .
0	3344667	3344648	Around 0335 GMT , Tab shares were up 19 cents , or 4.4 % , at A $ 4.56 , having earlier set a record high of A $ 4.57 .	Tab shares jumped 20 cents , or 4.6 % , to set a record closing high at A $ 4.57 .
1	1236820	1236712	The stock rose $ 2.11 , or about 11 percent , to close Friday at $ 21.51 on the New York Stock Exchange .	PG & E Corp. shares jumped $ 1.63 or 8 percent to $ 21.03 on the New York Stock Exchange on Friday .
```

### STS-B

数据集内容：

```
index	genre	filename	year	old_index	source1	source2	sentence1	sentence2	score
0	main-captions	MSRvid	2012test	0000	none	none	A man with a hard hat is dancing.	A man wearing a hard hat is dancing.	5.000
1	main-captions	MSRvid	2012test	0002	none	none	A young child is riding a horse.	A child is riding a horse.	4.750
2	main-captions	MSRvid	2012test	0003	none	none	A man is feeding a mouse to a snake.	The man is feeding a mouse to the snake.	5.000
3	main-captions	MSRvid	2012test	0007	none	none	A woman is playing the guitar.	A man is playing guitar.	2.400
4	main-captions	MSRvid	2012test	0008	none	none	A woman is playing the flute.	A man is playing a flute.	2.750
5	main-captions	MSRvid	2012test	0010	none	none	A woman is cutting an onion.	A man is cutting onions.	2.615
```

### QQP

数据集内容：

```
id	qid1	qid2	question1	question2	is_duplicate
201359	303345	303346	Why are African-Americans so beautiful?	Why are hispanics so beautiful?	0
263843	69383	380476	I want to pursue PhD in Computer Science about social network,what is the open problem in social networks?	I handle social media for a non-profit. Should I start going to social media networking events? Are there any good ones in the bay area?	0
172974	266948	175089	Is there a reason why we should travel alone?	What are some reasons to travel alone?	1
15329	29298	29299	Why are people so obsessed with having a girlfriend/boyfriend?	How can a single male have a child?	0
209794	314169	314170	What are some good baby girl names starting with D?	What are some good baby girl names starting with D or H?	0
```

### MNLI

数据集内容：

```
index	promptID	pairID	genre	sentence1_binary_parse	sentence2_binary_parse	sentence1_parse	sentence2_parse	sentence1	sentence2	label1	label2	label3	label4	label5	gold_label
0	63735	63735n	slate	( ( The ( new rights ) ) ( are ( nice enough ) ) )	( Everyone ( really ( likes ( the ( newest benefits ) ) ) ) )	(ROOT (S (NP (DT The) (JJ new) (NNS rights)) (VP (VBP are) (ADJP (JJ nice) (RB enough)))))	(ROOT (S (NP (NN Everyone)) (VP (ADVP (RB really)) (VBZ likes) (NP (DT the) (JJS newest) (NNS benefits)))))	The new rights are nice enough	Everyone really likes the newest benefits 	neutral	entailment	neutral	neutral	neutral	neutral
```

## [Data-to-Text Generation](https://aclweb.org/aclwiki/Data_sets_for_NLG)

These datasets contain data and corresponding texts based on this data.

