---
description: AI原來離我們那麼近...
---

# 【Hugging Face】Ep.1 平凡人也能玩的起的AI平台

<figure><img src="../.gitbook/assets/下載 (3).jpg" alt=""><figcaption><p><a href="https://vocus.cc/article/649d7961fd89780001b63b0a">圖片來源</a></p></figcaption></figure>

### 它到底是什麼？

簡單說Hugging Face是人工智慧開源平台，開發者發表和共享預訓練模型、資料庫和示範檔案等。Hugging Face共享超過10萬個預訓練模型，上萬資料庫，包括微軟、Google、彭博、英特爾等各行業上萬機構都有使用Hugging Face。

### 理念與目標

我們都知道最近火紅的OpenAI公開的ChatGPT非常熱門， 但在開發者服務方面， OpenAI正在搭起人工智慧的圍牆， 僅允許滿足條件的企業或個人進入， 而「[Hugging Face](https://huggingface.co/docs)」希望每個人都能做出生成式AI的模型， 有點像是[Github](https://github.com/)的概念， 讓整個資訊科技可以快速推進。

巨頭努力築起AI的圍牆的戰場之下， 「[Hugging Face](https://huggingface.co/docs)」有點像強力的民兵， 擁抱開放， 讓一般的平民老百姓也有機會接觸到高深的AI技術， 不再讓這些尖端技術掌握在巨頭手中， 因此也吸引了不少的擁護者， 其實一開始的「[Hugging Face](https://huggingface.co/docs)」是針對年輕人開發的聊天機器人， 且技術基於NLP(自然語言處理)， Transformer模型的出現瞬間成為自然語言領域最受開發者關注的模型，也讓[Hugging Face](https://huggingface.co/docs)一炮而紅。

### 商業模式

「[Hugging Face](https://huggingface.co/docs)」以賦能為出發點讓整個AI社群發揚光大， 因此只要在其中獲得1％的變現就能夠撐起一間公司， 類似於[elastic](https://www.elastic.co/?ultron=B-Stack-Trials-APJ-Exact\&gambit=Stack-Core\&blade=adwords-s\&hulk=paid\&Device=c\&thor=elasticsearch\&gclid=Cj0KCQjwtO-kBhDIARIsAL6LorcEhwruQCuEpuXsKJv0SaTVff-tTBBMSJc6bKQF8NlnadNjzHpc0XMaAnlHEALw\_wcB)、[mongodb](https://www.mongodb.com/)…等。

### **Hugging Face Hub**

相信只要是開發者都知道Github是一個儲存程式碼的倉庫， 但AI模型呢？ 總要有個地方集中控管吧！

如果有興趣的朋友請至這裡參考參考: [https://huggingface.co/docs/hub/index](https://huggingface.co/docs/hub/index)

<figure><img src="../.gitbook/assets/hub (1).png" alt=""><figcaption><p><a href="https://www.potatomedia.co/s/aHvvQP3M">圖片來源</a></p></figcaption></figure>

我們可以發現到除了模型以外， 資料集、靜態網頁空間、報告空間..， 非常的豐富， 讓我們可以將開發好的專案完整的放置Hub之上， 透過學習交流的方式加快人工智慧的進程。

### 組成的元件

使用[Hugging Face](https://huggingface.co/docs)務必要了解最重要的三大元件， 基本上各種任務的模型(語音辨識、影像分類、NLP…)， 都是離不開這三大元件的，

#### **Transformers**

顧名思義就是為了處理各種Transformer模型而開發的元件，

#### **Tokenizers**

我們都知道NLP的世界裡， 文字的最小的單位就是詞， 而要將文字化成詞的關鍵就是斷詞， Tokenizers就是扮演著這個角色， 提供了不同的策略也支援前處理、後處理。

#### Datasets

我們都知道AI訓練的重要養分來源就是資料集， 而Datasets元件就是扮演著如何將資料集管理好的角色， 並提供豐富的API(隨機分類、切割、整合pandas)， 讓我們更容易的處理資料。

#### 更多其他的元件

最佳化、加速器的Accelerate、Optimum， 甚至是無代碼工具的AutoTrain…， 都是[Hugging Face](https://huggingface.co/docs)涵蓋的強大功能。

### 簡易的使用方式讓我們輕鬆上手

這邊我們就使用wav2vec2的語音辨識模型試玩看看吧！

使用起來非常簡單, 我們只要使用pipeline搭配指定的任務， 就能進行簡單的AI任務， 以這裡的範例為例， 我們會使用「automatic-speech-recognition」語音辨識的任務來進行。

並指定「ydshieh/wav2vec2-large-xlsr-53-chinese-zh-cn-gpt」這個中文模型進行語音辨識。

最後直接將音檔進行辨識，產生文字， 整個操作流程非常簡易。

```python
import torch

# 引入pipeline
from transformers import pipeline

# 定義任務
asr = pipeline(
    "automatic-speech-recognition", 
    model='ydshieh/wav2vec2-large-xlsr-53-chinese-zh-cn-gpt', 
)

# 執行任務
result = asr('./test.wav')

text = result['text']

text
```

更多的使用方式請參考「[https://huggingface.co/docs/transformers/quicktour」。](https://huggingface.co/docs/transformers/quicktour%E3%80%8D%E3%80%82)

喜歡撰寫文章的你，不妨來了解一下：

[Web3.0時代下為創作者、閱讀者打造的專屬共贏平台 - 為什麼要加入？](https://www.potatomedia.co/s/2PmFxsq)

歡迎加入一起練習寫作，賺取知識！
