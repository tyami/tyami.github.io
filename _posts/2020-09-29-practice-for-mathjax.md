---
title: "Jekyll 블로그에 MathJax 수식 사용하기"
excerpt: "minimal-mistake Jekyll 블로그에서 수식을 사용해봅시다 (MathJax)"

categories:
- Blog

tags:
- Blog
- Jekyll
- MathJax

toc: true
toc_sticky: true
toc_label: "Jekyll 블로그에 MathJax 수식 사용하기"

use_math: true
---

## 참고
- [수식 live test](https://www.mathjax.org/#demo)
- [수식 작성방법](https://ghdic.github.io/math/default/mathjax-%EB%AC%B8%EB%B2%95/)
- [위 페이지 github code](https://raw.githubusercontent.com/ghdic/ghdic.github.io/master/_posts/default/2020-02-01-mathjax-%EB%AC%B8%EB%B2%95.md)

## 예제
### Inline 수식
```latex
This formula $ f(x) = x^2 $ is an example.  
This formula \\( f(x) = x^2 \\) is an example.
```
This formula $ f(x) = x^2 $ is an example.  
This formula \\( f(x) = x^2 \\) is an example.


### outline 수식
```latex
$$ \log_2 2 $$  
\[ \log_2 2 \]  
\[ probability=p(x) \]  
\[ information=I(x)=\log_2 \frac{1}{p(x)} \] 
\[ Entropy=H(S)=\sum_{i=1}^c p_i\log_2 \frac{1}{p_i}=-\sum_{i=1}^c p_i\log_2 p_i \]
```
$$ \log_2 2 $$  
\[ \log_2 2 \]  
\[ probability=p(x) \]  
\[ information=I(x)=\log_2 \frac{1}{p(x)} \]  
\[ Entropy=H(S)=\sum_{i=1}^c p_i\log_2 \frac{1}{p_i}=-\sum_{i=1}^c p_i\log_2 p_i \]

> 구글링 했을 때 나오는 $$ 문법으로는 Outline 수식 표현이 되지 않는다.  
> 따라서 \_includes\mathjax_support.html 파일 작성 시, 수식 시작과 끝을 인식하는 identifier들을 수정해주었다.

### aligned으로 수식 강제 줄 바꾸기
- begin/end로 수식 시작
- backslash **6번** 쓰면 강제 줄 바꿈
  - 보통 LaTex 문법은 2개인데 왜인지는 모르지만 안 됨
- &로 align할 위치 지정

```latex
\begin{aligned}
H(Play)&=-\sum_{i=1}^c p_i\log_2 p_i \\\\\\
&=-(\frac{5}{14}log_2\frac{5}{14}+\frac{9}{14}log_2\frac{9}{14}) \\\\\\
&=0.94
\end{aligned}
```
\begin{aligned}
H(Play)&=-\sum_{i=1}^c p_i\log_2 p_i \\\\\\
&=-(\frac{5}{14}log_2\frac{5}{14}+\frac{9}{14}log_2\frac{9}{14}) \\\\\\
&=0.94
\end{aligned}

### Equation number 넣기
- aligned 대신 eqnarray를 사용한다

```latex
\begin{eqnarray}
H(Play)&=&-\sum_{i=1}^c p_i\log_2 p_i \\\\\\
&=&-(\frac{5}{14}log_2\frac{5}{14}+\frac{9}{14}log_2\frac{9}{14}) \\\\\\
&=&0.94
\end{eqnarray}
```
\begin{eqnarray}
H(Play)&=&-\sum_{i=1}^c p_i\log_2 p_i \\\\\\
&=&-(\frac{5}{14}log_2\frac{5}{14}+\frac{9}{14}log_2\frac{9}{14}) \\\\\\
&=&0.94
\end{eqnarray}