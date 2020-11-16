---
title: "Python Jupyter notebook에서 shell command에 반복문 (for loop) 사용하는 법"
excerpt: "python shell command 명령어와 for loop를 함께 사용하는 법을 정리해봅니다"

categories:
- Python

tags:
- Python
- Jupyter notebook
- Shell command
- cmd
- argparse

toc: true
toc_sticky: true
toc_label:  "Python Shell command + for loop"

use_math: true
---

## Shell command in Python

[지난 포스팅]({{ site.url }}{{ site.baseurl }}/python/python-shell-command/)에서 Python에서 shell command를 사용하는 법을 정리했습니다.

`!`를 이용해 간단히 사용할 수 있었고, `argparse` 함수를 이용해 `*.py` 파일에 인자를 넣을 수도 있었습니다.

![2020-11-15-python-shell-command-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/Python/2020-11-15-python-shell-command/2020-11-15-python-shell-command-2.PNG)

이렇게 말이죠.

## Necessity of for loop

그런데, 만약 여러 `subject_id`를 반복적으로 shell command를 이용해 입력해야 하는 상황이 생긴다면 어떨까요?

```python
!python argparse_test.py --subject_id 1
!python argparse_test.py --subject_id 2
!python argparse_test.py --subject_id 3
!python argparse_test.py --subject_id 4
!python argparse_test.py --subject_id 5
```

이렇게 노가다를 하면 될까요?

![2020-11-16-python-argparse-for-loop-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/Python/2020-11-16-python-argparse-for-loop/2020-11-16-python-argparse-for-loop-1.PNG)

이 방법도 되긴 됩니다. 하지만 수백명의 데이터가 있다면, 어떨까요? 효과적인 방법은 `for loop`를 쓰는 방법입니다.

## Problem of for loop

하지만 `!` 이후의 명령어는 string 타입으로 argument parser에 전달되고, 이를 parsing해서 변수로 사용합니다.

![2020-11-16-python-argparse-for-loop-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/Python/2020-11-16-python-argparse-for-loop/2020-11-16-python-argparse-for-loop-2.PNG)

따라서 냅다 for loop의 변수를 값으로 사용하는 것은 옳지 않은 방법입니다.

## Usage of for loop

그렇다면 올바른 사용법은 어떤 것일까요?

```python
for sub in range(5):
    !python argparse_test.py --subject_id {sub}
```

간단히 변수에 `{ }` 괄호를 씌워주면 됩니다.

![2020-11-15-python-shell-command-3]({{ site.url }}{{ site.baseurl }}/assets/images/post/Python/2020-11-15-python-shell-command/2020-11-15-python-shell-command-3.PNG)

네 잘 됩니다.