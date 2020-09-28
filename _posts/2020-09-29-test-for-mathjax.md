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
This formula $ f(x) = x^2$ is an example.  
This formula \ (f(x) = x^2 \) is an example.

```
This formula $ f(x) = x^2 $ is an example.  
This formula \( f(x) = x^2 \) is an example.


### outline 수식
```latex
\[ \log_2 2 \]
\[ probability=p(x) \]
\[ information=I(x)=\log_2 \frac{1}{p(x)} \]
\[ Entropy=H(S)=\sum_{i=1}^c p_i\log_2 \frac{1}{p_i}=-\sum_{i=1}^c p_i\log_2 p_i \]
```
\[ \log_2 2 \]
\[ probability=p(x) \]
\[ information=I(x)=\log_2 \frac{1}{p(x)} \]
\[ Entropy=H(S)=\sum_{i=1}^c p_i\log_2 \frac{1}{p_i}=-\sum_{i=1}^c p_i\log_2 p_i \]

> 구글링 했을 때 나오는 $$ 문법으로는 Outline 수식 표현이 되지 않는다.
> \_includes\mathjax_support.html 파일 작성 시, 인식하는 부분을 수정해주었다.