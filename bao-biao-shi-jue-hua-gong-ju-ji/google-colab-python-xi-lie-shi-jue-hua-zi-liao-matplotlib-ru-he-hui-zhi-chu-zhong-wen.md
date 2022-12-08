---
description: 小技巧大受用
---

# 🖋 【Google Colab Python系列】 視覺化資料Matplotlib 如何繪製出中文？

<figure><img src="../.gitbook/assets/matplotlib中文化.drawio.png" alt=""><figcaption></figcaption></figure>

我們前篇有介紹到如何讓資料視覺化「📈【Google Colab Python系列】以Goodinfo為例，將資料視覺化吧！」，過程中雖然我們的報表呈現皆使用英文字眼，但假若欲繪製中文進行呈現時就會發生以下狀況：

```python
import matplotlib.pyplot as plt

data = [lYield, cYield, hYield]

# 這裡繪製出中文
labels = ['中文']

plt.boxplot(data, labels=labels)
plt.show()
```

```
/usr/local/lib/python3.8/dist-packages/matplotlib/backends/backend_agg.py:214: RuntimeWarning: Glyph 20013 missing from current font.
  font.set_text(s, 0.0, flags=flags)
/usr/local/lib/python3.8/dist-packages/matplotlib/backends/backend_agg.py:214: RuntimeWarning: Glyph 25991 missing from current font.
  font.set_text(s, 0.0, flags=flags)
/usr/local/lib/python3.8/dist-packages/matplotlib/backends/backend_agg.py:183: RuntimeWarning: Glyph 20013 missing from current font.
  font.set_text(s, 0, flags=flags)
/usr/local/lib/python3.8/dist-packages/matplotlib/backends/backend_agg.py:183: RuntimeWarning: Glyph 25991 missing from current font.
  font.set_text(s, 0, flags=flags)
```

由上述的訊息可以推估可能是某些字型缺失，因此無法正常顯現，這時候我們就需要下載字型並補足，以下會逐步說明步驟。



### 下載字型並存放至目錄

字型的下載可以到「[思源宋體](https://github.com/adobe-fonts/source-han-serif)」抓取唷！裡面涵蓋了中、日、韓的字型，那這邊的範例我們就隨便選一個中文字型來進行示範。

這邊我們會下載繁體中文的ttf檔，並將該檔掛入matplotlib的字型資料庫中，以便進行中文的繪製。

```python
import matplotlib as mpl
import matplotlib.font_manager as fm
import matplotlib.pyplot as plt

# 下載繁體中文字型
!wget -O SourceHanSerifTW-VF.ttf https://github.com/adobe-fonts/source-han-serif/raw/release/Variable/TTF/Subset/SourceHanSerifTW-VF.ttf

# 加入字型檔
fm.fontManager.addfont('SourceHanSerifTW-VF.ttf')

# 設定字型
mpl.rc('font', family='SourceHanSerifTWVF-Regular')
```

至於要設定哪種family請參閱「[official font readme file](https://github.com/adobe-fonts/source-han-serif/raw/release/SourceHanSerifReadMe.pdf).」。

### 加掛字型

### 資源參考

* [https://ithelp.ithome.com.tw/articles/10202385](https://ithelp.ithome.com.tw/articles/10202385)
* [https://sujingjhong.com/posts/how-to-show-matplotlib-visual-packages-in-chinese-on-colab/](https://sujingjhong.com/posts/how-to-show-matplotlib-visual-packages-in-chinese-on-colab/)
