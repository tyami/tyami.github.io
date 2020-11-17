---
title: "윈도우즈에서 Jekyll blog 로컬 서버 자동 시작 파일 만들기"
excerpt: "automatical execution of Jekyll blog local server"

categories:
  - Blog

tags:
  - Blog
  - Jekyll

toc: true
toc_sticky: true
toc_label: "Jekyll blog local server"
---

> 이번 포스팅에서는 **Jekyll blog를 local server에서 구동하는 방법**을 정리해보고자 합니다.

2020.11.17 기준 본 블로그는 Jekyll [minimal mistake](https://github.com/mmistakes/minimal-mistakes)을 바탕으로 만들어졌습니다.

블로그 관리 및 포스팅 시, 매번 git commit해서 수정된 걸 보다가 쓸데없이 Github commit수 만 뻥튀기되고, 이건 좀 아닌 것 같아서 로컬 서버를 구동해서 수정사항을 보기로 했습니다.

# Requirements

Jekyll blog 로컬 서버 구동에는 아래 두 가지가 필요합니다. 자세한 내용은 Procedure에서 설명해두었습니다.

- Jekyll blog folder
- Ruby
- Gemfile

# Procedure

## 1. Install Ruby

Ruby는 Python과 같은 프로그래밍 언어입니다. Jekyll은 Ruby를 기반으로 동작합니다.

![ruby-install.PNG]({{ site.url }}{{ site.baseurl }}/assets/images/post/Blog/2020-11-17-jekyll-local-server-start-batch/ruby-install.PNG)

[Ruby website](https://rubyinstaller.org/downloads/)에서 Ruby를 다운로드받아 설치합니다.

설치과정은 생략합니다.

**📌 NOTE**
- 설치 마지막에 `Run 'ridk install' ...`를 체크해줍니다. cmd 창이 나타나면서 무슨 글자가 뜨는데, `1`을 치고 엔터를 눌러줍니다. 그러면 자동으로 뭔가를 설치합니다.

## 2. Prepare Gemfile

Ruby 언어에서는 플러그인들을 `gem` 이라고 합니다. 

Jekyll server를 구동하기 위해서 필요한 플러그인을 모아둔 파일을 `Gemfile`이라고 합니다. Python의 `requirements.txt` 같은 역할이라고 보면 될 것 같습니다.

Jekyll blog의 theme에 따라 필요한 플러그인이 다르기 때문에, Gemfile을 다르게 쓰며, 일반적으로 Jekyll blog의 root 폴더에 포함되어 있습니다.

**Gemfile 예시**:

```ruby
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins

gem "tzinfo-data"
gem "wdm", "~> 0.1.0" if Gem.win_platform?

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jekyll-algolia"
  gem 'jekyll-include-cache'
  gem 'github-pages'
end
```

Jekyll server 구동 시, 특정 버전 이상의 `gem`을 설치하도록 요구할 수도 있습니다.  `gem 'gem name', "~> version"` 처럼 쓰면 됩니다. Error message를 잘 읽어보세요.

## 3. Local server 구동

![2020-11-17-jekyll-local-server-start-batch-2.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/Blog/2020-11-17-jekyll-local-server-start-batch/2020-11-17-jekyll-local-server-start-batch-2.png)

cmd를 실행합니다.

![2020-11-17-jekyll-local-server-start-batch-3.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/Blog/2020-11-17-jekyll-local-server-start-batch/2020-11-17-jekyll-local-server-start-batch-3.png)

```
D:
cd 경로
```
cmd에서 드라이브를 이동할 때는 `cd` 커맨드 없이 `D:` 와 같이 드라이브만 적습니다. 드라이브를 맞춰준 후 `cd` 명령어로 Jekyll blog가 설치된 폴더로 이동해줍니다. 

```
bundle exec jekyll serve
```
Jekyll blog local server를 구동합니다.

![jekyll-server-start]({{ site.url }}{{ site.baseurl }}/assets/images/post/Blog/2020-11-17-jekyll-local-server-start-batch/jekyll-server-start.PNG)

문제없이 구동이 되었다면 위와 같은 내용이 나타납니다.

```
bundle install
```

뭐가 없다고 그러면 `Gemfile` 파일에 추가해주고, `bundle install`로 설치합니다.


## 4. 로컬 서버 접속

[localhost:4000](http://localhost:4000/)로 접속합니다.

### 로컬 서버 업데이트

![jekyll-server-update]({{ site.url }}{{ site.baseurl }}/assets/images/post/Blog/2020-11-17-jekyll-local-server-start-batch/jekyll-server-update.PNG)

로컬 서버는 파일의 수정이 발생되면 거의 실시간으로 업데이트 됩니다. 다만 몇 초 정도 걸릴 수 있으니 기다려주세요.

**📌 NOTE**
- 최상단 루트에 있는 `_config.yml`의 수정사항은 Local server를 재시작해야 적용됩니다. 그 외에는 바로바로 적용됩니다.

### 로컬 서버 업데이트 에러

![jekyll-server-update-error]({{ site.url }}{{ site.baseurl }}/assets/images/post/Blog/2020-11-17-jekyll-local-server-start-batch/jekyll-server-update-error.PNG)

업데이트 시 문제가 발생하면, cmd에서 에러가 발생한 이유를 확인할 수 있습니다. 복사해서 구글링하면 됩니다 😁

## 5. Local server 자동 구동 파일 만들기

cmd로 경로까지 들어가서 `bundle exec jekyll serve` 치는 과정이 너무 귀찮다면, 간단하게 **클릭 한 번 만으로 로컬 서버를 구동시키는 파일**을 만들어봅시다.

![2020-11-17-jekyll-local-server-start-batch-5.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/Blog/2020-11-17-jekyll-local-server-start-batch/2020-11-17-jekyll-local-server-start-batch-5.png)


메모장을 열어, cmd에서 적었던 모든 명령어를 적어줍니다.

```
D:
cd 경로
bundle exec jekyll serve
```

![2020-11-17-jekyll-local-server-start-batch-6.png]({{ site.url }}{{ site.baseurl }}/assets/images/post/Blog/2020-11-17-jekyll-local-server-start-batch/2020-11-17-jekyll-local-server-start-batch-6.png)

`파일명.bat` 파일로 저장해줍니다. 이후 해당 파일을 실행하면 cmd가 실행되며, Jekyll local server가 구동됩니다.

끝.