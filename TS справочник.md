# Типы TypeScript

## Базовые типы

### Основные базовые типы данных. Краткая сводка.

1. boolean: `let isCompleted: boolean = false;`
2. string: `let stringVar: string = 'stringValue';` также можно определить шаблонную строку:
		`let stringVarTemplate: string = stringVar contains ${stringVar};`
3. number: `let numberVar: number = 42; let numberVar2: number = 0xf00d; let numberVar3: number = 0.5;`
4. undefined: `const undefinedVar: undefined = undefined;`
5. null: `const nullVar: null = null; //тип nullVar object, так как в JS null имеет тип object`
5. void: `const returnVoidFunction = (): void => {console.log('Hello')};`
6. Array:
	`let array: number[] = [1, 2, 3];
	let array2: Array<string> = ['hello', 'world']; //Generic type`
7. Tuple: используется для описания массивов со смешанными типами данных:
		`let x: [string, number] = ['hello', 10];`
8. Enum: тип перечисления, используется для набора ключей с известными значениями (набор именованных числовых констант, каждый элемент имеет свой индекс, при обращении к элементу enum возвращается его индекс):

```typescript
		enum Directions {
			Up,
			Down,
			Left,
			Right
		};
		Directions.Up; // получаем индекс 0
		Directions.Down; // получаем индекс 1
		Directions.Left; // получаем индекс 2
		Directions.Right; // получаем индекс 3
```

```typescript
		// переопределение enum:
		enum Directions {
			Up = 2,
			Down = 4,
			Left = 6,
			Right
		};
		Directions.Up; // получаем индекс 2
		Directions.Down; // получаем индекс 4
		Directions.Left; // получаем индекс 6
		Directions.Right; // получаем индекс 7, так как индекс следующего элемента с незаданным индексом, зависит от индекса предыдущего
```
9. Never: обычно используется совместно с функциями, обычно в следующих случаях:
		* функция возвращает ошибку и не заканчивает свое выполнение
		* функция, которая постоянно выполняется (например, бесконечный цикл)

```typescript
const msg = "hello";
const error = (msg: string): never => {
    throw new Error(msg);
};

// Function infinite loop
const infiniteLoop = (): never => {
    while (true) {
    }
};
```
10. Object: прездназначен для определения объекта или непримитива. Неспецифичный тип, не отражает конкретной структуры объекта.

### Основные методы работы с типами. Тип объединения.

если переменная может иметь несколько типов достаточно в их определении указать необходимые типы через оператор `|`:

```typescript
let x: boolean | number;
```
#### Пользовательские типы

пользовательский тип задается через ключевое слово `type`:

```typescript
type Name = string;

type TObjectSpecific = {
	someKey: Name
}

const obj: TObjectSpecific;
obj.someKey = 'someValue';
```

#### Enum подробнее

##### Подход Reverse enum

enum это некая смесь объекта и массива. При обращении к значению enum возвращается его индекс.
Иногда, помимо необходимости получения индекса, необходимо получить и само значение по ключу. Для этого
используется подход `Reverse enum`:


```typescript
		enum Directions {
			Up,
			Down,
			Left,
			Right
		};

		Directions[0]; // получаем ключ Up
		Directions[1]; // получаем ключ Down
		Directions[2]; // получаем ключ Left
		Directions[3]; // получаем ключ Right

// случай с переопределением индекса:
		enum DirectionsModified {
			Up = 2,
			Down = 4,
			Left = 6,
			Right = 8
		};

		Directions[2]; // получаем ключ Up
		Directions[4]; // получаем ключ Down
		Directions[6]; // получаем ключ Left
		Directions[8]; // получаем ключ Right

// случай со строковыми значениями индекса:
		enum links {
		    youtube = 'https://youtube.com/',
		    vk = 'https://vk.com/',
		    facebook = 'https://facebook.com/'
		}

		links.vk        // "https://vk.com/"
		links.youtube 	// "https://youtube.com/"
```

##### Константные перечисления

Если стоят требования к минимизации и оптимизации мощностей можно воспользоваться следующей конструкцией:

```typescript
// случай со строковыми значениями индекса:
		const enum links {
		    youtube = 'https://youtube.com/',
		    vk = 'https://vk.com/',
		    facebook = 'https://facebook.com/'
		}
		// данный код после прохождения через компилятор TypeScript не вернет ничего (JS код), если к links не будет далее никаких обращений
		// после данного выражения будет скомпилирован код на typescript, при этом нет генерации объекта
		const arr = [links.vk, links.facebook];

```

#### Функции

Рассмотрим функцию:

```typescript
const createPassword = (name, age) => `${name}${age}`;
```
Описание типов аргументов функции:

```typescript
const createPassword = (name: string, age: number | string) => `${name}${age}`;
// number | string тип объединения
```
Описание типов аргументов функции с дефолтными значениями:

