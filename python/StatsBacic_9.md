# 統計　第9回


```python
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


```python
# dictionaryからDataFrameを作る

data = {
    "X_i":[168,141,157,151,160,155,148,160]
}

df = pd.DataFrame(data)
df
```





  <div id="df-72ddbf3e-2739-436b-b412-c15ae1cb5aa9">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

   
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>X_i</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>168</td>
    </tr>
    <tr>
      <th>1</th>
      <td>141</td>
    </tr>
    <tr>
      <th>2</th>
      <td>157</td>
    </tr>
    <tr>
      <th>3</th>
      <td>151</td>
    </tr>
    <tr>
      <th>4</th>
      <td>160</td>
    </tr>
    <tr>
      <th>5</th>
      <td>155</td>
    </tr>
    <tr>
      <th>6</th>
      <td>148</td>
    </tr>
    <tr>
      <th>7</th>
      <td>160</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-72ddbf3e-2739-436b-b412-c15ae1cb5aa9')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      
  </div>

<div style="page-break-before:always"></div>



```python
μ=155
print("母平均は{}です".format(μ))

σ=14
print("母標準偏差は{}です".format(σ))
```

    母平均は155です
    母標準偏差は14です



```python
df["(X_i-μ)^2"]=df["X_i"].apply(lambda X_i : (X_i-μ)**2)
df
```





  <div id="df-2afdd929-545e-4cd5-b290-3767ee2bef8c">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

   
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>X_i</th>
      <th>(X_i-μ)^2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>168</td>
      <td>169</td>
    </tr>
    <tr>
      <th>1</th>
      <td>141</td>
      <td>196</td>
    </tr>
    <tr>
      <th>2</th>
      <td>157</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>151</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>160</td>
      <td>25</td>
    </tr>
    <tr>
      <th>5</th>
      <td>155</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>148</td>
      <td>49</td>
    </tr>
    <tr>
      <th>7</th>
      <td>160</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-2afdd929-545e-4cd5-b290-3767ee2bef8c')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

 
    
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

     
  </div>





```python
sum(df["(X_i-μ)^2"])/σ**2
```




    2.4693877551020407

<div style="page-break-before:always"></div>

## 確認のためのQ&A・7.3


```python
n=12
print("標本の大きさは{}です".format(n))

