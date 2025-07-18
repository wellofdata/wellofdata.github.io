---
title: "Rust 학습 일지 6"
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

## 1. Copy, Clone trait

`Clone` trait는 일종의 deepcopy를 구현하는 트레이트이다. 다음 조건을 만족시키면 `Copy` trait 역시 구현할 수 있다.

### Copy trait 구현 가능 조건

1. 개체가 가지고 있는 메모리 할당량 이상의 힙 메모리, 파일 핸들러 등 추가 자원을 관리하지 않는다.
2. 타입이 가변 참조가 아니다 (mutable reference)
3. (supertrait인) Clone trait가 구현되어 있어야 한다.

`Copy` 가 구현되어 있으면 따로 `.clone()` 을 호출하지 않아도 암묵적으로 알아서 값을 복사(***bitwise copy***)한다.

### 일반적인 구현 방법

``` Rust
#[derive(Copy,Clone)]
struct Mytype {
    field: u32,
    field2: i16
    // ... and so on
}
```
## 2. 그 놈의 Result, Option

인풋 스트림에서 데이터를 읽어서 데이터 타입을 변환하는 과정이 생각보다 매우 복잡하다. 보통 다른 언어는 데이터 타입간의 변환이 간편하다. 예를 들어 파이썬에서는 `int()`, `str()` 함수를 사용하면 어지간한 경우에는 거의 다 정수와 문자 사이의 변환이 된다. 근데 이 놈의 Rust는 `parse()`, `read_line()`, `to_digit()` 등 여러가지 '형 변환' 메소드 함수의 출력이 `Option<_,_>` 또는 `Result<_,_>` 이다. 물론 오류 처리가 거의 반 강제라는 장점이자 단점이 있긴 한데 어쨌든 변환된 데이터를 실제로 사용하려면 거의 무조건 변수 바인딩 등의 과정이 필요하다. 메소드 마다 뭐가 `Option`을 반환하고 뭐가 `Result`를 반환하는지 구분하는 것도 쉽지 않다. (검색하면 나오긴 하지만)
