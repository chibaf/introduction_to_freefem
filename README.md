# introduction_to_freefem
introduction to freefem


FreeFemの使い方を解説します。

FreeFemは有限要素法を使って微分方程式を数値的に解きます。ここでは２次元問題の解き方を考えます。

FreeFEM - An open-source PDE Solver using the Finite Element Method

freefem.org

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
