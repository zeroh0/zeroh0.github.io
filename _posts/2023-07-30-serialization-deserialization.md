---
layout: post
title: 직렬화와 역직렬화
description: 프로그래밍에서 직렬화와 역직렬화는 무엇인가?
tags: [cs]
---

# 직렬화와 역직렬화

직렬화와 역직렬화는 객체를 **쉽게 저장하고 전송하고 재구성할 수 있도록 하는** 프로그래밍의 두 가지 중요한 개념입니다.  
데이터베이스에 객체 저장, 네크워크를 통해 객체를 전송하거나 메모리에 캐싱과 같은 다양한 상황에서 사용됩니다.

## 직렬화

객체에는 identity, state, behavior 세 가지 기본 특성이 있습니다.  
**state는 객체의 값 또는 데이터를 나타냅니다.**

직렬화는 **객체의 state를 byte stream으로 변환**하는 프로세스입니다.  
변환된 byte stream을 파일에 저장하거나 네트워크를 통해 전송하거나 데이터베이스에 저장할 수 있습니다.  
byte stream은 객체의 state를 나타내며 추후에 객체의 새 복사본 객체를 만들기 위해 재구성할 수 있습니다.

직렬화를 사용하면 객체와 관련된 데이터를 저장하고 새로운 위치에 객체를 다시 생성할 수 있습니다.

<figure>
    <img src="https://github.com/zeroh0/TestLab/assets/89443479/16acd188-88aa-4321-9a80-c84eb9a64c2f" alt="serialization-process">
    <figcaption align="center">
        직렬화의 프로세스
    </figcaption>
</figure>

객체를 직렬화하려면 프로그래머가 먼저 형식을 결정한 다음 적절한 도구를 사용하여 객체를 해당 형식으로 변환해야 합니다.  

## 직렬화 형식

JSON, XML, binary와 같은 다양한 형식을 직렬화에 사용할 수 있습니다.  
JSON, XML은 사람이 읽을 수 있고 다른 시스템에서 쉽게 파싱할 수 있기 때문에 직렬화에 널리 사용되는 형식입니다.  
binary 형식은 일반적으로 텍스트 기반 형식보다 읽기 및 쓰기 속도가 빠르기 때문에 성능상의 이유로 사용됩니다.

## 역직렬화

역직렬화는 직렬화의 반대 프로세스입니다.  
**byte stream을 가져와서 다시 객체로 변환**하는 작업이 포함된다.  
Java에서는 `ObjectInputStream` 클래스를 사용하여 binary 형식을 역직렬화할 수 있으며 `Jackson` 라이브러리를 사용하여 JSON 형식을 파싱할 수 있습니다.

<figure>
    <img src="https://github.com/zeroh0/TestLab/assets/89443479/c703c4db-da8e-439e-adde-91c82f5721f2" alt="deserialization-process">
    <figcaption align="center">
        역직렬화의 프로세스
    </figcaption>
</figure>

---

- 출처
  - [What Are Serialization and Deserialization in Programming?](https://www.baeldung.com/cs/serialization-deserialization)