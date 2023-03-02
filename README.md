# introduction_to_freefem
introduction to freefem


FreeFemの使い方を解説します。

FreeFemは有限要素法を使って微分方程式を数値的に解きます。ここでは２次元問題の解き方を考えます。

FreeFEM - An open-source PDE Solver using the Finite Element Method

freefem.org

まずは、有限要素法の考え方をザックリ述べます。簡単のため、１次元問題で述べます。

（有限要素法の入門書には、サイエンス社から出ている菊地先生の有限要素法概論があります。）

以下の問題を有限要素法で解くことを考えます。

![equation](https://user-images.githubusercontent.com/1296728/222445685-818dcefb-7240-4920-9067-21165ebf7c21.png)

この方程式を内積をとって変形します

![FreeFEM -4](https://user-images.githubusercontent.com/1296728/222446189-ac1d73e2-1106-4a25-a03c-72df6d2d0acb.jpg)
