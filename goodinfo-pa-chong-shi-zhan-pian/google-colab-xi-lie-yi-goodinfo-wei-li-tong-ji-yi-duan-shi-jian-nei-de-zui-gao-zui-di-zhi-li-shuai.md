---
description: 一起動動手來玩玩Python吧
---

# 【Google Colab系列】以Goodinfo!為例，統計一段時間內的最高、最低殖利率

上一篇我們有介紹Google Colab Python的爬蟲基礎概念與技巧，還沒閱讀的朋友可以先進行閱讀，並建立基礎概念之後再接著進行實戰演練會比較容易上手唷！ 這裡就附上連結「[【Google Colab系列】該如何設計自己的爬蟲來抓取Html資料？](google-colab-xi-lie-gai-ru-he-she-ji-zi-ji-de-pa-chong-lai-zhua-qu-html-zi-liao.md)」供各位參考囉！



上一次我們已經示範如何抓取目標表格，這次的主軸會圍繞在如何切換表格內容並抓取某幾個cell的資料進行程式運算，因此這個篇章我們會學到以下幾個重要技能：

* 抓取某段範圍內的cell內容。
* 計算與統計。



### 分析切換「顯示依據」的行為

我們先打開F12並切到Network來觀察操作行為過程中會發送哪些網路封包。

<figure><img src="../.gitbook/assets/股利發放年度_封包觀察.png" alt=""><figcaption></figcaption></figure>

這時候就得考驗我們的觀察能力了，有沒有發現圖片上請求的URL已經有點不太一樣了，我們拿這段URL去試著請求看看，果不其然，刷新後的頁面就是股利發放政策的相關資訊了。

<figure><img src="../.gitbook/assets/股利發放年度頁面.png" alt=""><figcaption></figcaption></figure>

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

### 開始統計的精華與心法



### 結語

過程中其實都在訓練我們的觀察能力，如何察覺到某些地方的微小差異就能夠透過這樣的差異抓到一些規則，藉由這些規則進行排除法，一步步的獲取我們重要的資訊，



今天的範例都在這裡「[📦 **goodinfo\_yield.ipynb**](../jupyter-examples/goodinfo\_yield.ipynb)」歡迎自行取用。

\------------------------------------------------------------------------------------------------

喜歡撰寫文章的你，不妨來了解一下：

[Web3.0時代下為創作者、閱讀者打造的專屬共贏平台 - 為什麼要加入？](https://www.potatomedia.co/s/2PmFxsq)&#x20;

歡迎加入一起練習寫作，賺取知識，累積財富！
