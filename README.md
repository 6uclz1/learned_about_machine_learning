# learned_about_machine_learning

基本的な分析手法
================

機械学習についての概略の基本的な分析手法を学んでいく．
まず，基本的な分析手法は４つに分類される．\
\
  1. 線形回帰分析\
  2. ロジスティック回帰分析\
  3. 因子分析\
  4. クラスター分析\

今回は線形回帰分析について学んだ．
線形回帰分析とは複数の変数における相関関係を直線モデルによって説明しようとする分析手法のことである．

下記の図が使用するデータである．

[htb]

   顧客番号   年齢(x)   年間支出額(y)
  ---------- --------- ---------------
      1         32         40,000
      2         23         38,000
      3         39         46,000
      4         47         48,000

[population]

図の作成及び計算はPythonを用いた．

線形回帰分析
============

顧客の年齢と年間支出額がなんらかの関係があるのかを考えていく．
関係を分かりやすくするため，表１のデータをグラフにすると図１のようになる．

![表１の散布図](./src/output_1_0.png)

この際用いたソースコードが下記になる\

``` {caption="図１の作成に用いたもの"}
import numpy as np
import matplotlib.pyplot as plt

x = np.array([32, 23, 39, 47])
y = np.array([40000, 38000, 46000, 48000])

plt.scatter(x, y, color = 'blue')
plt.show()
```

図１でプロットされたデータに一本の近似した直線を引くことによって，年齢と年間支出額を予測する．
直線のグラフは，$y = \beta_1 x+ \beta_0$で表される．
このような変数xとyの間の関係のとき，xとyは線形関係にあるという．

年齢(x)が原因で，年間支出額(y)が結果と考えると，y方向に誤差が出ていると考えられる．
そのため垂直方向の距離を誤差とする．
この際，最小二乗法を使い，誤差を最小に近づける．

まず，グラフ $y=\beta_1 x+\beta_0$ の予想値を考える．
顧客番号２は年齢(x)が23で年間支出額が38,000円である．

年齢(x)に23に代入すると， $ y = 23\beta_1 +\beta_0 $ になる．
そこに実際の購入額である38,000円をyに代入すると，
$38,000 = 23\beta_1 + \beta_0$ になる．
さらに式を変形して$0 = 23\beta_1 + \beta_0 - 38,000$にし，
正と負の関係性を無視できるよう，二乗すると
$ 0=(23\beta_1 +\beta_0-38,000)^2 $ になる．

これにより，それぞれの残差のみに注目できるようになった．
同様に３つのデータについても，差の二乗を計算し，それらを合計すると，
$\beta_1$と$\beta_0$ の関係式になる． これを $\beta_1$と$\beta_0$
を調整して最小に近づける．

以下が式になる．

$$(32\beta_1+\beta_0-40,000)^2 + (23\beta_1+\beta_0-38,000)^2 + (39\beta_1+\beta_0-46,000)^2+(47\beta_1+\beta_0+48,000)^2 \geqq 0$$\
この式により，

$$\beta_1 \fallingdotseq 450.839 \hspace{4mm} \beta_0 \fallingdotseq 27107.913$$
が求まる．\
\
よって，$y = \beta_1 x+ \beta_0$が\
$$y \fallingdotseq 450.839 x+ 27107.913$$\
となり，近似した1本の直線が求められた．\
\
以下が図１に近似した１本の直線を加えたものである．\

![近似した1本の直線を追加したもの](./src/output_2_0.png)

図２を作る際，用いたソースコードが下記になる．\

``` {caption="図２の作成に用いたもの"}
import numpy as np
import matplotlib.pyplot as plt

x = np.array( [32, 23, 39, 47] )
y = np.array( [40000, 38000, 46000, 48000] )

plt.scatter(x, y, color = 'blue')
xdata = np.vstack( [x, np.ones(len(x))] ).T
b1, b0 = np.linalg.lstsq(xdata, y)[0]

plt.scatter(x, y, color = 'blue')
plt.plot(x, (b1 * x + b0), color = 'red')
plt.show()
```

ソースコード2では．numpy.liang.lstsg関数を使い，行列xdata，yを代入し，$(x\beta_1 + \beta_0 - y)^2$を最小にする$\beta_1$(b1)，$\beta_0$(b0)を求める．

xdataはnumpy.liang.lstsgでxの値が扱えるように，全要素が１の配列を加えたものである．

$$xdata = \left(
    \begin{array}{ccc}
      32 & 1 \\
      12 & 1 \\
      39 & 1 \\
      47 & 1
    \end{array}
  \right)$$

np.linalg.lstsq関数にxdata, yを代入した時，出力される値が以下になる．\

    np.linalg.lstsq(xdata, y) = ( x の配列， 残差， 階数， 特異値の配列 )
                                           = ( array ( [ 450.83932854,  27107.91366906 ] ),
                                                 array ( [ 4431654.67625901 ] ),
                                                 2,
                                                 array ( [ 72.71013252,  0.48644497] ) )

求めているのは，返り値の１つ目の要素なので[0]で一つ目を指定してb1,b0に代入する．

最後に，$y = \beta_1 x + \beta_0$に相当する$y = b1*x + b0$を
$plt.plot(x, (b1 * x + b0))$ で描写する．

<span>2</span>

NumPy Reference\
https://docs.scipy.org/doc/numpy/reference/index.html

解析情報グループ 最小二乗法\
http://kaiseki-web.lhd.nifs.ac.jp/documents/Python/leastmean.htm
