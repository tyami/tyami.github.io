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
This formula $f(x) = x^2$ is an example.  
```
This formula $f(x) = x^2$ is an example.  


### outline 수식
```latex
$$ \log_2 2 $$
$$ probability=p(x) $$
$$ information=I(x)=\log_2 \frac{1}{p(x)} $$
\[ Entropy=H(S)=\sum_{i=1}^c p_i\log_2 \frac{1}{p_i}=-\sum_{i=1}^c p_i\log_2 p_i $ \]
```
$$ \log_2 2 $$
$$ probability=p(x) $$
$$ information=I(x)=\log_2 \frac{1}{p(x)} $$
\[ Entropy=H(S)=\sum_{i=1}^c p_i\log_2 \frac{1}{p_i}=-\sum_{i=1}^c p_i\log_2 p_i $ \]

> 뭔가 문제가 있다. Outline 수식($$)만 인식을 못한다.