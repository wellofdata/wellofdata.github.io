---
title: "Rust 학습 일지 3"
categories:
  - logs
tags:
  - life
  - 일상
  - rust
  - program
  - 학습
published: true
---

## 오늘 배운 것

- Attribute 라고 하면 보통 다른 언어에서는 class variable 을 의미했던 것 같은데, rust에서 attrubute는 `derive`처럼 특정 기능(trait)을 구조체에 추가하기 위해 사용하는 구조체 정의전 `#`과 함께 등장하는 요소다. 
- 구조체 내를 구성하는 변수들은 각각 다른 타입을 가질 수 있지만 가변/불변성은 구조체 전체에 적용되는 공유 특성이다. 또한 만약 구조체 내의 변수가 참조를 사용할 경우 lifetime을 함께 사용해 구조체가 사라지기 전에 구조체가 참조하는 값이 사라지지 않도록 해야 제대로 컴파일이 된다.
- 구조체 내에 따로 정의된 함수를 associated function 이라고 하는데, 이 중 method가 아닌 함수들은 self 를 인자로 가지지 않는다. 네임스페이스 내에 정의된 모듈과 동일하게 `::` 문법으로 호출한다.
- `None` 대신 unit `()` 이 존재한다. return이 없는 함수는 unit을 반환하는 것으로 처리된다.
- `None` 은 `Option<T>`라는 이름의 `enum` type 자료형의 Variant 중 하나다. (다른 하나는 `Some`)