```typescript
const createPassword = (name: string, age: number | string = 20) => `${name}${age}`;
```
Описание типов аргументов функции с опциональными аргументами:

```typescript
const createPassword = (name: string, age?: number) => `${name}${age}`;
// тут аргумент age является опциональным
```
Rest параметры

```typescript
const createSkills = (name, ...skills) => `${name}, my skils are ${skills.join()}`;
// описание типов функции с rest аргументом
const createSkills = (name: string, ...skills: Array<string>) => `${name}, my skils are ${skills.join()}`;
```
Также у функций нужно описывать типы возвращаемых значений, поэтому
финальное описание функции с возвращаемым типом значения будет следующим:

```typescript
const createPassword = (name: string, age: number | string): string => `${name}${age}`;

const createEmptyObject = (): object => ({});

const greetUser = (): void => {
    alert("Hello, nice to see you!");
};
```

##### Функция как тип


```typescript
let myFunc: (firstArg: string) => void;

// Describe function type with wrong return type
let myFunc2: (firstArg: string) => number;

function oldFunc(name: string):void {
    alert(`Hello ${name}, nice to see you!`);
};

myFunc = oldFunc
myFunc2 = oldFunc; //здесь будет ошибка TS: Type 'void' is not assignable to type 'number'
```

#### Объекты

Простой способ описания типа объекта:

```typescript
let user: { name: string, age: number } = {
    name: 'Yauhen',
    age: 30,
};
// Try to change property
let user: { name: string, age: number } = {
    name: 'Yauhen',
  	/*
      Error:
 	The expected type comes from property 'age'
 	which is declared here on type '{ name: string; age: number; }'
    */
    age: 'test',		// <--- Must be a number
};
```

Расширение объекта свойством

```typescript
let user: { name: string, age: number } = {
    name: 'Yauhen',
    age: 30,
    nickName: 'webDev', // Object literal may only specify known properties, and 'nickName' does not exist in type '{ name: string; age: number; }'
};
// ошибка так как в описании типа user не содержится поля nickName
// добавим к описанию новое свойство и ошибка уйдет:
let user2: { name: string, age: number, nickName: string} = {
    name: 'Yauhen',
    age: 30,
    nickName: 'webDev',
};
```

Более практично и элегантно вынести описание типа в отдельную сущность:

```typescript
// Using Type for objects structure
type Person = { name: string, age: number, nickName: string };

// Apply Person type for objects with the same structure
let user: Person = {
    name: 'Yauhen',
    age: 30,
    nickName: 'webDev',
};

let admin: Person = {
    name: 'Max',
    age: 20,
    nickName: 'Mad',
};
```

На практике нередко бывает, что разные сущности объектов имеют похожую структуру, но несколько отличный состав полей. Используя пример выше, покажем как на практике можно расширить тип Person:

```typescript
// Updating type with optional properties
type Person = {
    name: string,
    age: number,
    nickName?: string, // сделали поле опциональным
    getPass?: () => string, // сделали поле опциональным
};
```

##### Пример безопасной деструктуризации объекта

```typescript
const { bla, bla1 } = obj || {};
};
```

#### Классы

##### Способ описания типов внутри тела класса

```typescript
class User {

    name: string;
    age: number;
    nickName: string;

    constructor(name: string, age: number, nickName: string) {
        this.name = name;
        this.age = age;
        this.nickName = nickName;
    }

};

// конструктор вызывается при инициализации нового экземпляра класса
const yauhen = new User('Yauhen', 31, 'webDev');
```

##### Модификаторы доступа полей класса


```typescript
// Class types modificators
class User {

    public name: string; // значение по умолчанию, если не задано явно, свойство получает его автоматически
  	private nickName: string; // элемент с модификатором private не может быть доступен за пределами класса (в том числе классы-наследники и объекты-экземпляры класса)
    protected age: number; // доступ к элементу с модификатором protected могут получить только наследники класса, экземпляры класса доступа к этому свойству не имеют
    readonly pass: number; // элемент становится доступным только для чтения
    nameAlternative: string = 'Default'; // в описании класса также можно добавлять дефолтные значения

    constructor(name: string, age: number, nickName: string, pass: number) {
        this.name = name;
        this.age = age;
        this.nickName = nickName;
        this.pass = pass;
        this.nickName = nickName ? nickName : nameAlternative;
    }

}

const yauhen = new User('Yauhen', 31, 'webDev', 123);

yauhen.name;	    // "Yauhen"
yauhen.nickName;  // Prop 'nickName' is private and only accessible within class 'User'
yauhen.age;		    // Prop 'age' is protected and only accessible within class 'User' and its subclasses
yauhen.pass = 42; // Cannot assign to 'pass' because it is a read-only property

const user = new User('Yauhen', 20); // { name: "Yauhen", age: 20, nickName: "Default" }
```

