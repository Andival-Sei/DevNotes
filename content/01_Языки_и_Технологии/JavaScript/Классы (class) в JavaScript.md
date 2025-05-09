
**Класс** — это расширяемый шаблон кода для создания объектов, который устанавливает в них начальные значения (свойства) и реализацию поведения (методы). Классы делают код более структурированным и удобным для реализации объектно-ориентированного подхода.

## Основные понятия
- Классы появились в стандарте ECMAScript 2015 (ES6).
- Класс — это разновидность функции.
- Классы позволяют создавать множество однотипных объектов (например, пользователей, товары и т.д.).
- Классы используют прототипное наследование под капотом.

---

## Синтаксис класса

```js
class MyClass {
  // свойства класса (новый синтаксис)
  prop = value;
  // конструктор
  constructor(...) {
    // инициализация объекта
  }
  // методы класса
  method1() { ... }
  method2() { ... }
  // геттер
  get something() { ... }
  // сеттер
  set something(value) { ... }
  // статический метод
  static staticMethod() { ... }
  // метод с вычисляемым именем
  [Symbol.iterator]() { ... }
}
```

- Для создания объекта используется `new MyClass()`.
- В конструкторе инициализируются свойства объекта.
- Методы класса записываются в прототип.

---

## Пример: базовый класс

```js
class User {
  constructor(name) {
    this.name = name;
  }
  greeting() {
    console.log(`Hello, I am ${this.name}`);
  }
  greet = () => {
    // стрелочная функция сохраняет контекст this
    console.log(`Hello, I am ${this.name}`);
  }
}
const userAlex = new User("Alex");
userAlex.greeting(); // Hello, I am Alex
const userOlga = new User("Olga");
userOlga.greet(); // Hello, I am Olga
console.log(typeof User); // function
```

---

## Класс как функция

- Класс — это функция, а методы класса — это методы прототипа.
- Класс нельзя вызвать без `new`:

```js
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}
alert(typeof User); // function
// User(); // Ошибка: Class constructor User cannot be invoked without 'new'
```

---

## Функция-конструктор и class: сравнение

```js
// Функция-конструктор
function UserFnClass(name) {
  this.name = name;
  this.greeting = function () {
    console.log(`Hello, I am ${this.name}`);
  }
}
UserFnClass.prototype.greet = function () {
  console.log(`Hello, I am ${this.name}`);
}
const userFnVlad = new UserFnClass("Vlad");
userFnVlad.greeting();
userFnVlad.greet();
```

- Класс — это синтаксический сахар над функцией-конструктором и прототипом, но с рядом отличий:
  - Класс нельзя вызвать без `new`.
  - Методы класса не перечисляются в цикле for..in.
  - Класс всегда работает в строгом режиме (`use strict`).

---

## Геттеры и сеттеры, приватные поля

```js
class User1 {
  profession = "software engineer";
  #skills = ""; // приватное поле
  constructor(name) {
    this.name = name;
  }
  get skills() {
    return this.#skills;
  }
  set skills(newSkills) {
    if (typeof newSkills !== "string") return;
    this.#skills = newSkills;
  }
}
const userAnna = new User1("Anna");
console.log(userAnna.profession);
userAnna.profession = "developer";
console.log(userAnna.profession);
console.log(userAnna.skills);
userAnna.skills = "html, css, js, ts, react";
userAnna.skills = 111;
console.log(userAnna.skills);
```

---

## Статические методы и свойства

```js
class User2 {
  static #instanceCount = 0;
  constructor(name) {
    this.name = name;
    User2.#instanceCount++;
  }
  greeting() {
    console.log(`Hello, I am ${this.name}`);
  }
  static getInstanceCount() {
    return User2.#instanceCount;
  }
}
const userKen = new User2("Ken");
userKen.greeting();
const userNatalia = new User2("Natalia");
userNatalia.greeting();
console.log(User2.getInstanceCount()); // 2
```

---

## Class Expression

- Классы можно определять как выражения (анонимные или с именем):

```js
let User = class {
  sayHi() {
    alert("Привет");
  }
};
```

- Имя класса, заданное внутри выражения, видно только внутри класса:

```js
let User = class MyClass {
  sayHi() {
    alert(MyClass); // видно только внутри класса
  }
};
new User().sayHi();
// alert(MyClass); // ошибка
```

---

## Динамическое создание классов

```js
function makeClass(phrase) {
  return class {
    sayHi() {
      alert(phrase);
    }
  };
}
let User = makeClass("Привет");
new User().sayHi(); // Привет
```

---

## Итоги
- Классы — современный способ создавать объекты и реализовывать наследование в JavaScript.
- Класс — это функция, методы класса — это методы прототипа.
- Классы поддерживают приватные поля, геттеры/сеттеры, статические методы и свойства.
- Классы нельзя вызвать без new, методы класса не перечисляются в for..in, всегда строгий режим.
- Классы делают код более структурированным и читаемым. 