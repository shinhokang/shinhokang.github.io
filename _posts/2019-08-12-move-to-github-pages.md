---
layout: post
title: 'Blog Gitlab에서 Github pages로 이주'
author: shinho.kang
date: 2018-08-12 18:00
tags: [GitHub, GitLab, Pages, Jekyll]
---

# GitLab Pages 에서 GitHub Pages로 호스팅 이주. 왜?

[Jekyll, Gitlab(또는 Github)을 이용한 블로그 개설](https://shinhokang.github.io/2018/06/03/start-jekyll-blog-in-gitlab-pages.html) 에서 작성한 바와 같이, 본 블로그의 호스팅은 GitLab Pages를 통해 하고 있었다. 그리고 한동안 개인거취의 문제로 잊고 있었는데, 금일 또 다른 포스팅 [독일 회사 개발자 - 한국에서 원격근무하기 (Remote Working)](https://shinhokang.github.io/2018/08/12/remote-work-in-korea.html)을 올리고나서 GitHub으로의 이주를 결심했다.

일단 배포가 되질 않았다. 

GitLab에서는 코드가 푸쉬되면, CI/CD 과정을 통해서 설정된 Pipelines 내에서 빌드와 배포가 이루어진다. 이 과정에서 GitLab Page로 빌드된 페이지들이 배포되어야 하는데, 한참을 기다려도, 아무리 다시 푸쉬를 해봐도, 아무리 CI/CD Job을 다시 실행시켜 보아도 배포가 되지 않았다.

Issue Tracker를 보니 이에 대한 다양한 이슈제기들이 있었지만, 제대로 해결되었다는 답변은 찾지 못했다.
그래서 GitHub으로 옮겼다.


# GitHub과 GitLab의 차이점

앞서 설명했듯이 GitLab에서는 CI 과정을 통해 빌드된 코드를 Pages에 배포해야 한다. 그래서 유저 입장에서는 CI 설정파일 `.gitlab-ci.yml`을 직접 편집해야 했다. 

그러나 Github에서는 그런 과정이 필요없다. GitHub과 GitHub Pages가 알아서 해준다. Repository의 이름만 제대로 설정되어 있으면 된다. 게다가 반영속도도 상당히 빠르다. 이것만으로도 옮길 이유는 충분하다. ㄷㄷ

본 블로그의 주소는 이제 [https://shinhokang.github.io](https://shinhokang.github.io) 다.