##### Минимизация описания класса с использованием конструктора


```typescript
// Minimization of Class properties
class User {
// при таком подходе для каждого свойства обязательно нужно задавать модификатор
    constructor(
        public name: string,
        public age: number,
        public nickName: string,
        public pass: number
    ) {}

}
```

##### Использование аксессоров

При использовании приватных полей класса, к ним можно обратиться извне посредством методов класса, которые именуют геттеры и сеттеры. Они используются как правило для защиты от возможных ошибок, связанными с изменением полей, которые защищены, а также, если нужна дополнительная логика при получении или изменении защищенных свойств класса.

```typescript
// Minimization of Class properties
// Get access to private property
class User {

    private age: number = 20;

    constructor(public name: string) {}

    setAge(age: number) {
        this.age = age;
    }

    set myAge(age: number) {
        this.age = age;
    }
}

const yauhen = new User('Yauhen');

yauhen.setAge(30);	// 30
yauhen.myAge = 31;	// 31
```

##### Наследование | Inheritance

###### Статические свойства


```typescript
// Class with static property
class User {

    static secret = 12345;  // static property доступно только при обращении к самому классу, а не экземпляру

    constructor(public name: string, public age: number) {}

}

User.secret // обращаемся к статическому свойству класса
```

Также для получения доступа к статическому свойству класса можно использовать геттер:

```typescript
// Class with static property
class User {

    static secret = 12345;

    constructor(public name: string, public age: number) {}

    getPass(): string {
        return `${this.name}${User.secret}`;
    }

}

const yauhen = new User('Yauhen', 30);
yauhen.getPass();	// "Yauhen12345"
```

Наследование класса

```typescript
 // родительский класс
class User {

    private nickName: string = 'webDev';
    static secret = 12345;

    constructor(public name: string, public age: number) {}

    getPass(): string {
        return `${this.name}${User.secret}`;
    }

}

// наследование
class Yauhen extends User {

    name: string = 'Yauhen';

}

const max = new User('Max', 20);
const yauhen = new Yauhen(31);	// Ошибка: Expected 2 arguments, but got 1
```

Для исправления ошибки нужно в наследуемом классе описать конструктор с ключевым словом `super`:

```typescript
// Realization of constructor in the inherited class
class Yauhen extends User {

    name: string = 'Yauhen';

    constructor(age: number) {
        super(name, age); // вызывает конструктор родительского класса
    }

}

// No error
// Create instances based on 'User' and 'Yauhen' classes
const max = new User('Max', 20);
const yauhen = new Yauhen(31);
```

Также в дочернем классе можно переопределять методы:

```typescript
class Yauhen extends User {

    name: string = 'Yauhen';

    constructor(age: number) {
        super(name, age);
    }

    getPass(): string {
        return `${this.age}${this.name}${User.secret}`;
    }

}

const yauhen = new Yauhen(31);

yauhen.getPass(); // переопределенный метод getPass возвращает "31Yauhen12345"
```

##### Абстрактные классы

В TypeScript абстрактные классы это отдельная сущность по отношению к JS. Абстрактные классы это основные классы, от которых идет наследование.
Пример:

```typescript
// Abstract class example
abstract class User {

    constructor(public name: string, public age: number) {}

    greet(): void {
        console.log(this.name);
    }

    abstract getPass(): string;   // Abstract method его реализация обязательно должна быть описана в потомке

}

const max = new User('Max', 20);  // Cannot create an instance of an abstract class
```

Особенности:
1. Абстрактный класс содержит детали реализации своих элементов (свойств и методов).
2. От абстрактного класса нельзя создать экземпляр.

Пример наследования от абстрактного класса

```typescript
// Create class using Abstraction
class Yauhen extends User { // Non-abstract class 'Yauhen' does not implement inherited abstract member 'getPass' from class 'User'

    name: string = 'Yauhen';

    constructor(age: number) {
        super(name, age);
    }

}
// тут сразу получаем ошибку о том, что не реализован метод getPass
```

Исправление ошибки:

```typescript
// Realization of 'getPass' method
class Yauhen extends User {

    name: string = 'Yauhen';

    constructor(age: number) {
        super(name, age);
    }

    getPass(): string {
        return `${this.age}${this.name}`;
    }

}

// Call prototype method
yauhen.greet();		// "Yauhen"
// Call personal method
yauhen.getPass();	// "31Yauhen"
```

#### Пространства имен и модули

Пространство имен это глобальная сущность, которая инкапсулирует в себе необходимые данные: функции, переменные, классы и т.п.
Используется во избежание использования глобальных переменных и для оптимизации разработки. Пространство имен создает свою локальную область видимости.
Пример:

