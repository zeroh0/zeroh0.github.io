---
layout: post
title: fetch 에러처리
description: fetch 에러 핸들링
tags: [js]
---

# fetch 에러 핸들링

ajax, axios를 사용하지 않고 화면 단에서 간단하게 api를 확인하기 위하여 자바스크립트 기본 내장 비동기 통신 함수인 `fetch`를 사용해 보았다.

```js
const fetchBoardList = () => {
  fetch("/api/board", {
    method: "get",
  })
    .then((response) => {
      console.log(response);
      return response.json();
    })
    .catch((error) => {
      console.log(error);
    });
};
```

api 요청에 대한 작업이 정상적으로 완료됐을 시 then, 예외 발생 시 catch를 실행하는 함수를 작성하였다.  
api가 정상적으로 then을 탔을 경우를 확인한 뒤, catch를 타는지 확인하기 위해 서버에서 일부로 에러를 발생시켜 보았는데

![response](https://github.com/zeroh0/zeroh0.github.io/assets/89443479/78e1d3d0-ef44-4bef-94b9-af4f94fc7404)

예상과는 반대로 catch가 아닌 then을 타고 있는 걸 확인할 수 있었다.

![mdn-fetch](https://github.com/zeroh0/zeroh0.github.io/assets/89443479/93417985-5482-4d5e-bac6-d94c6b459dbe)

문서를 살펴본 결과 네트워크 오류가 있을 때만 reject 된다는 걸 확인할 수 있었다.  
그래서 추가로 response의 `ok`와 `status` 속성을 확인해야 한다고 한다.

해당 옵션으로 에러처리를 하면 다음과 같이 코드를 작성할 수 있다.

```js
const fetchBoardList = () => {
  fetch("/api/board", {
    method: "get",
  })
    .then((response) => {
      console.log(response);
      if (response.ok && response.status === 200) {
        return response.json();
      }
      throw Error("에러가 발생했습니다.");
    })
    .catch((error) => {
      console.log(error);
    });
};
```

아쉬운 점은 에러 발생 시 아래 이미지와 같은 에러 response를 내려줘도 사용할 수 없는 것이 나에게는 큰 단점으로 느껴졌다.

![error](https://github.com/zeroh0/zeroh0.github.io/assets/89443479/7ac901be-9608-4834-9c48-35ffb880c5da)

- 요약
  - 화면 단에서 정의한 에러 메시지만 사용 가능
  - 서버에서 에러 메시지를 내려줘도 사용 불가
