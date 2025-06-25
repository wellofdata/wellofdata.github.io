---
title: "Rust 학습 일지 2"
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

### 소유권 (ownership)

1. `Copy` trait 이 구현된 data type 들은 보통 stack 에 저장이 되고, 변수에 할당되거나  argument로 함수에 전달되었을때 소유권 이전 발생없이 복사된다. e.g.) i64, char, boolean, f64, tuple with only Copy triats
2. 반면, `Drop` trait 이 구현된 경우 함수 할당이나 함수에 인자로 전달되었을때 소유권 이전 (이동)이 발생하게 된다. 따라서 값에 binding 되어 있던 기존의 변수를 소유권 이전 후에 다시 사용하려하면 에러가 발생한다.

    e.g.)

    ``` rust
    let s = String::from("hello"); // String type has Drop trait
    let t = s; // ownership is moved from s to t

    println!(s); //Error since the ownership was moved.
    ```

3. `String` 과 string literal(`&str`) 은 다르다. `String`은 기본적으로 힙 메모리에 저장되고 `&str` 은 string slice로써 일종의 포인터로 저장된다. 함수에 인자로 전달하거나 할 때 참조를 통해 전달하면 유용할 때가 많다.