## var / let / const

| 키워드| 중복 선언| 재할당| 특징 및 주의사항| 스코프 유형|
|-----|--------|-----|-------------|----------|
| `var`   | ✅ 가능   | ✅ 가능  | - 함수 레벨 스코프를 가짐<br>- 호이스팅 시 `undefined` 할당됨<br>- 동일 변수명 중복 선언 가능하지만, 유지보수에 비효율적 | 함수 레벨 스코프 |
| `let`   | ❌ 불가   | ✅ 가능  | - 블록 레벨 스코프를 가짐<br>- 재할당 가능하지만 중복 선언 불가능<br>- `var`보다 안전하며, 변수 변경이 필요한 경우 사용 | 블록 레벨 스코프 |
| `const` | ❌ 불가   | ❌ 불가  | - 블록 레벨 스코프를 가짐<br>- 선언과 동시에 초기화 필수<br>- 상수 선언 시 사용되며, 객체의 속성 변경은 가능 | 블록 레벨 스코프 |
------------------


## 스코프(Scope)

### 1. 함수 레벨 스코프 (Function Scope)
- `var` 키워드는 함수 내부에서 선언될 경우, 해당 함수 내에서만 접근 가능합니다.
- 함수 외부에서 접근할 수 없지만, 함수 내 어디에서든 사용할 수 있습니다.

```js
function example() {
    var x = 10;
    console.log(x); // 10
}
console.log(x); // ReferenceError: x is not defined
```

### 2. 블록 레벨 스코프 (Block Scope)
- `let`과 `const`는 {} 중괄호(블록) 안에서만 유효합니다.
- 블록을 벗어나면 접근할 수 없습니다.

```js
{
    let y = 20;
    console.log(y); // 20
}
console.log(y); // ReferenceError: y is not defined
```
------------------


## 호이스팅
- 변수 선언 방식에 따라 호이스팅(코드 실행 시 변수와 함수 선언을 최상단으로 끌어올리는 현상)의 동작 방식이 다릅니다.
- ✅ `var`는 호이스팅 시 `undefined` 값이 할당됩니다.
- ❌ `let`과 `const`는 선언 전 접근이 불가능하여 `ReferenceError`가 발생합니다.

```js
console.log(a); // undefined
var a = 5;

console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 10;
```

- 함수 선언 방식에 따라 호이스팅 방식이 다릅니다.
- ✅ 함수 선언문 (Function Declaration)
    - 함수 선언문은 호이스팅 시 전체 함수가 메모리에 저장되므로, 선언 전에 호출이 가능합니다.
- ❌ 함수 표현식 (Function Expression)
    - 함수 표현식은 변수에 할당되는 방식이므로 변수의 호이스팅 규칙을 따릅니다.
    - `var`를 사용하면 `undefined`로 초기화되므로, 호출 시 `TypeError`가 발생합니다.
    - `let`과 `const`는 선언 전에 접근할 수 없으므로 `ReferenceError`가 발생합니다.

```js
sayHello(); // "Hello, world!"

function sayHello() {
    console.log("Hello, world!");
}

greet(); // TypeError: greet is not a function
var greet = function() {
    console.log("Hi!");
};

greetLet(); // ReferenceError: Cannot access 'greetLet' before initialization
let greetLet = function() {
    console.log("Hi!");
};
```
------------------


## const와 객체(배열)의 변경
- `const`로 선언된 객체와 배열은 재할당이 불가능하지만, 내부 속성 변경은 가능합니다.

```js
const person = { name: "John", age: 30 };
person.age = 31; // ✅ 가능
person = { name: "Jane", age: 25 }; // ❌ TypeError 발생

const numbers = [1, 2, 3];
numbers.push(4); // ✅ 가능
numbers = [4, 5, 6]; // ❌ TypeError 발생
```

1. `var`는 지양
    - 호이스팅 문제와 함수 레벨 스코프 특성으로 인해 유지보수가 어려움.
2. `let`은 값이 변경될 가능성이 있을 때 사용
    - 반복문, 조건문 등에서 값이 변경될 경우 적절하게 활용.
3. `const`는 기본적으로 사용하고, 변경이 필요할 때만 let 사용
    - 불변성을 유지하는 것이 유지보수에 유리.
------------------

# DOM (Document Object Model)

## 1. DOM이란?
- **DOM(문서 객체 모델, Document Object Model)**은 HTML 문서를 메모리에 **트리 구조**로 표현한 객체 모델입니다.
- 웹 브라우저가 HTML 페이지를 해석하여 **각 요소를 객체로 변환하고 트리 형태로 구성**합니다.
- 이를 통해 **JavaScript에서 웹 페이지 요소를 동적으로 조작**할 수 있습니다.

---

## 2. DOM의 특징
1. **트리 구조(Tree Structure)**  
   - HTML 문서의 계층적 구조를 트리 형태로 표현합니다.  
   - 각 HTML 요소는 노드(Node)로 변환되며, 부모-자식 관계를 가집니다.
   
2. **JavaScript와 상호작용 가능**  
   - JavaScript를 이용해 DOM을 조작하여 **HTML 요소 추가, 변경, 삭제**가 가능합니다.
   
3. **브라우저가 DOM을 생성 및 관리**  
   - 웹 브라우저는 HTML을 해석한 후, DOM을 자동으로 생성합니다.
   
---

## 3. DOM 트리 구조 예시
다음과 같은 HTML이 있다고 가정합니다.

```html
<!DOCTYPE html>
<html>
<head>
    <title>DOM 예제</title>
</head>
<body>
    <h1 id="title">Hello, DOM!</h1>
    <p class="text">DOM이란 무엇인가?</p>
</body>
</html>

이 HTML 문서는 다음과 같은 DOM 트리 구조를 가집니다.

php-template
복사
편집
Document
│
├── <html>
│   ├── <head>
│   │   └── <title> "DOM 예제"
│   ├── <body>
│       ├── <h1 id="title"> "Hello, DOM!"
│       ├── <p class="text"> "DOM이란 무엇인가?"
```