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

部分積分１次元

$$
\int^b_a u''v\,dx=\left[u'v\right]^b_a-\int^b_a u'v\,'dx
$$

２次元

$$
\int_\Omega \Delta u(x) v(x) dx=\int_{\partial \Omega}\frac{\partial u}{\partial n}(x)v(x)d\sigma-\int_\Omega \nabla u\cdot \nabla v dx
$$

$\frac{\partial}{\partial n}$は法線方向微分。