```typescript
// Define namespace
namespace Utils {

    const SECRET: string = '123321';
    const PI: number = 3.14;

    const getPass = (name: string, age: number): string => `${name}${age}`;

    const isEmpty = <T>(data: T): boolean => !data;

};
// если мы попробуем обратится к какой-нибудь константе Utils как к свойству объекта это вызовет ошибку
// Try to call method outside namespace
const myPass = Utils.getPass('Yauhen', 31);	// Property 'getPass' does not exist on type 'typeof Utils'
```

Чтобы получить доступ к данным namespace, их нужно экспортировать. Пример:

```typescript
// Export data from Namespace
namespace Utils {

    export const SECRET: string = '123321';
    const PI: number = 3.14; // данная константа остается приватной, так как не экспортируется
    // она не будет доступна из данного пространства имен

    export const getPass = (name: string, age: number): string => `${name}${age}`;

    export const isEmpty = <T>(data: T): boolean => !data;

};

// Calling exported from namespace methods
const myPass = Utils.getPass('Yauhen', 31);		// "Yauhen31"
const isSecret = Utils.isEmpty(Utils.SECRET);	// "false"
```

##### Инкапсулирование пространством имен нескольких файлов


```typescript
// Находится в отдельном файле "Utils.ts"
// Export
namespace Utils {

    export const SECRET: string = '123321';

    export const getPass = (name: string, age: number): string => `${name}${age}`;

};

// Получение доступа из другого файла (устаревший метод)
// File "Customers.ts"
// Import
/// <reference path="Utils.ts" />			// <--- Import здесь обязательно 3 слэша

// Calling "Utils" namespace method
const myPass = Utils.getPass('Yauhen', 31);	// "Yauhen31"

// рекомендовано использовать ES6 модули, которые создают свое пространство имен

// Import/Export (ES6 Modules)

// File "Utils.ts"
export const SECRET: string = '123321';

export const getPass = (name: string, age: number): string => `${name}${age}`;

// File "Customers.ts"
import { getPass, SECRET } from "./Utils";

const myPass = getPass(SECRET, 31);	// "Yauhen31"
```

#### Интерфейс

##### Основы

Интерфейсы помогают описать форму объекта или то, как он будет выглядеть
Определяется интерфейс следующим образом:

```typescript
// Simple interface example
interface User {
    name: string,
    age: number,
}
```

Принципиальная разница между `type` и `interface`: `type` задает псевдоним для любой разновидности типа, включая примитивы. `Interface` представляет собой всегда именованный тип объекта и обеспечивает мощный способ определения сущностей. Также `interface` может быть использован в выражениях `extends` или `implements`, то есть может наследоваться и расширяться другими интерфейсами.

Пример создания объекта с использованием интерфейса:

```typescript
interface User {
    name: string,
    age: number,
    option?: string,
}

const yauhen: User = {
    name: 'Yauhen',
    age: 31,
}
```

Также в интерфейсе можно использовать модификатор доступа:


```typescript
interface User {
    readonly name: string, // нельзя изменять
    age: number,
    option?: string,
}

const yauhen: User = {
    name: 'Yauhen',
    age: 31,
}

yauhen.age = 30;
yauhen.name = 'Max'; // Cannot assign to 'name' because it is a read-only property. Ошибка, так как пытаемся имзменить свойство,
// помеченное как только для чтения
```

##### Проверка на лишнее свойство


```typescript
interface User {
    name: string,
    age: number,
}

const yauhen: User = {
    name: 'Yauhen',
    age: 31,
  	/*
      Error:
      Object literal may only specify known properties, and 'nickName' does not exist in type 'User'
    */
    nickName: 'webDev',		// <--- Didn't described in interface "User" | Ошибка, так как свойство nickName отсутствует в интерфейсе User
}
```

##### Расширение свойств интерфейса


```typescript
// Interface extension
interface User {
    name: string,
    age: number,
    [propName: string]: any; // добавление строкового индекса позволяет добавлять новые свойства в объект с типом User
}

const yauhen: User = {
    name: 'Yauhen',
    age: 31,
    nickName: 'webDev',
    justTest: 'test',
}
```

##### Использование интерфейсов с классами

Интерфейсы также могут описывать классы. Класс, которые основан на определенном интерфейсе при его описании
должен содержать слово `implements` и далее сам интерфейс, который он реализует (имплементирует):

```typescript
// Class Interface
interface User {
    name: string,
    age: number,
    getPass(): string,
}

class Yauhen implements User {
    name: string = 'Yauhen';
    age: number = 31;
    nickName: string = 'webDev'; // в интерфейсе не описано поле nickName, при этом ошибки нет

    getPass() {
        return `${this.name}${this.age}`;
    }
}
```
При этом в реализуемом от интерфейса классе могут быть дополнительные поля, не описанные в интерфейсе, но набор данных в интерфейсе должен быть реализован.