s=8.6
print("標本標準偏差は{}です".format(s))
```

    標本の大きさは12です
    標本標準偏差は8.6です



```python
x1,x2= stats.chi2.interval(alpha=0.95, df=n-1)    #下側2.5%点,上側2.5%点
x1 = Decimal(str(x1)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("下側2.5%点は{}です".format(x1))
print("上側2.5%点は{}です".format(x2))
```

    下側2.5%点は3.82です
    上側2.5%点は21.9です



```python
up = np.sqrt((n-1)*s**2/float(x1))
bottom = np.sqrt((n-1)*s**2/float(x2))

up = Decimal(str(up)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
bottom = Decimal(str(bottom)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入

print("{}≦σ≦{}".format(bottom,up))
print("信頼係数95%")
```

    6.09≦σ≦14.59
    信頼係数95%

<div style="page-break-before:always"></div>

## 確認のためのQ&A・7.4


```python
# dictionaryからDataFrameを作る

data = {
    "LED電球の寿命（日）":[2752,2563,2689,2405,2570,2477,2523,2549,2601,2594,2643,2590,2721,2683,2477]
}

df = pd.DataFrame(data)
df
```





  <div id="df-f77f378c-c47b-4d6d-8505-b2f81ccfbd78">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LED電球の寿命（日）</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2752</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2563</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2689</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2405</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2570</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2477</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2523</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2549</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2601</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2594</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2643</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2590</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2721</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2683</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2477</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-f77f378c-c47b-4d6d-8505-b2f81ccfbd78')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  
    
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

     
  </div>


<div style="page-break-before:always"></div>


```python
# 平均を求める
print(df["LED電球の寿命（日）"].mean())

# 不偏分散を求める
np.var(df["LED電球の寿命（日）"],ddof=1)

# 普遍分散の平方根を求める（不偏標準偏差とはいいません）
print(np.sqrt(stats.tvar(df["LED電球の寿命（日）"])))

x = df["LED電球の寿命（日）"].mean()
s = np.sqrt(stats.tvar(df["LED電球の寿命（日）"]))
```

    2589.133333333333
    97.04628252736399



```python
# 母平均μの推定

x1,x2 = stats.t.interval(alpha=0.95,loc=x,scale=s/np.sqrt(len(df)),df=len(df)-1)    # 下方信頼限界 , 上方信頼限界
x1 = Decimal(str(x1)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("LED電球の寿命の平均は信頼係数95%のもとで")
print("{} 日≦ μ ≦ {}日".format(x1,x2))
```

    LED電球の寿命の平均は信頼係数95%のもとで
    2535.4 日≦ μ ≦ 2642.9日

<div style="page-break-before:always"></div>

```python
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
```

    LED電球の寿命の標準偏差は信頼係数95%のもとで
    71.1 日≦ μ ≦ 153.0日

<div style="page-break-before:always"></div>

## 練習問題　大問1



```python
n=20
print("標本の大きさは{}です".format(n))

s=8
print("標本標準偏差は{}gです".format(s))
```

    標本の大きさは20です
    標本標準偏差は8gです



```python
x1,x2= stats.chi2.interval(alpha=0.90, df=n-1)    #下側2.5%点,上側2.5%点
x1 = Decimal(str(x1)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("下側2.5%点は{}です".format(x1))
print("上側2.5%点は{}です".format(x2))
```

    下側2.5%点は10.1です
    上側2.5%点は30.1です



```python
up = np.sqrt((n-1)*s**2/float(x1))
bottom = np.sqrt((n-1)*s**2/float(x2))

up = Decimal(str(up)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入
bottom = Decimal(str(bottom)).quantize(Decimal('0.01'), rounding=ROUND_HALF_UP)    # 四捨五入

print("{}(mg)≦σ≦{}(mg)".format(bottom,up))
print("信頼係数90%")
```

    6.36(mg)≦σ≦10.97(mg)
    信頼係数90%

<div style="page-break-before:always"></div>

## 練習問題　大問2


```python
# dictionaryからDataFrameを作る

data = {
    "木の高さ（m）":[10.5,9.7,9.5,10.1,11.0,9.4,9.5,10.2,10.5,10.0,
                     12.3,12.8,11.5,13.0,11.0,12.0,10.9,9.9,11.7,12.1]
}

df = pd.DataFrame(data)
df
```





  <div id="df-779f3aad-982a-4b91-9231-28da2b8eae2d">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

   
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>木の高さ（m）</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>9.4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>9.5</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10.2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>10.5</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>12.3</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12.8</td>
    </tr>
    <tr>
      <th>12</th>
      <td>11.5</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>11.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>12.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>10.9</td>
    </tr>
    <tr>
      <th>17</th>
      <td>9.9</td>
    </tr>
    <tr>
      <th>18</th>
      <td>11.7</td>
    </tr>
    <tr>
      <th>19</th>
      <td>12.1</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-779f3aad-982a-4b91-9231-28da2b8eae2d')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  
    
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      
  </div>


<div style="page-break-before:always"></div>


```python
# 平均を求める
print(df["木の高さ（m）"].mean())

# 不偏分散を求める
np.var(df["木の高さ（m）"],ddof=1)

# 普遍分散の平方根を求める（不偏標準偏差とはいいません）
print(np.sqrt(stats.tvar(df["木の高さ（m）"])))

x = df["木の高さ（m）"].mean()
s = np.sqrt(stats.tvar(df["木の高さ（m）"]))
```

    10.879999999999999
    1.1358280077361602



```python
# 母平均μの推定

x1,x2 = stats.t.interval(alpha=0.95,loc=x,scale=s/np.sqrt(len(df)),df=len(df)-1)    # 下方信頼限界 , 上方信頼限界
x1 = Decimal(str(x1)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入
x2 = Decimal(str(x2)).quantize(Decimal('0.1'), rounding=ROUND_HALF_UP)    # 四捨五入

print("木の高さの平均は信頼係数95%のもとで")
print("{} m≦ μ ≦ {}m".format(x1,x2))
```

    木の高さの平均は信頼係数95%のもとで
    10.3 m≦ μ ≦ 11.4m

<div style="page-break-before:always"></div>

```python
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
```

    木の高さの標準偏差は信頼係数95%のもとで
    0.86 m≦ μ ≦ 1.66m

