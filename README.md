[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# Airbnb JavaScript Style Guide() {

**คู่มือแนะนำการเขียนจาวาสคริปต์ที่เข้าท่ามากที่สุด** โดย [Airbnb](https://github.com/airbnb/javascript/)
> คู่มือนี้ผมแปลโดยใส่คำอธิบายและตัวอย่างเพิ่มเติม (ไม่แปลตรงตัว) เพื่อให้ผู้อ่านสามารถเข้าใจเนื้อหาต่างๆได้ดียิ่งขึ้น ในกรณีที่เจอข้อผิดพลาดใดๆ กรุณา Fork และ PR ถ้ามีคำถามสามารถเปิด Issue ได้เลยครับ หวังว่าคู่มือนี้จะมีประโยชน์ต่อผู้อ่านไม่มากก็น้อย :pray:

[คลิ๊กที่นี่สำหรับจาวาสคริปต์เวอร์ชั่น 5 (ES5)](es5/)

## Table of Contents

  1. [Types](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6 Styles](#ecmascript-6-styles)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide)
  1. [Chat With Us About Javascript](#chat-with-us-about-javascript)
  1. [Contributors](#contributors)
  1. [License](#license)

## Types

  - [1.1](#1.1) <a name='1.1'></a> **Primitives**: เมื่อใช้งานตัวแปรพื้นฐาน (ตัวแปรที่อ้างอิงด้วยค่า) สามารถเข้าใช้งานได้โดยอ้างอิงค่าของตัวแปร

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - [1.2](#1.2) <a name='1.2'></a> **Complex**: เมื่อใช้งานตัวแปรที่มีความซับซ้อน (ตัวแปรที่อ้างอิงไปยังค่าที่อยู่ของตัวแปรอื่น) สามารถเข้าใช้งานได้โดยอ้างอิงค่าที่อยู่ของตัวแปรนั้นๆ

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ กลับไปด้านบน](#table-of-contents)**

## References

  - [2.1](#2.1) <a name='2.1'></a> ใช้ `const` สำหรับค่าคงที่ และหลีกเลี่ยงการใช้ `var`

  > เพราะว่าการใช้งาน `const` จะทำให้เราไม่สามารถเปลี่ยนแปลงค่าได้อีก ซึ่งป้องกันข้อผิดพลาดต่างๆที่อาจจะเกิดขึ้น (ในกรณีที่เราลืมไปเปลี่ยนแปลงค่าของตัวแปร หรือมีไลบรารี่อื่นๆที่เราใช้มาเปลี่ยนแปลงค่าตัวแปรของเรา)

    ```javascript
    // ไม่ดี
    var a = 1;
    var b = 2;

    // ดี
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name='2.2'></a> ถ้าต้องการตัวแปรที่เปลี่ยนแปลงค่าได้ให้ใช้ `let` และหลีกเลี่ยงการใช้ `var`

  > เพราะว่า `let` จะมีค่าอยู่แค่ในปีกกาที่ประกาศ (Block-scoped) ซึ่งไม่เหมือน `var` ที่มีค่าอยู่ในฟังก์ชันที่ประกาศ (Function-scoped)

    ```javascript
    // ไม่ดี
    var count = 1;
    if (true) {
      count += 1;
    }

    // ดี
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  - [2.3](#2.3) <a name='2.3'></a> `let` และ `const` จะมีค่าอยู่แค่ในปีกกาที่ประกาศ (Block-scoped) เท่านั้น

    ```javascript
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError เมื่อออกนอกปีกกาที่ประกาศจะไม่สามารถเรียกใช้งานตัวแปรได้
    console.log(b); // ReferenceError เมื่อออกนอกปีกกาที่ประกาศจะไม่สามารถเรียกใช้งานตัวแปรได้
    ```

**[⬆ กลับไปด้านบน](#table-of-contents)**

## Objects

  - [3.1](#3.1) <a name='3.1'></a> ควรใช้ปีกกา `{}` ในการประกาศออบเจ็กต์

    ```javascript
    // ไม่ดี
    const item = new Object();

    // ดี
    const item = {};
    ```

  - [3.2](#3.2) <a name='3.2'></a> อย่าใช้[คำสงวน](http://es5.github.io/#x7.6.1) เป็นคีย์ เพราะมันจะใช้ไม่ได้ใน IE8. [อ่านเพิ่มเติม](https://github.com/airbnb/javascript/issues/61) แต่ถ้าเราสร้างโมดูลของตัวเองก็สามารถใช้คำเหล่านี้ได้ (แต่ไม่ใช้จะดีกว่าในความเห็นของผมนะครับ)

    ```javascript
    // ไม่ดี
    const superman = {
      default: { clark: 'kent' }, // default เป็นคำสงวน
      private: true,
    };

    // ดี
    const superman = {
      defaults: { clark: 'kent' },
      hidden: true,
    };
    ```

  - [3.3](#3.3) <a name='3.3'></a> ใช้คำที่มีความหมายเหมือนกันแทนคำสงวน

    ```javascript
    // ไม่ดี
    const superman = {
      class: 'alien', // class เป็นคำสงวน
    };

    // ไม่ดี
    const superman = {
      klass: 'alien', // แปลงคำไม่ใช่สิ่งดี เพราะจะทำให้เดาความหมายได้ยาก
    };

    // ดี
    const superman = {
      type: 'alien',
    };
    ```

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name='3.4'></a> ถ้าต้องการสร้างพรอพเพอร์ตี้ของออบเจ็คต์จากตัวแปร (Dynamic property) ให้สร้างตอนที่ประกาศออบเจ็คต์โดยใช้ `[]`

  > เพราะจะทำให้พรอพเพอร์ตี้ทั้งหมดถูกสร้างไว้ในที่เดียว ซึ่งทำให้ดูได้ง่ายกว่าการสร้างแยกกัน

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // ไม่ดี
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true; // สร้างหลังจากประกาศออบเจ็คต์เสร็จแล้ว ทำให้มองยากกว่า

    // ดี
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true, // สร้างตอนประกาศออบเจ็คต์ ทำให้มองเห็นพรอพเพอร์ตี้ของออบเจ็คต์ทั้งหมดในที่เดียว
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name='3.5'></a> สร้างเมท็อตโดยใช้วิธีการประกาศแบบย่อ (Object method shorthand)

    ```javascript
    // ไม่ดี
    const atom = {
      value: 1,

      addValue: function (value) { // การประกาศแบบปกติ
        return atom.value + value;
      },
    };

    // ดี
    const atom = {
      value: 1,

      addValue(value) { // การประกาศแบบย่อ ซึ่งตัดคีย์เวิร์ดฟังก์ชันออกไป ทำให้โค้ดอ่านง่ายขึ้น
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a>
  - [3.6](#3.6) <a name='3.6'></a> สร้องพรอพเพอร์ตี้โดยใช้วิธีการประกาศแบบย่อ (Property value shorthand)

  > เพราะว่าทำให้อ่านง่ายขึ้น และเข้าใจได้ง่ายกว่า

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // ไม่ดี
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // ดี
    const obj = {
      lukeSkywalker, // มีค่าเท่ากับด้านบนเพียงแต่ทำให้อ่านง่ายขึ้น (ถ้าต้องการให้ชื่อตัวแปรและชื่อพรอพเพอร์ตี้ต่างกัน ต้องใช้วิธีการประกาศแบบด้านบน)
    };
    ```

  - [3.7](#3.7) <a name='3.7'></a> พรอพเพอร์ตี้ที่ประกาศโดยใช้วิธีการประกาศแบบย่อ ให้ใส่ไว้ด้านบนสุดของการประกาศออบเจ็กต์

  > เพราะทำให้รู้ได้ว่าพรอพเพอร์ตี้ใด ที่ประกาศโดยใช้วิธีการประกาศแบบย่อ

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // ไม่ดี
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // ดี
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

**[⬆ กลับไปด้านบน](#table-of-contents)**

## Arrays

  - [4.1](#4.1) <a name='4.1'></a> ใช้วงเล็บก้ามปู `[]` ในการประกาศอาร์เรย์

    ```javascript
    // ไม่ดี
    const items = new Array();

    // ดี
    const items = [];
    ```

  - [4.2](#4.2) <a name='4.2'></a> ใช้ฟังก์ชัน Array#push ในการใส่ค่าเข้าไปในอาร์เรย์แทนการใส่ค่าโดยตรง

    ```javascript
    const someStack = [];


    // ไม่ดี
    someStack[someStack.length] = 'abracadabra';

    // ดี
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> ใช้ `...` (Spreads) ในการทำสำเนาอาร์เรย์.

    ```javascript
    // ไม่ดี
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // ดี
    const itemsCopy = [...items];
    ```
  - [4.4](#4.4) <a name='4.4'></a> ใช้ฟังก์ชัน Array#form ในการแปลงอ็อบเจ็กต์เป็นอาร์เรย์

    ```javascript
    const foo = document.querySelectorAll('.foo'); 
    const nodes = Array.from(foo);
    ```

**[⬆ กลับไปด้านบน](#table-of-contents)**

## Destructuring

  - [5.1](#5.1) <a name='5.1'></a> ใช้รูปแบบของ `Destructuring`` เมื่อต้องการเข้าถึงอ็อบเจ็กต์ที่มีหลายพรอพเพอร์ตี้

  > เพราะจะได้ไม่ต้องสร้างตัวแปรชั่วคราวมารับพรอพเพอร์ตี้เหล่านั้น

    ```javascript
    // ไม่ดี
    function getFullName(user) {
      const firstName = user.firstName; // ต้องสร้างตัวแปรทั่วคราวมารับค่า
      const lastName = user.lastName; // ต้องสร้างตัวแปรทั่วคราวมารับค่า

      return `${firstName} ${lastName}`;
    }

    // ดี
    function getFullName(obj) {
      const { firstName, lastName } = obj; // ใช้ Destructuring ในการแปลงค่าอ็อบเจ็กต์ให้เป็นตัวแปร
      return `${firstName} ${lastName}`;
    }

    // ดีที่สุด
    function getFullName({ firstName, lastName }) { // รับค่าโดยใช้ Destructuring
      return `${firstName} ${lastName}`;
    }
    ```

  - [5.2](#5.2) <a name='5.2'></a> ใช้รูปแบบของ `Destructuring`` เมื่อต้องการเข้าถึงอาร์เรย์

    ```javascript
    const arr = [1, 2, 3, 4];

    // ไม่ดี
    const first = arr[0];
    const second = arr[1];

    // ดี
    const [first, second] = arr; // ใช้ Destructuring ในการแปลงค่าอาร์เรย์ให้เป็นตัวแปร
    ```

  - [5.3](#5.3) <a name='5.3'></a> ใช้ Destructuring ในรูปแบบออบเจ็กต์ในการส่งค่าหลายค่ากลับไปจากฟังก์ชัน (อย่าใช้ Destructuring ในรูปแบบอาร์เรย์ )

  > เพราะ Destructuring ในรูปแบบออบเจ็กต์จะทำให้ลำดับการส่งค่าไม่สำคัญ (สามารถสลับที่กันได้) เผื่อว่าในอนาคตเราอาจจะเพิ่มค่าเข้าไป ก็จะไม่ต้องกังวลเรื่องลำดับ

    ```javascript
    // ไม่ดี
    function processInput(input) {
      return [left, right, top, bottom];
    }

    // คนที่เรียกใช้งานฟังก์ชันจะต้องคำนึงถึงลำดับของตัวแปรที่จะส่งไป
    const [left, __, top] = processInput(input);

    // ดี
    function processInput(input) {
      return { left, right, top, bottom };
    }

    // คนที่เรียกใช้งานฟังก์ชันสามารถส่งเฉพาะค่าที่ตนเองต้องการ โดยมันจะจับคู่ให้อัตโนมัติ
    const { left, right } = processInput(input);
    ```


**[⬆ กลับไปด้านบน](#table-of-contents)**

## Strings

  - [6.1](#6.1) <a name='6.1'></a> ใช้เขี้ยวเดียว (Single quotes) `''`สำหรับสตริง

    ```javascript
    // ไม่ดี
    const name = "Capt. Janeway";

    // ดี
    const name = 'Capt. Janeway';
    ```

  - [6.2](#6.2) <a name='6.2'></a> สตริงที่ยาวกว่า 80 ตัวอักษร ควรจะแยกเขียนในหลายบรรทัด และค่อยทำการเชื่อมต่อกัน
  - [6.3](#6.3) <a name='6.3'></a> หมายเหตุ: ไม่ควรใช้สตริงที่ยาวมากเกินไป เพราะจะมีผลต่อประสิทธิภาพของแอพพลิเคชั่น  -   [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // ไม่ดี
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // ไม่ดี
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // ดี
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a>
  - [6.4](#6.4) <a name='6.4'></a> เมื่อต้องการสร้างสตริงที่มีตัวแปร ให้ใช้เทมเพลตสตริง (Template strings) ซึ่งดีกว่าการเชื่อมสตริงด้วยตนเอง

  > เพราะว่าเทมเพลตสตริงจะทำให้อ่านง่ายกว่า

    ```javascript
    // ไม่ดี
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // ไม่ดี
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // ดี
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

**[⬆ กลับไปด้านบน](#table-of-contents)**


## Functions

  - [7.1](#7.1) <a name='7.1'></a> ทุกครั้งที่จะประกาศฟังก์ชันให้ประกาศในรูปแบบ Function declarations (อย่าประกาศแบบ Function expressions)

  > เพราะ Function declarations มีชื่อให้เห็นชัดเจน เมื่อทำการดีบัคโค้ดจะสามารถเห็นชื่อฟังก์ชั่นใน Call stacks นอกจากนั้นจาวาสคริปต์จะ Hoisting ฟังก์ชั่นที่ประกาศแบบ Function declarations ทำให้สามารถเรียกใช้ฟังก์ชั่นได้ทุกที่ เมื่อใดที่ต้องการใช้งาน Function expressions ให้ใช้ [Arrow Functions](#arrow-functions) แทนเสมอ

    ```javascript
    // ไม่ดี
    const foo = function () {
    };

    // ดี
    function foo() {
    }
    ```

  - [7.2](#7.2) <a name='7.2'></a> Function expressions - การประกาศฟังก์ชันและใช้ตัวแปรในการอ้างอิงฟังก์ชันดังกล่าว (อาจจะไม่ใช้ตัวแปรในกรณีที่เป็น Anonymous function) ดังตัวอย่างต่อไปนี้

    ```javascript
    // immediately-invoked function expression (IIFE)
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [7.3](#7.3) <a name='7.3'></a> อย่าประกาศฟังก์ชันประเภท Function Declarations ไว้ภายใน if, else, while, และอื่นๆ เพราะบราวเซอร์จะตีความหมายผิด ถ้าจำเป็นต้องประกาศ ให้ประกาศในรูปแบบของ Function Expressions
  - [7.4](#7.4) <a name='7.4'></a> **หมายเหตุ:** ECMA-262 บอกไว้ว่าใน if, else, while, และอื่นๆ จะต้องประกอบไปด้วย statements เท่านั้น ซึ่งการประกาศฟังก์ชันประเภท Function Declarations ไม่ใช่ statement [อ่านเพิ่มเติมเกี่ยวกับ  ECMA-262](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)

    ```javascript
    // ไม่ดี
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // ดี
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  - [7.5](#7.5) <a name='7.5'></a> อย่าตั้งชื่อพารามิเตอร์ว่า `arguments` เพราะมันจะไปทับออบเจ็กต์ `arguments` ที่จาวาสคริปต์มีให้ในทุกๆฟังก์ชัน

    ```javascript
    // ไม่ดี
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // ดี
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

  <a name="es6-rest"></a>
  - [7.6](#7.6) <a name='7.6'></a> ให้ใช้ `...` (Rest) แทนการใช้พารามิเตอร์ `arguments`

  > เพราะ `...` สามารถทำให้รู้ว่าฟังก์ชันนั้นมีการรับค่าพารามิเตอร์ อีกทั้ง `...` จะได้ค่าอาร์เรย์จริงๆ ไม่ใช่ค่าออบเจ็กต์เหมือน `arguments`

    ```javascript
    // ไม่ดี
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // ดี
    function concatenateAll(...args) { // ทำให้รู้ว่าฟังก์ชั่นนี้รับพารามิเตอร์
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> ใส่ค่าเริ่มต้นให้กับพารามิเตอร์ทุกตัว (Default parameter)

    ```javascript
    // แย่มาก
    function handleThings(opts) {
      // ไม่ควรเปลี่ยนค่าของพารามิเตอร์ อ่านเพิ่มเติมได้ที่ http://spin.atomicobject.com/2011/04/10/javascript-don-t-reassign-your-function-arguments
      // นอกจากนั้นถ้า opts เป็นเท็จ จะได้ค่าที่เป็นออบเจ็กต์ ซึ่งดูเหมือนว่า
      // จะเป็นค่าที่เราต้องการ แต่ความจริงนั้นจะทำให้เกิดบัค
      opts = opts || {};
      // ...
    }

    // แย่
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // ดี
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> หลีกเลี่ยงการตั้งค่าที่ยากๆเป็นค่าเริ่มต้นของพารามิเตอร์

  > เพราะจะทำให้สับสนได้ง่าย

  ```javascript
  var b = 1;
  // ไม่ดี
  function count(a = b++) {
    console.log(a);
  }
  count();  // 1
  count();  // 2
  count(3); // 3 เพราะว่ามีการกำหนดอาร์กิวเมนต์เป็น 3 ดังนั้นค่าเริ่มต้นจะไม่ถูกเรียก (= b++ ไม่ถูกเรียก)
  count();  // 3
  ```


**[⬆ กลับไปด้านบน](#table-of-contents)**


# };