Также можно создавать классы на основании нескольких интерфейсов:


```typescript
// Create Class based on multiple interfaces
interface User {
    name: string,
    age: number,
}

// Separate interface with one method
interface Pass {
    getPass(): string,
}

class Yauhen implements User, Pass {
    name: string = 'Yauhen';
    age: number = 31;

    getPass() {
        return `${this.name}${this.age}`;
    }
}
```

##### Расширение интерфейсов

Механизм расширения прост. Используется ключевое слово `extends` после описания какого-либо интерфейса, после чего указывается псевдоним интерфейса, от которого идет расширение:

```typescript
interface User {
    name: string,
    age: number,
}

interface Admin extends User {
    getPass(): string,
}
// класс Yauhen использует расширенный от User интерфейс Admin
class Yauhen implements Admin {
    name: string = 'Yauhen';
    age: number = 31;

    getPass() {
        return `${this.name}${this.age}`;
    }
}
```
Также поддерживается расширение от нескольких интерфейсов, в этом случае родительские интерфейсы перечисляются через запятую после слова `extends`.

## Generic types | Обобщенные типы

Базовый синтакс создания Generic типа:


```typescript
type MyType<T> = T; // после названия имени следуют угловые скобки которые и генерируют обобщенные типы

const MyType2: MyType<string> = 'test'; // применяем Generic тип к другому типу
```

Типы Generic используется для обобщения типов в общем виде. Они позволяют создавать компоненты, способные работать с различными типами.

Рассмотрим следующий пример:

```typescript
const getter = (data: any): any => data;
```
В приведенном выше примере используется тип `any`, который служит обобщением, позволяющим использовать аргумент абсолютно любого типа.
Однако такой подход считается плохой практикой при использовании TypeScript, так как в данном случае сама цель типизации теряется:
ведь никаких проверок выполняться не будет.

Вот какая проблема потери контроля над типами может вытечь из вышеприведенного примера:


```typescript
const getter = (data: any): any => data;
// на выходе мы хотим обратиться к свойству length возвращаемых данных, но в данных с типом number данное свойство не определено
getter(10).length;        // undefined
getter('test').length;    // 4
```

В некоторых случаях может возникнуть необходимость написания такой функции, тип результата которой будет зависеть от типа принимаемого аргумента. Пример ниже показывает как в таком случае можно использовать Generic:


```typescript
// Using of generic type
const getter = <T>(data: T): T => data;
// или с использованием ключевого слова function
function getter<T>(data: T): T {
    return data;
}
// при использовании Generic типа, получим следующую ошибку
getter(10).length;        // Property 'length' does not exist on type '10'
getter('test').length;    // 4
```

Вот как выглядит работа Generic наглядно:


```typescript
// Generic behavior explanation
// For a number
const getter = (data: number): number => data;
getter(10).length;        // Property 'length' does not exist on type '10'

// For a string
const getter = (data: string): string => data;
getter('test').length;    // 4
```

То есть в процессе компиляции компилятор сам определяет тип который нужно вернуть.

Данный процесс является контролируемым и мы можем строго задать тип данных, которые будет принимать функция, описанная с помощью Generic:


```typescript
// Описываем функции с помощью Generic
const getter = <T>(data: T): T => data;

// При вызове функции определяем тип аргумента
getter<number>(10).length;		  // Property 'length' does not exist on type '10'
getter<string>('test').length;	// 4
```

Еще пример манипуляции с Generic типами:


```typescript
// Описываем функции с помощью Generic
const mergeObjects = <T, R>(a: T, b: R): T & R => Object.assign({}, a, b); //возвращаемый тип можно опустить
// так как он вычисляется от Object.assign

const merged = mergeObjects({name: 'a'}, {age: 25})

console.log(merged.age); // здесь компилятор уже знает, что должно быть возвращено, так как Generic типы T и R вычисляются
// при вызове функции с определенным набором параметров

//если функция обязательно должна работать с объектами такой подход не подойдет и нужно специфицировать Generic типы

const mergeObjects2 = <T extends object, R extends object>(a: T, b: R): T & R => Object.assign({}, a, b);

```

Еще один полезные пример для работы с объектами и их ключами:


```typescript
// тип R расширяет набор ключей от T
function getObjectValue<T extends object, R extends keyof T>(obj: T, key: R) {
  return obj[key]
}

const person = {
  name: 'Vladilen',
  age: 26,
  job: 'typescript'
}

console.log(getObjectValue(person, 'name'))
console.log(getObjectValue(person, 'age'))
console.log(getObjectValue(person, 'job'))
```

### Generic types при работе с классами

Для придания гибкости описываемым классам используется следующий синтаксис:


