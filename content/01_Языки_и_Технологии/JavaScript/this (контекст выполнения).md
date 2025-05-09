# Ключевое слово `this` в JavaScript

**this** — это специальное ключевое слово, которое ссылается на текущий объект, в контексте которого выполняется код. Контекст определяется не только местоположением, но и способом вызова функции.

## Основные случаи использования `this`

### 1. Глобальный контекст
- В глобальной области видимости (this вне функции):
  - В браузере `this` ссылается на объект `window`.
  - В Node.js — на объект `global`.

```js
console.log(this); // В браузере: window, в Node.js: global
```

---

### 2. В методах объекта
- Если `this` используется в методе объекта, оно указывает на сам объект.

```js
const person = {
  name: "Alex",
  sayHello: function() {
    // this указывает на объект person
    console.log(`Hello, ${this.name}`);
  }
};
person.sayHello(); // Выведет "Hello, Alex"
```

---

### 3. Контекст `this` в стрелочных функциях
- Стрелочные функции не имеют собственного `this`. Вместо этого `this` берётся из окружающего контекста (лексического окружения).

```js
const person1 = {
  age: 33,
  sayAge: () => {
    // this здесь не указывает на person1, а берётся из внешнего контекста
    console.log(`Age: ${this.age}`);
  },
  sayAgeWithContext: function() {
    // this указывает на person1
    console.log(`Age: ${this.age}`);
  }
};
person1.sayAge(); // Выведет "Age: undefined"
person1.sayAgeWithContext(); // Выведет "Age: 33"
```

---

### 4. Контекст `this` в событиях (DOM)
- В обработчике события `this` ссылается на элемент, который вызвал событие.

```js
// <button id="btn">Click me</button>
document.getElementById('btn').onclick = function() {
  // this указывает на кнопку
  this.textContent = 'Нажато!';
};
```

---

### 5. Методы call, apply и bind
- Позволяют вручную задать, каким будет значение `this` внутри функции.
  - `call` — вызывает функцию с определённым `this` и аргументами по отдельности.
  - `apply` — вызывает функцию с определённым `this`, аргументы передаются массивом.
  - `bind` — возвращает новую функцию с определённым `this`, которую можно вызвать позже.

```js
function greet(greeting, emoji) {
  // this.name будет взято из переданного объекта
  console.log(`${greeting}, ${this.name} - ${emoji}`);
}
const person = { name: "Alex" };
greet.call(person, "Hello", "💥"); // "Hello, Alex - 💥"
greet.apply(person, ["Hi", "💎"]); // "Hi, Alex - 💎"
const boundGreet = greet.bind(person);
boundGreet("Hey", "🟢"); // "Hey, Alex - 🟢"
```

---

### 6. Потеря контекста `this`
- Если передать метод объекта как отдельную функцию, `this` может потеряться и не будет указывать на исходный объект.

```js
const person1 = {
  age: 33,
  sayAgeWithContext: function() {
    console.log(`Age: ${this.age}`);
  }
};
const sayAge = person1.sayAgeWithContext;
sayAge(); // "Age: undefined" — потеря контекста
const boundSayAge = person1.sayAgeWithContext.bind(person1);
boundSayAge(); // "Age: 33" — контекст сохранён
```

---

## Итоги
- `this` — это ссылка на объект, в контексте которого вызвана функция.
- Контекст зависит от способа вызова функции.
- Стрелочные функции не имеют собственного `this`.
- Методы `call`, `apply`, `bind` позволяют управлять значением `this` вручную.
- Потеря контекста — частая ошибка, особенно при передаче методов как колбэков. 