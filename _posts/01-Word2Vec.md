# Word2vec -->为了的到词向量


![](/img/01-1.png)

## CBOW

给出一个词的<mark>上下文</mark>，得到这个词
“我是最`_`的yuan”
帅    $w_t$

## Skip-gram

给出一个词，得到这个词的上下文
“帅”
“我是`_`的yuan”

## NNLM和Word2Vec的区别

NNLM ==> 重点是预测下一个词
Word2Vec ==> CBOW和Skip-gram重点是得到一个Q矩阵

## Word2Vec的缺点

00010代表apple × Q = 10， 12， 19

apple（苹果， 苹果手机）

假设数据集里面的apple只有苹果这个意思， 没有手机的意思（训练）

词向量不能多意 ==> ELMO