```typescript
// Generic class
class User<T> {

    constructor(public name: T, public age: T) {}

    public getPass(): string {
        return `${this.name}${this.age}`;
    }

}
// теперь конструктор класса может принимать не только строки, но и числа
const yauhen = new User('Yauhen', '31');
const max = new User(123, 321);

yauhen.getPass();     // "Yauhen31"
max.getPass();        // "123321"
```

После имени класса в <> указывается Generic тип, далее описываем в конструктуре данный Generic тип

Если необходимо использовать несколько различных типов для конструктора класса, используется следующий синтаксис:


```typescript
// Multiple generic types
class User<T, K> {

    constructor(public name: T, public age: K) {}

    public getPass(): string {
        return `${this.name}${this.age}`;
    }

}

const yauhen = new User('Yauhen', '31');	// string, string
const max = new User(123, 321);				// number, number
const leo = new User('Leo', 22);			// string, number

yauhen.getPass();     // "Yauhen31"
max.getPass();        // "123321"
leo.getPass();        // "Leo22"
```

### Уточненное описание Generic типов

Например, для корректной работы метода класса, один из аргументов обязательно должен быть типом number, как в примере ниже:


```typescript
// Specify generic type 'K' with key-word 'extends'
class User<T, K extends number> {

    constructor(public name: T, public age: K) {}

    public getPass(): string {
        return `${this.name}${this.age}`;
    }

    public getSecret(): number {
        return this.age**2;
    }
}

const yauhen = new User('Yauhen', 31);
const leo = new User(123, 321);
/*
  Error:
  Argument of type '"20"' is not assignable to parameter of type 'number'
*/
const max = new User('Max', '20');
```

То есть, в данном случае Generic K расширяет тип `number`, что строго задает тип второго аргумента конструктора класса.

В качестве объяснения дженерик типов воспользуемся примером:


```typescript
function mergeObjects (a: object, b: object) {
	return Object.assign({}, a, b)
}

const merged = mergeObjects({name: 'Test'}, {age: 26})

console.log(merged.name); //здесь в TS будет ошибка, так как глобальный тип object не содержит поля name
```
Чтобы избежать подобного поведения можно использовать дженерик типы:


```typescript
function mergeObjects<T, R> (a: T, b: R) { // задаем дженерики T и R, данные типы подстраиваются под объекты
	return Object.assign({}, a, b)
}

const merged = mergeObjects({name: 'Test'}, {age: 26})

console.log(merged.name);
```

Используя дженерик-типы мы не привязываемся к конкретному объекту.

Если нужно указать, что мы работаем с объектом, можно использовать синтаксис:


```typescript
function mergeObjects<T extends object, R extends object> (a: T, b: R) { // в таком случае на вход ожидаются объекты
	return Object.assign({}, a, b)
}
```

Пример с расширением дженерик типов:

```typescript
function getObjectValue<T extends object, R extends keyof T>(obj: T, key: R) {
   return obj[key]
 }
 //R extends keyof T означает что дженерик тип R содержит в себе ключи типа T
 const person = {
  name: 'Vladilen',
  age: 26,
  job: 'typescript'
}
console.log(getObjectValue(person, 'name'))
console.log(getObjectValue(person, 'age'))
console.log(getObjectValue(person, 'job')) // если из person исключить поле job, будет ошибка
```
### Generic types с классами


```typescript
class Collection<T extends number | string | boolean> { // определяем дженерик тип T как один из примитивных типов
  constructor(private _items: T[] = []) {}

  add(item: T) {
    this._items.push(item)
  }

  remove(item: T) {
    this._items = this._items.filter(i => i !== item)
  }

  get items(): T[] {
    return this._items
  }
}

const strings = new Collection<string>(['I', 'Am', 'Strings'])
strings.add('!')
strings.remove('Am')
console.log(strings.items)
```


```typescript
interface Car {
  model: string
  year: number
}

function createAndValidateCar(model: string, year: number): Car {
  const car: Partial<Car> = {}

  if (model.length > 3) {
    car.model = model
  }

  if (year > 2000) {
    car.year = year
  }

  return car as Car
}
```
если функция не должна вносить изменения в аргумент можно воспользоваться директивой TS `Readonly`


```typescript
const cars: Readonly<Array<string>> = ['Ford', 'Audi']
// cars.shift() // из-за Readonly нельзя изменить исходный массив
// cars[1]

const ford: Readonly<Car> = {
  model: 'Ford',
  year: 2020
}
```

## Decorators | Декораторы

Декораторы это возможность TypeScript по добавлению аннотаций и метапрограммного синтаксиса для объявлений классов и функций.
По сути декоратор это обычная функция, которая может быть прикреплена к объявлению класса, методу, аксессору, свойству или параметру. Декораторы оборачивают декорируемую сущность и модифицируют ее поведение. Имя декоратора соответствует тому к какому месту применен декоратор.

