---
layout: post
title: 'Jekyll, Gitlab(또는 Github)을 이용한 블로그 개설'
author: shinho.kang
date: 2018-06-03 10:00
tags: [jekyll, gitlab-pages]
---

2016년, Github pages에 jekyll로 블로그를 만들었던 적이 있었지만, 몇개 포스팅 하지도 않고 닫았던 (후회되는) 경험이 있다.  
다시 한번 마음을 다잡고, 공간을 마련했다. 포스팅 이외의 것에 신경을 쓰고 싶지 않아서 테마도 붙이지 않고 별 다른 설정도 하지 않았다.  
처음엔 node.js 기반인 Hexo를 시도했지만, 몇개의 버그를 만난 뒤 다시 jekyll로 돌아왔다.

더불어 이번에는 github이 아니라 gitlab을 이용하기로. github과 bitbucket은 활발히 사용했는데, gitlab은 아직 경험이 없어서 한번 이용해보기로 했다.

  
# Jekyll이란?
참고: [https://jekyllrb-ko.github.io/](https://jekyllrb-ko.github.io/)  
참고: [정적 웹사이트 생성기의 역습 - 동적 스크립트를 넘어 다시 정적 컨텐츠로](http://blog.nacyot.com/articles/2014-01-15-static-site-generator/)  

markdown 문법으로 포스트만 작성해두면, 알아서 HTML 기반의 사이트를 만들어 주는 생성기이다. 기본 테마만으로도 아주 심플한 블로그 형의 사이트를 만들어 주기도 하고, html,css 그리고 ruby를 이용해서 취향에 맞는 사이트를 제작 할 수도 있다.

**Git과 Markdown을 사용할 수 있으면 좋다**


# Local 환경 구축

참고: [https://jekyllrb.com/docs/installation/](https://jekyllrb.com/docs/installation/)

Jekyll은 ruby를 기반으로 한다. 따라서 로컬 환경에 ruby와 gem (ruby의 패키지 관리자)이 설치되어 있어야 한다.  
난 맥북을 사용하기 때문에 macOS 설치 과정을 따랐다.

  
## ruby 설치
맥에는 기본적으로 루비가 설치되어 있다. 하지만 최신버전이 아니니까 최신버전을 설치!  
이를 위해 Homebrew 부터 설치한다.  
  
### homebrew 설치 (macOS의 패키지 매니저 - ruby를 설치하기 )  
참고: [https://brew.sh/](https://brew.sh/)

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### ruby 설치  
참고: [https://www.ruby-lang.org/ko/documentation/installation/#homebrew](https://www.ruby-lang.org/ko/documentation/installation/#homebrew)
```
brew install ruby
```

  
## jekyll 설치
rubygem을 이용하여 jekyll과 bundler를 설치한다.
```
gem install jekyll bundler
```
  
## jekyll project 생성
```
jekyll new username.gitlab.io
```
github을 이용하고 싶다면 `username.github.io` 를 사용하면 된다. 여기서 username은 gitlab이나 github의 본인 계정명.  
후에 `https://username.gitlab.io/` 또는 `https://username.github.io/` 로 접근할 수 있어야 하기때문에 Repository 명도 이렇게 맞추는 것이 좋다.

  
## Local에서 jekyll 사이트 돌려보기
참고: [https://www.bytesandwich.com/jekyll/software/blogging/2016/09/14/how-does-jekyll-work.html](https://www.bytesandwich.com/jekyll/software/blogging/2016/09/14/how-does-jekyll-work.html)


## Posting 작성
`_post` 폴더 내에 `YYYY-MM-DD-글제목.md`로 파일을 저장하면 된다. 파일 내용은 일정한 양식을 따라야 하는데, 기본 생성되어 있는 파일을 확인하면 도움이 될 것이다.

### 사이트 생성
```
bundle exec jekyll build
```
프로젝트 폴더 내에 `_site` 폴더를 생성해준다. 이 폴더만으로도 하나의 웹사이트가 완성되는 것이다.

### 웹서버 실행
```
bundle exec jekyll serve
```
프로젝트 폴더 내에 `_site` 폴더를 생성해주고, 웹서버를 실행한다. 브라우져에서 [http://127.0.0.1:4000](http://127.0.0.1:4000)를 통해 현재 상태의 사이트를 열어볼 수 있다.  
위 명령어 대신 `bundle exec jekyll serve --watch`를 실행하면 포스트를 작성하면서 변경내용이 자동으로 반영(빌드)되도록 할 수 있다.


# Gitlab-pages 설정하여 [https://username.gitlab.io/](https://username.gitlab.io/)에 블로그 호스팅

## Repository 설정
일단 Project(Repository)를 생성하고, 변경내용을 commit/push 한다.

* 새로운 Project 생성: 이름은 `username.gitlab.io`
* Gitlab > Project > Settings > CI/CD 에서 Runners settings 확인: Shared Runners가 활성화 되어 있어야 한다.


## Local project 추가 설정

Gitlab의 CI (Continuous Integration) 기능을 이용하기 위한 설정
Project 폴더 내에 `.gitlab-ci.yml` 파일을 만든다. 내용은..
```
image: ruby:2.3

variables:
  JEKYLL_ENV: production

before_script:
  - bundle install

test:
  stage: test
  script:
  - bundle exec jekyll build -d test
  artifacts:
    paths:
    - test
  except:
  - master

pages:
  stage: deploy
  script:
  - bundle exec jekyll build -d public
  artifacts:
    paths:
    - public
  only:
  - master
```


## Local project 내 config 설정

`_config.yml` 파일을 열어 몇가지를 변경한다. 제일 중요한건 `baseurl`, `url` 항목. 나머지는 취향에 맞게 변경하면 된다. 

```
baseurl: ""
url: "https://username.gitlab.io"
```



## Local project와 repository 연결하기
```
> git remote add origin https://gitlab.com/username/username.gitlab.io.git

> git add .

> git commit -m "Initial commit"

> git push -u origin master
```

push까지 완료하면 곧 gitlab의 CI 기능이 실행되며 jekyll build 가 실행된다. 빌드가 완료되면 [https://username.gitlab.io](https://username.gitlab.io) 를 이용해 사이트를 볼 수 있게 된다.


# Github-pages 이용하기
참고: [https://angrypark.github.io/starting-my-blog/](https://angrypark.github.io/starting-my-blog/)  
참고: [https://brunch.co.kr/@hee072794/39](https://brunch.co.kr/@hee072794/39)

Github에서는 별도의 CI 설정이 필요없다..... (아니! 그럼 그냥 github으로 할걸!!)



# 맺음말
CSS 는 손을 좀 봐야할 것 같다. 기본 태그에 대한 스타일이 조금 맘에 안드네. margin 정도만 맞춰줘도 될 듯.

Git, CI에 관해서는 별도로 다룰 일이 있을 것 같다. 
