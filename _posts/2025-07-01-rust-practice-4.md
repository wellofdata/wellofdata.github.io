---
title: "Rust 학습 일지 4"
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

- `match` 의 경우 exhaustiveness 가 강제된다. 이때 `if let <pattern> = <expression> {sth1} else {sth2}` 구문을 사용해 볼 수 있다. 이 구문은 expression 이 pattern 에 매칭이 가능할 경우 그 값을 매칭한뒤 sth1 을 실행한다.
- `let <pattern> = <expression> else {sth2}` 구문의 경우 `if let...` 구문과 유사하지만, if 부분이 빠지면서 패턴에 매칭되면 실행되는 부분 (sth1) 이 사라지고 sth2 만 남아 패턴이 매칭되지 않았을때만 sth2가 실행된다. 대신 매칭 결과가 되는 값들을 binding 된 채로 바깥 scope 에 남긴다. 
- 함수 인자로 값이 전달되면 `Copy` trait이 구현되지 않은 데이터의 경우 소유권이 이전된다. 이때 이 데이터의 하위 데이터 (struct 의 field 등)만 따로 빼서 반환하고 싶다면 전체 데이터의 소유권을 가지고 있어야 한다. 즉, 참조자로 빌려온 구조체 인스턴스에 대해 하위 필드의 데이터에 대한 소유권을 부분적으로 변경할 수 없다.
- 프로필에 따라 overflow 는 패닉을 발생시키기도 하고, 그냥 wrap-around 되기도 한다.