Рассмотрим на примере базовую структуру декоратора:

```typescript
// Base structure of Decorator :)
const logClass = () => ();
```
Но функция из примера выше едва ли полезна на практике.

Нужно учитывать простое правило при написании декораторов: в качестве единственного аргумента функция классового декоратора должна принимать конструктор декорируемой сущности:

```typescript
// Class Decorator | Декоратор класса
const logClass = (constructor: Function) => {
    console.log(constructor);	// Result of call: class User {}
};

@logClass		// <--- применяем декоратор к классу
class User {

    constructor(public name: string, public age: number) {}

    public getPass(): string {
        return `${this.name}${this.age}`;
    }

}
```

**Важно!** Если декоратор класса вернет значение, то он заменит объявление класса с помощью предоставленного конструктора.

Существует 4 основных типа декоратора:
1. Класса
2. Свойства
3. Метода
4. Аксессора

Далее рассмотрим каждый из типов декораторов.

### Декоратор свойства

Декоратор свойства принимает 2 аргумента:
1. target по сути это прототип класса, к которому применяется декоратор
2. propertyKey это имя поля к которому применяется декоратор (тип либо строка либо символ)

Данный декоратор должен возвращать null, либо дескриптор свойства. Если вернуть дескриптор, то он будет использован для
вызова метода objectDefineProperty.


```typescript
// Property Decorator
const logProperty = (target: Object, propertyKey: string | symbol) => {
    console.log(propertyKey);	// Result of call: "secret"
};

class User {

    @logProperty		// <--- Apply decorator for property
    secret: number;

    constructor(public name: string, public age: number, secret: number) {
        this.secret = secret;
    }

    public getPass(): string {
        return `${this.name}${this.age}`;
    }

}
```

### Декоратор метода

Декоратор метода принимает 3 аргумента:
1. target прототип класса
2. propertyKey имя метода
3. descriptor

Данный вид декоратора может вернуть либо null, либо дескриптор свойства.


```typescript
// Method Decorator
const logMethod = (
  target: Object,
  propertyKey: string | symbol,
  descriptor: PropertyDescriptor
) => {
    console.log(propertyKey);   // Result of call: "getPass"
};

class User {

    constructor(public name: string, public age: number) {}

    @logMethod			// <--- Apply decorator for method
    public getPass(): string {
        return `${this.name}${this.age}`;
    }

}
```

### Декоратор аксессора

По аналогии с декоратором метода


```typescript
// get/set Decorator
const logSet = (
  target: Object,
  propertyKey: string | symbol,
  descriptor: PropertyDescriptor
) => {
    console.log(propertyKey);	// Result of call: "myAge"
};

class User {

    constructor(public name: string, public age: number) {}

    @logSet		// <--- Apply decorator for set
    set myAge(age: number) {
        this.age = age;
    }

}
```

### Фабрика декораторов

Фабрика декораторов это функция, которая возвращает выражение и которая будет вызвана декоратором во время исполнения программы.


```typescript
// Factory Decorator
function factory(value: any) {        // Factory
    return function (target: any) {   // Decorator
        console.log(target);          // Decorator logic
    }
}

// Applying Factory Decorator
const enumerable = (value: boolean) => {
    return (
      target: any,
      propertyKey: string | symbol,
      descriptor: PropertyDescriptor
    ) => {
        descriptor.enumerable = value;
    };
}

class User {

    constructor(public name: string, public age: number) {}

    @enumerable(false)			// <--- Call decorator factory with argument
    public getPass(): string {
        return `${this.name}${this.age}`;
    }

}
```

## Утилиты

### Readonly<T>

Используется для создания типа, все свойства которого предназначены только для чтения


```typescript
// Readonly<T>
interface User {
    name: string;
}

const user: Readonly<User> = {
    name: 'Yauhen',
};

user.name = 'Max';      // Error: cannot reassign a readonly property
```

Еще пример:


```typescript
type Article1 = { title: string; page: number };
type Article2 = Readonly<Article1>; // теперь поля title и page доступны только для чтения

const article1: Article1 = { title: 'Article 1'; page: 1 };
const article2: Article2 = { title: 'Article 2'; page: 2 };

article1.title = 'other'; // ошибки нет
article2.title = 'other' // ошибка при изменнии свойства доступного только для чтения

```



### Required<T>

Required помогает создать тип, где все поля обязательны


```typescript
// Required<T>
interface Props {
    a?: number;
    b?: string;
};

const obj: Props = { a: 5 };              // OK

const obj2: Required<Props> = { a: 5 };   // Error: property 'b' missing
```

### Record<K, T>

Record утилита, которая создает тип с набором свойств первого аргумента K, а также присваивает им тип второго T


