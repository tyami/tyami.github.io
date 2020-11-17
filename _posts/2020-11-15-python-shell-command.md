---
title: "Python Jupyter notebook에서 Shell command (cmd) 사용하기"
excerpt: "! 명령어와 argparse 함수를 이용해서 Python에서 shell command를 쓰는 법을 정리해봅니다"

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
toc_label: "Python Shell command (cmd) 명령어 사용"

use_math: true
---

# Function

```python
def print_subject_id(subject_id):
    print('Subject ID: ', subject_id )
```

이 함수는 `subject_id`를 입력으로 받아 화면에 출력해주는 간단한 함수입니다. 이 함수를 여러 Python 코드 내에서 사용하려면 일반적으로 `utils.py` 파일 안에 작성해준 후 `import utils`를 통해 불러와 사용합니다.

# Shell command

Shell (실행 > cmd) 에서 Python 코드를 실행하고자 할 때, 함수의 입력 변수들은 명령어 뒤에 쭉 이어서 작성합니다. Github 등 사람들이 작성해둔 코드를 보면, 아래와 같이 `--`를 이용해 변수의 값을 넣어주는 것을 많이 보셨을 겁니다.

예를 들어 키 `height`와 몸무게 `weight`를 받아 BMI 지수를 계산하는 `BMI.py`는 아래와 같이 호출합니다.

```python
!python BMI.py --height 185 --weight 85
```

height, weight와 같이 변수 역할을 하는 것을 **인자 (argument)**라고 부르고, 185, 85와 같이 데이터의 역할을 하는 것을 **값**이라고 부릅니다.

# argparse

argparse는 Python 내장 라이브러리로, 인자에 대한 값을 정해진 형태에 맞추어 편하게 넣도록 만들어졌습니다.

## Import

별도로 설치할 필요 없이 아래 함수로 불러옵니다.

```python
import argparse
```

## argparse basic

일반적으로 아래와 같이 `*.py` 파일을 작성합니다.

`argparse_test.py`:

```python
import argparse

# ArgumentParser 생성
parser = argparse.ArgumentParser(description='argparse 테스트 파일입니다.')

# Argument 정의
parser.add_argument('--subject_id', required=True, type=int, help='Subject ID를 입력하세요 (int)')

# Argument parsing
args = parser.parse_args()

subject_id = args.subject_id

print('Subject ID: ', subject_id )
```

위 파일을 한 줄 한 줄 뜯어보면 아래 순서로 구성되어 있습니다. Github 등에 사람들이 올려둔 파일

1. `argparse.ArgumentParser` 함수를 이용해 argument parser class를 생성해줍니다.
2. `add_argument` 함수를 이용해 각 arguement의 이름, 조건 (`requried`, `type`, `default`), 도움말 (`help`)을 정의합니다.
   3 `parse_args` 함수를 이용해 인자들의 값을 불러옵니다 (parsing).
3. 값을 사용해 `*.py` 파일 내 코드를 실행합니다.

## Shell command in Python

Python (Jupyter notebook)에서 Shell command 실행은 아래와 같이 명령어 앞에 `!`를 붙여주면 됩니다.

### Help

```python
!python argparse_test.py -h
```

**📌 NOTE**

![2020-11-15-python-shell-command-1]({{ site.url }}{{ site.baseurl }}/assets/images/post/Python/2020-11-15-python-shell-command/2020-11-15-python-shell-command-1.PNG)

`-h` 인자는 각 `argument`에 대한 설명을 보여줍니다.

### Usage

```python
!python argparse_test.py --subject_id 1
```

인자와 키 사이에 공백을 넣어 입력합니다.

![2020-11-15-python-shell-command-2]({{ site.url }}{{ site.baseurl }}/assets/images/post/Python/2020-11-15-python-shell-command/2020-11-15-python-shell-command-2.PNG)

## argparse application

맨 처음 예시에서 예로 든 BMI 지수를 내뱉는 함수를 작성해봅시다.

```python
import argparse

parser = argparse.ArgumentParser(description='argparse 응용 파일입니다.')

parser.add_argument('--subject_id', required=True, type=int, help='Subject ID를 입력하세요 (int)')
parser.add_argument('--height', required=True, type=float, help='Subject의 height (cm)를 입력하세요 (float)')
parser.add_argument('--weight', required=True, type=float, help='Subject의 weight (kg)를 입력하세요 (float)')

args = parser.parse_args()

subject_id = args.subject_id
height = args.height
weight = args.weight

def BMI(height, weight):
    height = height / 100 # cm to m

    return weight / height**2

print('Subject ID: ', subject_id)
print('Height (cm): ', height)
print('Weight (kg): ', weight)

bmi = BMI(height, weight)
print('BMI: ', bmi)

if bmi < 20:
    print('저체중입니다')
elif bmi < 24:
    print('정상 체중입니다')
elif bmi < 29:
    print('과체중입니다')
else:
    print('비만입니다')
```

```python
!python argparse_application_BMI.py -h
```

![2020-11-15-python-shell-command-3]({{ site.url }}{{ site.baseurl }}/assets/images/post/Python/2020-11-15-python-shell-command/2020-11-15-python-shell-command-3.PNG)

이 함수는 shell command로 `subject_id`, `height` 그리고 `weight`를 입력으로 받습니다. 이후 parsing된 변수를 이용해 BMI 지수를 계산해서 출력합니다.

```python
!python argparse_application_BMI.py --subject_id 1 --height 185 --weight 87
```

![2020-11-15-python-shell-command-4]({{ site.url }}{{ site.baseurl }}/assets/images/post/Python/2020-11-15-python-shell-command/2020-11-15-python-shell-command-4.PNG)

잘 되긴 하는데, 좀 열받네요.
