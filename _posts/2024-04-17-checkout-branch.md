---
layout: post
title: cannot lock ref 'new branch' 'target branch' exist; cannot create 'new branch'
description: 새로운 브랜치 생성하던 중 발생한 문제
tags: [git]
---

`develop` 브랜치에서 `develop/jangdongho` 라는 브랜치를 새롭게 생성하던 중 에러가 발생하였다.

![image](https://github.com/zeroh0/zeroh0.github.io/assets/89443479/b2bfb6a3-20b2-41b8-86e3-37edd164d257)

특정 브랜치에 하위 브랜치를 생성하려고 시도한 경우 이런 에러가 발생한다고 한다.  
예를 들어 `dev` 라는 브랜치가 존재할 때 `dev/newbranch` 같이 하위 브랜치를 생성할 수 없다고 한다.

앞으로 해결방법으로는 크게 두가지로 나눌 수 있겠다.

1. 불필요한 브랜치면 삭제 후 브랜치 생성
2. 특정 브랜치에 하위 브랜치를 생성하는 규칙을 가지지 않는 것

---

- 참고
  - [[ Git ] git cannot lock, cannot lock ref, cannot create 에러](https://goddaehee.tistory.com/351)