```typescript
// Record<T>
interface PageInfo {
    title: string;
}

type Page = 'home' | 'about' | 'contact';

const x: Record<Page, PageInfo> = {
    about: { title: 'about' },
    contact: { title: 'contact' },
    home: { title: 'home' },
};
```
Еще пример:


```typescript
// Record<T>
type Dimensions1 = { width: number; height: number; length: number };
// на основании типа Dimensions1 создадим другой
// в качестве первого аргумента указываются наименования ключей, а в качестве второго их тип данных
type Dimensions2 = Record<'width' | 'height' | 'length', number>

// вариация самостоятельной реализации подобной утилиты

type MyRecord<K extends keyof any, T> = { [P in K]: T };

```

Пример описания объекта

```typescript

type TObj = Record<string, string | null>;

const obj: TObj = { id: '123', corrEst: null, }

```

### Pick<T, K>

Pick создает тип выбирая заданные нами свойства из интерфейса Todo, конструируем объект только из выбранных типов.


```typescript
// Pick<T, K>
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Pick<Todo, 'title' | 'completed'>;

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
};
```

### Omit<T, K>

Omit позволяет удалять ненужные свойства у объекта


```typescript
// Omit<T, K>
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Omit<Todo, 'description'>;

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
};
```

### Exclude<T, U>

Exclude создает тип исключая из него все типы, которые передаются вторым аргументом


```typescript
// Exclude<T, U>
type T0 = Exclude<"a" | "b" | "c", "a">;                      // "b" | "c" тип "a" исключаем
type T1 = Exclude<"a" | "b" | "c", "a" | "b">;                // "c"
type T2 = Exclude<string | number | (() => void), Function>;  // string | number исключили функцию
```

### Extract<T, U>

Extract конструтирует тип оставляя в нем, только переданные свойства


```typescript
// Extract<T, U>
type T0 = Extract<"a" | "b" | "c", "a" | "f">;                // "a"
type T1 = Extract<string | number | (() => void), Function>;  // () => void
```

Полезный паттерн, связанный с переменной, используемой как индекс с типом string


```typescript

 const isNullValues2 = <T extends IDocumentQuerySelectorsDictionary['movieSelectors']>(obj: T): boolean => {
    for (let key in obj) {
      if (obj[key] === null) {
        return true;
      }
    }
    return false;
  };


// если не использовать Generic, а описать просто тип аргумента как IDocumentQuerySelectorsDictionary['movieSelectors'], то далее
// будет ошибка в obj[key] 'string' can't be used to index type
// Generic преобразует ключ как

//let key: Extract<keyof T, string>; // указывает на то, что мы отбираем ключи типа Т с типом string
```


### NonNullable<T>

NonNullable выбрасывает из создаваемого типа все несуществующие типы


```typescript
// NonNullable<T>
type T0 = NonNullable<string | number | undefined>;   // string | number
type T1 = NonNullable<string[] | null | undefined>;   // string[]
```

### ReturnType<T>

ReturnType создает тип, состоящий из возвращаемого функцией типа


```typescript
// ReturnType<T>
declare function f1(): { a: number, b: string };

type T0 = ReturnType<() => string>;                                  // string
type T1 = ReturnType<(s: string) => void>;                           // void
type T2 = ReturnType<(<T>() => T)>;                                  // {} по дефолту дженерик тип пустой объект
type T3 = ReturnType<(<T extends X, X extends number[]>() => T)>;    // number[]
type T4 = ReturnType<typeof f1>;                                     // { a: number, b: string }
type T5 = ReturnType<any>;                                           // any
type T6 = ReturnType<never>;                                         // any
type T7 = ReturnType<string>;                                        // Error
type T8 = ReturnType<Function>;                                      // Error
```

### InstanceType<T>

InstanceType создает тип, состоящий из типа экземпляра функции конструктора


```typescript
// InstanceType<T>
class C {
    x = 0;
    y = 0;
}

type T0 = InstanceType<typeof C>;     // C
type T1 = InstanceType<any>;          // any
type T2 = InstanceType<never>;        // any
type T3 = InstanceType<string>;       // Error
type T4 = InstanceType<Function>;     // Error
```

### ReturnType<Type>

Данная утилита возвращает вычисленный тип результата работы функции


```typescript
type T0 = ReturnType<() => string>;

type T0 = string
type T1 = ReturnType<(s: string) => void>;

//type T1 = void
```

### Partial<Type>

Partial возвращает тип всех свойств расширяемого типа, которые будут установлены как опциональные. Эта утилита вернет тип, представляющий все подмножества данного типа.

Например:


```typescript
type Person1 = {name: string, age: number};
type Person2 = Partial<Person1>

// В типе Person2 поля name и age приняли вид {name?: string | undefined; age?: string | undefined}
// пример собственной реализации Partial

type MyPartial<T> = { [P in keyof T]?: T[P] }

```

## Типизация React

