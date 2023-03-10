# introduction_to_freefem

FreeFemの使い方を解説します。

FreeFemは有限要素法を使って微分方程式を数値的に解きます。ここでは２次元問題の解き方を考えます。

FreeFEM - An open-source PDE Solver using the Finite Element Method

[https//freefem.org](https://freefem.org)

まずは、有限要素法の考え方をザックリ述べます。簡単のため、１次元問題で述べます。

（有限要素法の入門書には、サイエンス社から出ている菊地先生の有限要素法概論があります。）

以下の問題を有限要素法で解くことを考えます。

$$
-\frac{d^2}{dx^2}u=f
$$

![equation](https://user-images.githubusercontent.com/1296728/222445685-818dcefb-7240-4920-9067-21165ebf7c21.png)

この方程式を内積をとって変形します

![FreeFEM -4](https://user-images.githubusercontent.com/1296728/222446189-ac1d73e2-1106-4a25-a03c-72df6d2d0acb.jpg)

以下の方程式を得ます

$$
(\frac{d}{dx}u,\frac{d}{dx}v)=(f,v)
$$

![equation](https://user-images.githubusercontent.com/1296728/222448037-5d165653-4ce6-4b14-9ed9-63d872e8ceb5.png)

得た方程式から近似方程式を考えます。区間を分割し、その上で以下のような区分一次関数を考えます

![FreeFEM 0 4](https://user-images.githubusercontent.com/1296728/222480868-270f19a9-a90e-4190-a425-dd64db8527d6.jpg)

近似解がこの区分一次関数の線形結合で表されるとして、これを先の方程式に代入します

![FreeFEM -7](https://user-images.githubusercontent.com/1296728/222481484-162aae4e-d3b8-4b05-9c9b-360a4eabbc39.jpg)

最後に以下の連立方程式を得ます

![FreeFEM -8](https://user-images.githubusercontent.com/1296728/222481653-f6e6b827-801d-4bf5-b324-b95ef6584c89.jpg)

と言うことで、近似解を求める問題が連立一次方程式を求めることに帰着されました

いろいろ説明を端折ってます。こんな感じと思っていただければ充分です。

有限要素法の入門を明治大学の桂田先生がpdfで公開しています。

http://nalab.mind.meiji.ac.jp/~mk/lecture/ouyousuuchikaisekitokuron-2015/fem.pdf

FreeFemですが、これで特に重要なのは境界と境界条件の設定、方程式をfreefemの言語で記述することです。
領域のメッシュ分割は自動で行われますし、連立方程式の生成とその解を求めることも自動で行われます

有限要素法では２階の方程式を解くことが多いのですが、方程式に関数をかけて積分して弱形式化して、部分積分して階数を下げます。

有限要素法では弱形式にして部分積分で微分の階数を下げることをやります。これをやると、近似に使う基底関数の次数を低く取れると言う利点があります。
例えば、２階の問題だと、区分一次関数は2階微分でゼロですが、階数を下げれば微分は区分定数です。

部分積分１次元

$$
\int^b_a u''v\,dx=\left[u'v\right]^b_a-\int^b_a u'v\,'dx
$$

２次元

$$
\int_\Omega \Delta u(x) v(x) dx=\int_{\partial \Omega}\frac{\partial u}{\partial n}(x)v(x)d\sigma-\int_\Omega \nabla u\cdot \nabla v dx
$$

$\frac{\partial}{\partial n}$は法線方向微分。

# メッシュ生成

以下のような境界を持った領域をfreefemでメッシュ分割します

コード：boundary-freefem.edp: 境界の作図

<img width="1121" alt="boundary-pic" src="https://user-images.githubusercontent.com/1296728/222585355-ffd49aaa-48b3-41e0-b394-57a33d57ff4f.png">

![FreeFEM -11](https://user-images.githubusercontent.com/1296728/222694848-e69d65b1-5380-4eae-9560-21341ad3b58b.jpg)

境界を $\Gamma_1$, $\Gamma_2$, $\Gamma_3$, $\Gamma_4$ の４つの部分で考えます。これらは以下のようにパラメータ表示できます。

$$
\begin{eqnarray*}
\begin{array}{l}
\Gamma_1: x=t,y=0,\quad R_1 \le t \le R_2 \\
\Gamma_2: x=R_2 \cos t, y=R_2 \sin t, \quad 0 \le t \le \pi \\
\Gamma_3: x=t,y=0,\quad -R_2 \le t \le -R_1 \\
\Gamma_4: x=R_1 \cos (\pi-t)t,y=R_1 \sin (\pi-t),\quad -\pi \le t \le 0
\end{array}
\end{eqnarray*}
$$

これらの曲線は向きを持ってます。繋ぐと、反時計まわりの閉曲線になります。これをfreefemの命令で表すと

<img width="433" alt="freefem-boundary-1" src="https://user-images.githubusercontent.com/1296728/222696954-16491f3c-0c8f-49a6-80df-1a3a81d973a5.png">

向きを持った曲線の進行方向左側が領域の中身になります。
ここまでくると、メッシュ生成は以下のコードでできます。

<img width="443" alt="mesh-gen-1" src="https://user-images.githubusercontent.com/1296728/222697658-2edcb508-9be2-48e9-9114-d017ab7dae85.png">

コード：mesh-freefem.edp: メッシュ生成

<img width="1121" alt="freefem-mesh-1" src="https://user-images.githubusercontent.com/1296728/222698671-3130c8a5-e562-4b4a-a5bf-0dccbeffe66e.png">
