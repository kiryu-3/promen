# 統計　第9回

``` py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
!pip install japanize-matplotlib
import japanize_matplotlib
import seaborn as sns
from scipy import stats
from scipy.stats import t
from scipy.stats import norm
from decimal import Decimal, ROUND_HALF_UP, ROUND_HALF_EVEN    # 小数の四捨五入を行うためにimport
```

## 確認のためのQ&A・7.1

``` py
# dictionaryからDataFrameを作る

data = {
    "X_i":[168,141,157,151,160,155,148,160]
}

df = pd.DataFrame(data)
df
```
	X_i	(X_i-μ)^2
0	168	169
1	141	196
2	157	4
3	151	16
4	160	
| TH 左寄せ | TH 中央寄せ | TH 右寄せ |
| :---: | :---: | :---: |
| TD | TD | TD |
| TD | TD | TD |

μ=155
print("母平均は{}です".format(μ))

σ=14
print("母標準偏差は{}です".format(σ))

df["(X_i-μ)^2"]=df["X_i"].apply(lambda X_i : (X_i-μ)**2)
df

sum(df["(X_i-μ)^2"])/σ**2

## 確認のためのQ&A・7.3

n=12
print("標本の大きさは{}です".format(n))

s=8.6
print("標本標準偏差は{}です".format(s))

x1,x2= stats.chi2.interval(alpha=0.95, df=n-1)    #下側2.5%点,上側2.5%点
x1 = Decimal(str(x1)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("下側2.5%点は{}です".format(x1))
print("上側2.5%点は{}です".format(x2))

up = np.sqrt((n-1)*s**2/float(x1))
bottom = np.sqrt((n-1)*s**2/float(x2))

up = Decimal(str(up)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
bottom = Decimal(str(bottom)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入

print("{}≦σ≦{}".format(bottom,up))
print("信頼係数95%")

## 確認のためのQ&A・7.4

# dictionaryからDataFrameを作る

data = {
    "LED電球の寿命（日）":[2752,2563,2689,2405,2570,2477,2523,2549,2601,2594,2643,2590,2721,2683,2477]
}

df = pd.DataFrame(data)
df

# 平均を求める
print(df["LED電球の寿命（日）"].mean())

# 不偏分散を求める
np.var(df["LED電球の寿命（日）"],ddof=1)

# 普遍分散の平方根を求める（不偏標準偏差とはいいません）
print(np.sqrt(stats.tvar(df["LED電球の寿命（日）"])))

x = df["LED電球の寿命（日）"].mean()
s = np.sqrt(stats.tvar(df["LED電球の寿命（日）"]))

# 母平均μの推定

x1,x2 = stats.t.interval(alpha=0.95,loc=x,scale=s/np.sqrt(len(df)),df=len(df)-1)    # 下方信頼限界 , 上方信頼限界
x1 = Decimal(str(x1)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("LED電球の寿命の平均は信頼係数95%のもとで")
print("{} 日≦ μ ≦ {}日".format(x1,x2))

# 母標準偏差σの推定

x1,x2= stats.chi2.interval(alpha=0.95, df=len(df)-1)    #下側2.5%点,上側2.5%点
x1 = Decimal(str(x1)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

up = np.sqrt((len(df)-1)*s**2/float(x1))
bottom = np.sqrt((len(df)-1)*s**2/float(x2))

up = Decimal(str(up)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入
bottom = Decimal(str(bottom)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("LED電球の寿命の標準偏差は信頼係数95%のもとで")
print("{} 日≦ μ ≦ {}日".format(bottom,up))

## 練習問題　大問1


n=20
print("標本の大きさは{}です".format(n))

s=8
print("標本標準偏差は{}gです".format(s))

x1,x2= stats.chi2.interval(alpha=0.90, df=n-1)    #下側2.5%点,上側2.5%点
x1 = Decimal(str(x1)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("下側2.5%点は{}です".format(x1))
print("上側2.5%点は{}です".format(x2))

up = np.sqrt((n-1)*s**2/float(x1))
bottom = np.sqrt((n-1)*s**2/float(x2))

up = Decimal(str(up)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
bottom = Decimal(str(bottom)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入

print("{}(mg)≦σ≦{}(mg)".format(bottom,up))
print("信頼係数90%")

## 練習問題　大問2

``` py
# dictionaryからDataFrameを作る

data = {
    "木の高さ（m）":[10.5,9.7,9.5,10.1,11.0,9.4,9.5,10.2,10.5,10.0,
                     12.3,12.8,11.5,13.0,11.0,12.0,10.9,9.9,11.7,12.1]
}

df = pd.DataFrame(data)
df

# 平均を求める
print(df["木の高さ（m）"].mean())

# 不偏分散を求める
np.var(df["木の高さ（m）"],ddof=1)

# 普遍分散の平方根を求める（不偏標準偏差とはいいません）
print(np.sqrt(stats.tvar(df["木の高さ（m）"])))

x = df["木の高さ（m）"].mean()
s = np.sqrt(stats.tvar(df["木の高さ（m）"]))

# 母平均μの推定

x1,x2 = stats.t.interval(alpha=0.95,loc=x,scale=s/np.sqrt(len(df)),df=len(df)-1)    # 下方信頼限界 , 上方信頼限界
x1 = Decimal(str(x1)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("木の高さの平均は信頼係数95%のもとで")
print("{} m≦ μ ≦ {}m".format(x1,x2))

# 母標準偏差σの推定

x1,x2= stats.chi2.interval(alpha=0.95, df=len(df)-1)    #下側2.5%点,上側2.5%点
x1 = Decimal(str(x1)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

up = np.sqrt((len(df)-1)*s**2/float(x1))
bottom = np.sqrt((len(df)-1)*s**2/float(x2))

up = Decimal(str(up)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
bottom = Decimal(str(bottom)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入

print("木の高さの標準偏差は信頼係数95%のもとで")
print("{} m≦ μ ≦ {}m".format(bottom,up))