---
description: 一起動動手來玩玩Python吧
---

# 【Google Colab系列】以Goodinfo!為例，統計一段時間內的最高、最低殖利率

上一篇我們有介紹Google Colab Python的爬蟲基礎概念與技巧，還沒閱讀的朋友可以先進行閱讀，並建立基礎概念之後再接著進行實戰演練會比較容易上手唷！ 這裡就附上連結「[【Google Colab系列】該如何設計自己的爬蟲來抓取Html資料？](google-colab-xi-lie-gai-ru-he-she-ji-zi-ji-de-pa-chong-lai-zhua-qu-html-zi-liao.md)」供各位參考囉！



這一篇章會以捕魚的四大步驟進行比喻，由淺入深，逐步完成屬於自己的統計程式，目標是能夠以生活化例子建立撰寫爬蟲的基礎概念，未來假若我們需要進一步蒐集資料進行統計分析時，就將這套心法搬出來舞弄，相信概念與技巧熟練之後，遇到任何奇耙的網站資料也都能夠迎刃而解。



上一次我們已經示範如何抓取目標表格，這次的主軸會圍繞在如何切換表格內容並抓取某幾個cell的資料進行程式運算，因此這個篇章我們會學到以下幾個重要技能：

* 抓取某段範圍內的cell內容。
* 過濾標題。
* 使用pandas進行計算與統計。

一些基本的API筆記如下：

```
# 顯示有哪些欄位
node.columns

# 顯示Columns(列)為名稱的數據
node[['名稱']] 
```

### 觀察： 分析切換「顯示依據」的行為

我們先打開F12並切到Network來觀察操作行為過程中會發送哪些網路封包。

<figure><img src="../.gitbook/assets/股利發放年度_封包觀察.png" alt=""><figcaption></figcaption></figure>

這時候就得考驗我們的觀察能力了，有沒有發現圖片上請求的URL已經有點不太一樣了，我們拿這段URL去試著請求看看，果不其然，刷新後的頁面就是股利發放政策的相關資訊了。



```
https://goodinfo.tw/tw/StockDividendPolicy.asp?STOCK_ID=xxx
```

<figure><img src="../.gitbook/assets/股利發放年度頁面.png" alt=""><figcaption></figcaption></figure>

### 撒網： 定義明確目標

首先我們成功抓到表格之後，下一步就是定義我們今天要統計的數值，以這次的目標為例，統計的為N段時間內的最高、最低殖利率。

因此我們需要抓取的元素有「股利發放年度」、「現金股利」、「年度股價」這些資訊輔助我們進行每一年殖利率的計算。



至於殖利率怎麼計算請參考「...」。

### 收網： 撰寫程式進行抓取重要區塊

這時候我們就可以大膽的將這串URL寫到程式碼中，進一步進行抓取。

```
import requests

STOCK_ID='3231'

headers = {
  'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36'
}
res = requests.get('https://goodinfo.tw/tw/StockDividendPolicy.asp?STOCK_ID={0}'.format(STOCK_ID), headers = headers)
res.encoding = 'utf-8'
res.text
```

再來我們一樣藉由F12(Network)來錨定關注範圍的表格。

<figure><img src="../.gitbook/assets/限縮表格範圍.png" alt=""><figcaption></figcaption></figure>

接著我們運用「BeautifulSoup」來抓取該表格的標籤。

```
from bs4 import BeautifulSoup
bs = BeautifulSoup(res.text, 'html.parser')
data = bs.select_one('#tblDetail')
print(data)
```

再來使用pandas套件來檢驗我們抓取的表格是否正確。

```
import pandas
dfs = pandas.read_html(data.prettify())
print(dfs)
```

<figure><img src="../.gitbook/assets/股利資訊.png" alt=""><figcaption></figcaption></figure>

看樣子是我們所需要的資訊沒有錯，接下來就要開始抓取最高與最低股利來計算這兩者的平均殖利率囉！

### 料理：統計的精華與心法

首先我們一樣用取節點的技巧，並印出前N筆的方式來縮減範圍，並且框出我們需要的欄位。

```python
import pandas
import datetime



dfs = pandas.read_html(data.prettify())
node = dfs[0]

node.head(5)
```

<figure><img src="../.gitbook/assets/抓取殖利率.png" alt=""><figcaption></figcaption></figure>

這邊有幾篇關於Pandas的教學，建議可以先行閱讀，才比較容易知道如何使用這個強大的套件。

* [https://www.learncodewithmike.com/2020/11/python-pandas-dataframe-tutorial.html](https://www.learncodewithmike.com/2020/11/python-pandas-dataframe-tutorial.html)

接著我們直接進入重點，我們先試試看抓取重要的幾個欄位並留下即可，並且我們試著將columns印出，記錄我們需要的欄位名稱，下一步我們會使用這些資訊留下重要的欄位資訊。

```python
# 這邊我們將標題僅留下第3列即可
node.columns = node.columns.get_level_values(3)

# 印出標題名稱
node.columns
```

<figure><img src="../.gitbook/assets/標題名稱.png" alt=""><figcaption></figcaption></figure>



### 結語

過程中其實都在訓練我們的觀察能力，如何察覺到某些地方的微小差異就能夠透過這樣的差異抓到一些規則，藉由這些規則進行排除法，一步步的獲取我們重要的資訊，



今天的範例都在這裡「[📦 **goodinfo\_yield.ipynb**](../jupyter-examples/goodinfo\_yield.ipynb)」歡迎自行取用。

\------------------------------------------------------------------------------------------------

喜歡撰寫文章的你，不妨來了解一下：

[Web3.0時代下為創作者、閱讀者打造的專屬共贏平台 - 為什麼要加入？](https://www.potatomedia.co/s/2PmFxsq)&#x20;

歡迎加入一起練習寫作，賺取知識，累積財富！
