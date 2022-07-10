# 15장 let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

ES5까지는 변수를 선언할 수 있는 유일한 방법이 var였습니다. var 키워드는 다음과 같은 문제점이 존재합니다.

- 변수 중복 선언 사용

```jsx
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

- 함수 레벨 스코프

```jsx
var x = 1;

if (true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

```jsx
var i = 10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i); // 5
```

- 변수 호이스팅

```jsx
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

의 문제점이 있습니다. 이를 보완하기 위해 ES6는 새로운 변수 선언인 let, const를 도입했습니다.

## let 키워드

### 1) 변수 중복 선언 금지

- var : 중복 선언 하여도 중복 선언된 변수 앞에 var가 없는 것 처럼 행동한다.
- let : 중복 선언을 금한다.

```jsx
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var foo = 456;

let bar = 123;
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 2) 블록 레벨 스코프

- var : 함수 코드 블록만을 지역 스코프로 인정한다.
- let : 모든 코드 블록(if, for, 함수 등)을 지역 스코프로 인정한다.

```jsx
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

### 3) 변수 호이스팅

- var : 변수 호이스팅이 발생한다.
- let : 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

let 키워드로 선언한 변수는 “선언 단계”와 “초기화 단계”가 분리되어 진행된다. 즉 변수 호이스팅에 의해 선언은 런타임 이전에 되지만, 초기화는 선언문에 도달했을 때 실행되기 때문에 초기화 이전에 접근하려고 하면 참조 에러가 발생한다.

![Untitled](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd56a1baa-4449-4191-8de1-4506493d2a3d%2FUntitled.png?table=block&id=5cd8753e-7b33-4986-9819-19d83b4dc093&spaceId=2ad0c350-f7c4-49fc-8e10-0df09d3d599e&width=2000&userId=f7809fb4-8b68-4ac4-940d-e69a299a85e6&cache=v2)

- 일시적 사각지대 : 스코프 시작 지점부터 변수를 참조할 수 없는 구간

### 4) 전역 객체와 let

- var : 전역 변수, 전역 함수로 선언하면 window 객체의 프로퍼티가 된다.
- let : 전역 변수, 전역 함수로 선언하면 window 객체의 프로퍼티가 아니다.

```jsx
// 이 예제는 브라우저 환경에서 실행해야 한다.
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x); // undefined
console.log(x); // 1
```

## const 키워드

const 키워드의 특징은 let과 거의 동일합니다. let과 다른 점을 중심으로 살펴보겠습니다.

### 1) 선언과 초기화

const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 합니다. 그렇지 않으면 에러가 발생합니다.

```jsx
const foo; // SyntaxError: Missing initializer in const declaration
```

const 키워드도 let 키워드와 마찬가지로 블록 레벨의 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작합니다.

### 2) 재할당 금지, 상수

const 키워드는 재할당이 금지됩니다. 따라서 상수를 표현하는 데 사용하기도 합니다.

const 키워드로 선언된 값을 변경할 수 있는 방법은 없습니다.

상수로 표현하려면 이름을 대문자로 표현해주는 것이 좋습니다.

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.

//세율을 의미하는 0.1은 변경할 수 없는 상수이다.
// 대문자로 상수임을 알려주자.
const TAX_RATE = 0.1;
```

### 3) const 키워드와 객체

const 키워드로 선언된 변수에 객체를 할당하면 값을 변경할 수 있습니다. 객체는 메모리 재할당 없이 변경할 수 있기 때문입니다. 즉, const 키워드는 재할당을 금지할 뿐 불변을 의미하진 않습니다.

```jsx
const person = {
  name: "Lee",
};

// 객체는 변경 가능한 값이다. 따라서 재할당없이 변경이 가능하다.
person.name = "Kim";

console.log(person); // {name: "Kim"}
```

## var vs let vs cost

- ES6를 사용한다면 `var` 키워드는 사용하지 않는다.
- 재할당이 필요하면 `let` 키워드를 사용한다. 변수의 스코프는 최대한 좁게 만든다.
- 읽기 전용으로 사용(변경이 필요없는) 하는 원시 값과 객체에는 `const` 를 사용한다.
