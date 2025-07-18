---
title: "Rust 학습 일지 5"
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

### trait

이 놈의 트레이트... 지금까지 봤던 것과는 다르게 문법도 특이하고 개념도 다소 생소하다. 일단 핵심 키워드는 interface 인 것 같다. 찾아보니 Java 나 C#, Go 에서 비슷한 개념이 존재하는 듯 하다.

#### Orphan Rule

trait 를 정의하고 구현할때는 **반드시** 대상 타입 또는 trait 둘 중에 하나는 현재 크레이트에 정의되어야 함.

#### 주요 쓰임새

  1. operator overloading (일반적 행동규칙을 새로운 타입에 추가)
  2. extension trait (기존 타입에 새로운 행동규칙을 추가)
  3. generic programming : type parameter 를 사용해서, 특정 trait을 구현하고 있는 (*trait bound*) 임의의 타입에 대해서 공통적으로 작동하는 메소드를 구현하는 것을 목표로 하는 것 같다.

#### Deref coersion

``` rust
impl Deref for String {
    type Target = str;
    
    fn deref(&self) -> &str {
        // [...]
    }
}
```

> *컴파일러는 T 타입의 참조가 필요할 때, U 타입의 참조를 가지고 있다면 U가 Deref<Target = T> 트레이트를 구현했는지 확인합니다. 구현했다면, U를 T로 자동으로 역참조하여 변환해 줍니다.*
> *by Gemini*

참조자에 대한 자동 타입 변환 장치인 듯하다. 스마트 포인터에 사용하는 것이 가장 적절하다고 한다.학기 중에 프로젝트 한다고 C++ 공부할때 스마트 벡터인지 스마트 메모리인지 그런게 있었던 것 같은데 (자동으로 메모리 해제되는) 그것이랑 비슷한 건지는 잘 모르겠다.

#### Marker trait, dynamically sized trait (DST)

str 은 DST 로 컴파일 타임에 사이즈를 알 수 없다. (Sized trait 미구현) 반면, &str 타입은 사이즈가 존재한다. (8<길이>+8<포인터>=16 바이트)

#### 예시 정의들

##### trait bounds

``` rust
fn print_if_even<T>(n: T)
where
    T: IsEven + Debug
//  ^^^^^^^^^^^^^^^^^
//  This is a `where` clause
{
    // [...]
}
```

##### traits with concret type

``` rust
trait IsEven {
    fn is_even(&self) -> bool;
}

impl IsEven for i32 {
    fn is_even(&self) -> bool {
        self % 2 == 0
    }
}

impl IsEven for i64 {
    fn is_even(&self) -> bool {
        self % 2 == 0
    }
}

// Etc.
```

