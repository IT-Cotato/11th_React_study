### **1. this 바인딩이란?**

<aside>
❓

 JavaScript에서 `this` 키워드를 사용할 때, **그 값이 어디를 참조할까?**

`this`는 실행되는 **문맥(Context)** 에 따라 동적으로 결정되며, 특정 객체를 가리키게 됨

</aside>

---

### **2. this가 결정되는 방식**

**(1) 전역 실행 컨텍스트에서의 this**

JavaScript 코드가 **전역에서 실행될 때** `this`는 **전역 객체**를 가리킴

브라우저 환경에서는 `window` 객체가 되고, Node.js에서는 `global` 객체가 됨

```jsx
console.log(this); // 브라우저에서는 window
```

즉, 전역 실행 컨텍스트에서는 this가 **window**를 가리킴

---

**(2) 객체의 메서드에서의 this**

객체 안에서 메서드를 호출하면, `this`는 **그 객체를 참조함**

```jsx
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name);
  },
};
obj.greet(); // "Alice"
```

여기서 `this.name`은 `obj` 객체의 `name` 속성을 참조하기 때문에 **"Alice"** 가 출력

즉, **메서드를 호출한 객체가 this가 된다**는 원칙이 적용

---

**(3)  생성자 함수에서의 this**

생성자 함수에서는 `this`가 **새로 생성된 객체(인스턴스)** 를 가리킴

```jsx
function Person(name) {
  this.name = name;
}
const p1 = new Person("Bob");
console.log(p1.name); // "Bob"
```

여기서 `new Person("Bob")`을 실행하면 `this`는 새롭게 생성된 `p1` 객체를 가리키게 됨

따라서 `p1.name`을 출력하면 `"Bob"`이 나오는 것!

---

**(4) 화살표 함수에서의 this**

화살표 함수는 일반 함수와 다르게 **자신만의 this를 가지지 않음**

대신 **자신을 포함하는 외부 스코프의 this를 상속**받음

```jsx
const arrowFunc = () => {
  console.log(this);
};
arrowFunc(); // window (부모 스코프를 따름)
```

이 코드에서 `arrowFunc`는 **전역 실행 컨텍스트에서 실행되었기 때문에** `this`는 `window`를 가리킴

즉, **화살표 함수 내부의 this는 자신이 선언된 환경의 this를 그대로 사용**한다는 점이 중요

---

### **정리하자면…**

- **전역 컨텍스트에서는** `this`가 `window` (또는 Node.js에서는 `global`)
- **객체 메서드에서는** `this`가 **해당 객체**
- **생성자 함수에서는** `this`가 **새로 생성된 인스턴스**
- **화살표 함수에서는** `this`가 **외부 스코프를 상속**
