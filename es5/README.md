[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# Airbnb JavaScript Style Guide() {

**คู่มือแนะนำการเขียนจาวาสคริปต์ที่เข้าท่ามากที่สุด** โดย [Airbnb](https://github.com/airbnb/javascript/)
> คู่มือนี้ผมแปลโดยใส่คำอธิบายและตัวอย่างเพิ่มเติม (ไม่แปลตรงตัว) เพื่อให้ผู้อ่านสามารถเข้าใจเนื้อหาต่างๆได้ดียิ่งขึ้น ในกรณีที่เจอข้อผิดพลาดใดๆ กรุณา Fork และ PR ถ้ามีคำถามสามารถเปิด Issue ได้เลยครับ หวังว่าคู่มือนี้จะมีประโยชน์ต่อผู้อ่านไม่มากก็น้อย :pray:

## <a name='TOC'>สารบัญ</a>

  1. [Types](#types)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
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
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
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

  - **Primitives**: เมื่อใช้งานตัวแปรพื้นฐาน (ตัวแปรที่อ้างอิงด้วยค่า) สามารถเข้าใช้งานได้โดยอ้างอิงค่าของตัวแปร

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo; // bar เก็บค่า 1 โดยจะไม่เกี่ยวข้องกับ foo อีกต่อไป

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complex**: เมื่อใช้งานตัวแปรที่มีความซับซ้อน (ตัวแปรที่อ้างอิงไปยังค่าที่อยู่ของตัวแปรอื่น) สามารถเข้าใช้งานได้โดยอ้างอิงค่าที่อยู่ของตัวแปรนั้นๆ

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo; // bar ไม่ได้เก็บค่า [1,2] แต่ bar ชี้ไปยังที่อยู่ของอาร์เรย์ ซึ่งเป็นที่ๆเดียวกันกับที่ foo ชี้ไป

    bar[0] = 9; // เมื่อเปลี่ยนแปลง bar, foo ก็จะถูกเปลี่ยนแปลงด้วย

    console.log(foo[0], bar[0]); // => 9, 9
    ```
**[[⬆ กลับไปด้านบน]](#TOC)**

## Objects

  - ควรใช้ปีกกา `{}` ในการประกาศออบเจ็กต์

    ```javascript
    // ไม่ดี
    var item = new Object();

    // ดี
    var item = {};
    ```

  - อย่าใช้[คำสงวน](http://es5.github.io/#x7.6.1) เป็นคีย์ เพราะมันจะใช้ไม่ได้ใน IE8. [อ่านเพิ่มเติม](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // ไม่ดี
    var superman = {
      default: { clark: 'kent' }, // default เป็นคำสงวน
      private: true
    };

    // ดี
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - ใช้คำที่มีความหมายเหมือนกันแทนคำสงวน

    ```javascript
    // ไม่ดี
    var superman = {
      class: 'alien' // class เป็นคำสงวน
    };

    // ไม่ดี
    var superman = {
      klass: 'alien' // แปลงคำไม่ใช่สิ่งดี เพราะจะทำให้เดาความหมายได้ยาก
    };

    // ดี
    var superman = {
      type: 'alien'
    };
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**

## Arrays

  - ใช้วงเล็บก้ามปู `[]` ในการประกาศอาร์เรย์

    ```javascript
    // ไม่ดี
    var items = new Array();

    // ดี
    var items = [];
    ```

  - ใช้ฟังก์ชัน Array#push ในการใส่ค่าเข้าไปในอาร์เรย์แทนการใส่ค่าโดยตรง

    ```javascript
    var someStack = [];

    // ไม่ดี
    someStack[someStack.length] = 'abracadabra';

    // ดี
    someStack.push('abracadabra');
    ```

  - ใช้ฟังก์ชัน Array#slice ในการทำสำเนาอาร์เรย์ - [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // ไม่ดี
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // ดี
    itemsCopy = items.slice();
    ```

  - ใช้ฟังก์ชัน Array#slice ใช้การแปลงอาร์เรย์ให้เป็นออบเจ็กต์ (ต้องเป็นอาร์เรย์ที่จัดวางเหมือนออบเจ็กต์เท่านั้นถึงจะทำการแปลงได้)

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Strings

  - ใช้เขี้ยวเดียว (Single quotes) `''`สำหรับสตริง

    ```javascript
    // ไม่ดี
    var name = "Bob Parr";

    // ดี
    var name = 'Bob Parr';

    // ไม่ดี
    var fullName = "Bob " + this.lastName;

    // ดี
    var fullName = 'Bob ' + this.lastName;
    ```

  - สตริงที่ยาวกว่า 80 ตัวอักษร ควรจะแยกเขียนในหลายบรรทัด และค่อยทำการเชื่อมต่อกัน
  - หมายเหตุ: ไม่ควรใช้สตริงที่ยาวมากเกินไป เพราะจะมีผลต่อประสิทธิภาพของแอพพลิเคชั่น  -   [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // ไม่ดี
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // ไม่ดี
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // ดี
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - เมื่อต้องการสร้างสตริง ควรใช้ฟังก์ชัน Array#join แทนการเชื่อมต่อโดยใช้เครื่องหมายบวก `+` โดยเฉพาะสำหรับ IE -  [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // ไม่ดี
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // ดี
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Functions

  - Function expressions - การประกาศฟังก์ชันและใช้ตัวแปรในการอ้างอิงฟังก์ชันดังกล่าว ดังตัวอย่างต่อไปนี้

    ```javascript
    // anonymous function expression
    var anonymous = function() {
      return true;
    };

    // named function expression
    var named = function named() {
      return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - อย่าประกาศฟังก์ชันประเภท Function Declaration ไว้ภายใน if, else, while, และอื่น ๆ เพราะบราวเซอร์จะตีความหมายผิด ถ้าจำเป็นต้องประกาศ ให้ประกาศในรูปแบบของ Function Expression
  - **หมายเหตุ:** ECMA-262 บอกไว้ว่าใน if, else, while, และอื่น ๆ จะต้องประกอบไปด้วย statements เท่านั้น ซึ่งการประกาศฟังก์ชันประเภท Function Declaration ไม่ใช่ statement [อ่านเพิ่มเติมเกี่ยวกับ  ECMA-262](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // ไม่ดี
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // ดี
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - อย่าตั้งชื่อพารามิเตอร์ว่า `arguments` เพราะมันจะไปทับออบเจ็กต์ `arguments` ที่จาวาสคริปต์มีให้ในทุกๆฟังก์ชัน

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

**[[⬆ กลับไปด้านบน]](#TOC)**



## Properties

  - ใช้จุด `.` ในการเข้าถึงพรอพเพอร์ตี้ (properties).

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // ไม่ดี
    var isJedi = luke['jedi'];

    // ดี
    var isJedi = luke.jedi;
    ```

  - ใช้วงเล็บก้ามปู `[]` ในการเข้าถึงพรอพเพอร์ตี้ โดยการใช้ตัวแปร

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop]; //  เข้าถึงพรอพเพอร์ตี้ี้ของ luke โดยใช้ตัวแปร prop
    }

    var isJedi = getProp('jedi');
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Variables

  - ใช้ `var` ในการประกาศตัวแปรเสมอ ถ้าไม่ใช้จะมีผลให้ตัวแปรที่ประกาศขึ้นใหม่เป็นตัวแปรแบบ `global` ซึ่งอาจมีผลต่อไฟล์หรือโมดูลอื่นๆ

    ```javascript
    // ไม่ดี
    superPower = new SuperPower();

    // ดี
    var superPower = new SuperPower();
    ```

  - ใช้หนึ่ง `var` ต่อหนึ่งตัวแปร เพราะดูง่ายกว่า และป้องกันข้อผิดพลาดได้ อย่างเช่น บางครั้งใส่สลับกันระหว่าง `;` และ `,` ซึ่งทำให้ได้ผลลัพธ์ที่ผิด

    ```javascript
    // ไม่ดี
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // ไม่ดี
    var items = getItems(),
        goSportsTeam = true; // ตัวอย่างข้อผิดพลาดที่ใส่ ; แทนที่จะเป็น ,
        dragonball = 'z';

    // ดี
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - ตัวแปรที่ยังไม่มีค่า ให้ประกาศไว้ข้างหลังสุดของการประกาศตัวแปรทั้งหมด ซึ่งจะมีประโยชน์เมื่อเรามาใส่ค่าให้ตัวแปรเหล่านั้นในภายหลัง โดยที่ตัวแปรเหล่านั้นจะต้องใช้ค่าของตัวแปรอื่นๆที่เราประกาศไว้ก่อนหน้า

    ```javascript
    // ไม่ดี
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // ไม่ดี
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // ดี
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - ประกาศตัวแปรทั้งหมดไว้ข้างบนสุดของฟังก์ชัน ซึ่งจะทำให้เราไม่สับสนและยังป้องกันการ hoisting ของจาวาสคริปต์ได้อีกด้วย

    ```javascript
    // ไม่ดี
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // ดี
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // ไม่ดี
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // ดี
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Hoisting

  - เวลาคอมไพล์จาวาสคริปต์จะอ่านตัวแปรที่ประกาศไว้ก่อนหน้าสิ่งอื่นๆในสโคป แต่ค่าที่ใส่ให้ตัวแปรจะยังไม่ถูกอ่าน

    ```javascript
    // สมมุติว่าเราไม่ได้ประกาศตัวแปร notDefined
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // ประกาศตัวแปรหลังจากใช้งาน ในจาวาสคริปต์นั้นทำได้ (ไม่มี error)
    // เพราะว่าตัวแปรจะถูกคอมไพล์และดึงขึ้นมาไว้ข้างบนสโคป
    // แต่ค่าของตัวแปรไม่ได้ถูกดึงขึ้นมาด้วย จึงทำให้ค่าของตัวแปรนั้นเป็น undefined
    function example() {
      console.log(declaredButNotAssigned); // => undefined (ไม่ error แต่ยังไม่มีค่า)
      var declaredButNotAssigned = true;
    }

    // ตัวอย่างเมื่อคอมไพล์เลอร์ทำงานในตัวอย่างข้างต้น
    // คอมไพล์เลอร์จะอ่านตัวแปรและดึงขึ้นมาไว้ด้านบนของสโคป
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonymous function expressions - การประกาศฟังก์ชันโดยไม่ใส่ชื่อฟังก์ชัน คอมไพล์เลอร์จะอ่านตัวแปรและดึงขึ้นไปด้านบนของสโคป แต่จะยังไม่อ่านฟังก์ชัน

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions - การประกาศฟังก์ชันโดยใส่ชื่อฟังก์ชัน ได้ผลลัพธ์เหมือนตัวอย่างก่อนหน้า

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // ประกาศฟังก์ชันชื่อเดียวกับตัวแปร ก็ได้ผลลัพธ์เช่นเดียวกันกับตัวอย่างก่อนหน้า
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Function declarations - การประกาศฟังก์ชันโดยไม่ได้ใส่ค่าฟังก์ชันให้ตัวแปร คอมไพล์เลอร์จะอ่านทั้งชื่อและฟังก์ชัน

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() { // ถึงจะประกาศทีหลังใช้แต่ คอมไพล์เลอร์จะอ่านทั้งชื่อและตัวฟังก์ชัน
        console.log('Flying');
      }
    }
    ```

  - อ่านเพิ่มเติมได้ที่ [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) โดย [Ben Cherry](http://www.adequatelygood.com/).

**[[⬆ กลับไปด้านบน]](#TOC)**



## Comparison Operators & Equality

  - ใช้ `===` และ `!==` แทน `==` และ `!=`
  - การเปรียบเทียบโอเปอเรเตอร์ จาวาสคริปต์จะแปลงค่าเหล่านั้นเป็น boolean โดยใช้ฟังก์ชัน `ToBoolean` และใช้กฏต่างๆดังต่อไปนี้:

    + **Objects** ได้ผลลัพธ์เป็น **true**
    + **Undefined** ได้ผลลัพธ์เป็น **false**
    + **Null** ได้ผลลัพธ์เป็น **false**
    + **Booleans** ได้ผลลัพธ์ **ขึ้นอยู่กับค่าของ boolean**
    + **Numbers** ได้ผลลัพธ์เป็น **false** ถ้า **+0, -0, or NaN**, นอกนั้นได้ **true**
    + **Strings** ได้ผลลัพธ์เป็น **false** ถ้า `''`, นอกนั้นได้ **true**

    ```javascript
    if ([0]) {
      // true
      // เพราะ array คือออบเจ็กต์
    }
    ```

  - ใช้ Shortcuts.

    ```javascript
    // ไม่ดี
    if (name !== '') {
      // ...stuff...
    }

    // ดี
    if (name) {
      // ...stuff...
    }

    // ไม่ดี
    if (collection.length > 0) {
      // ...stuff...
    }

    // ดี
    if (collection.length) {
      // ...stuff...
    }
    ```

  - อ่านเพิ่มเติมได้ที่ [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) โดย Angus Croll.

**[[⬆ กลับไปด้านบน]](#TOC)**


## Blocks

  - ใช้วงเล็บปีกกา `{}` ในกรณีที่ประกาศบล็อกมากกว่าหนึ่งบรรทัด

    ```javascript
    // ไม่ดี
    if (test)
      return false;

    // ดี
    if (test) return false; // วางไว้บรรทัดเดียวกันจะอ่านง่ายกว่า

    // ดี
    if (test) {
      return false;
    }

    // ไม่ดี
    function() { return false; }

    // ดี
    function() { // ถ้ามีวงเล็บปีกกาให้วางไว้คนละบรรทัดจะอ่านง่ายกว่า
      return false;
    }
    ```

  - ถ้าประกาศโดยมีทั้ง `if` และ `else` ให้ใส่  `else` ไว้บรรทัดเดียวกับวงเล็บปีกกาปิดของ `if`

    ```javascript
    // ไม่ดี
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // ดี
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[[⬆ กลับไปด้านบน]](#TOC)**


## Comments

  - ใช้ `/** ... */` สำหรับคอมเม้นต์ที่มากกว่าหนึ่งบรรทัด และควรจะบอกประเภทและค่าของพารามิเตอร์พร้อมทั้งค่าที่จะรีเทิร์น

    ```javascript
    // ไม่ดี
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // ดี
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - ใช้ `//` สำหรับคอมเม้นต์บรรทัดเดียว โดยใส่ไว้บรรทัดบนของสิ่งที่ต้องการคอมเม้นต์ และเพิ่มบรรทัดว่างไว้ด้านบนคอมเม้นต์ด้วย

    ```javascript
    // ไม่ดี
    var active = true;  // is current tab

    // ดี
    // is current tab
    var active = true;

    // ไม่ดี
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // ดี
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - ใส่ `FIXME` หรือ `TODO` ไว้ด้านหน้าคอมเม้นต์ ซึ่งจะช่วยให้ผู้พัฒนาระบบท่านอื่นๆทราบได้ว่าสิ่งเหล่านั้นอาจจะต้องแก้ไข หรือยังไม่ได้ทำ (IDE บางตัวสามารถค้นหาคอมเม้นต์เหล่านี้อัตโนมัติ และบอกถึงสิ่งที่ควรจะแก้ไขหรือทำเพิ่ม)

  - ใช้ `// FIXME:` เพื่อบอกปัญหา

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - ใช้ `// TODO:` เพื่อบอกแนวทางในกาแก้ไขปัญหา (แต่ยังไม่ได้ทำ)

    ```javascript
    function Calculator() {

      // TODO: total ควรจะใส่เป็นพารามิเตอร์ โดยมีค่าหรือไม่มีค่าก็ได้ (ถ้าไม่มีค่าใส่มา ให้ค่าเริ่มต้นเป็น 0)
      this.total = 0;

      return this;
    }
  ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Whitespace

  - ควรตั้งค่าหนึ่งแท็บเท่ากับสองช่องว่าง (สามารถตั้งค่าใน Editor หรือ IDE ได้)

    ```javascript
    // ไม่ดี
    function() {
    ∙∙∙∙var name;
    }

    // ไม่ดี
    function() {
    ∙var name;
    }

    // ดี
    function() {
    ∙∙var name;
    }
    ```

  - ใส่ช่องว่างก่อนวงเล็บปีกกาเปิด

    ```javascript
    // ไม่ดี
    function test(){
      console.log('test');
    }

    // ดี
    function test() {
      console.log('test');
    }

    // ไม่ดี
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // ดี
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - ใส่ช่องว่างก่อนเปิดวงเล็บสำหรับ control statements (`if`, `else`, `while`, และอื่นๆ) แต่สำหรับพารามิเตอร์ไม่ต้องใส่ช่องว่าง

    ```javascript
    // ไม่ดี
    if(isJedi) {
      fight ();
    }

    // ดี
    if (isJedi) {
      fight();
    }

    // ไม่ดี
    function fight () {
      console.log ('Swooosh!');
    }

    // ดี
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - ใส่ช่องว่างเวลาประกาศตัวแปร

    ```javascript
    // ไม่ดี
    var x=y+5;

    // ดี
    var x = y + 5;
    ```

  - ลงท้ายไฟล์ด้วยการขึ้นบรรทัดใหม่เสมอ (แค่หนึ่งบรรทัดเท่านั้น)

    ```javascript
    // ไม่ดี
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // ไม่ดี
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // ดี
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - ใส่ย่อหน้าเวลาเรียกใช้เมท็อตแบบต่อเนื่อง (method chaining) ให้วางจุด `.` ไว้ด้านหน้าเสมอ เพื่อบอกว่าเป็นการเรียกเมท็อต

    ```javascript
    // ไม่ดี
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // ไม่ดี
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // ดี
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // ไม่ดี
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // ดี
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - ใส่บรรทัดว่างหลังจากบล็อก และก่อนที่จะขึ้น statement ใหม่

    ```javascript
    // ไม่ดี
    if (foo) {
      return bar;
    }
    return baz;

    // ดี
    if (foo) {
      return bar;
    }

    return baz;

    // ไม่ดี
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // ดี
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```


**[[⬆ กลับไปด้านบน]](#TOC)**

## Commas

  - อย่าวางจุลภาค `,` ไว้ด้านหน้า

    ```javascript
    // ไม่ดี
    var story = [
        once
      , upon
      , aTime
    ];

    // ดี
    var story = [
      once,
      upon,
      aTime
    ];

    // ไม่ดี
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // ดี
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - อย่าใส่จุลภาค ถ้าไม่มีค่าอื่นๆต่อท้ายแล้ว เพราะอาจจะทำให้เกิดปัญหาใน IE6/7 และ IE9 นอกจากนั้นใน ES3 จะเพิ่มความยาวของอาร์เรย์ถ้าเจอจุลภาคอยู่ด้านหลังสุดซึ่งเป็นสิ่งที่ผิด อ่านเพิ่มเติมที่ ([ES5](http://es5.github.io/#D)):

  > Edition 5 ได้แก้ไขข้อผิดพลาดนี้ โดยไม่เพิ่มความยาวของอาร์เรย์จากจุลภาคที่อยู่หลังสุด ซึ่งเป็นข้อผิดพลาดใน ES3

    ```javascript
    // ไม่ดี
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // ดี
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Semicolons

  - ควรใส่ `;` เมื่อจบ statement

    ```javascript
    // ไม่ดี
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // ดี
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // ดี (เป็นการป้องกันไม่ให้ฟังก์ชันถูกตีความเป็น argument เมื่อทำการต่อไฟล์สองไฟล์ที่ใช้ IIFEs)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [อ่านเพิ่มเติม](http://stackoverflow.com/a/7365214/1712802)

**[[⬆ กลับไปด้านบน]](#TOC)**


## Type Casting & Coercion

  - เวลาแปลงค่าสตริงให้ใส่ `''` ไว้ด้านหน้า เพราะเวลาอ่านจะทราบได้ทันที่ว่าค่าที่จะได้ จะเป็นชนิดสตริง
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // ไม่ดี
    var totalScore = this.reviewScore + '';

    // ดี
    var totalScore = '' + this.reviewScore;

    // ไม่ดี
    var totalScore = '' + this.reviewScore + ' total score';

    // ดี
    var totalScore = this.reviewScore + ' total score';
    ```

  - เวลาใช้ `parseInt` ในการแปลงค่าให้เป็นตัวเลข ควรจะใส่เลขฐานที่ต้องการแปลงด้วย เพราะถ้าไม่ใส่อาจจะมีข้อผิดพลาดได้ถ้าค่าที่แปลงเป็นสตริงที่ไม่ได้ประกอบไปด้วยตัวเลขทั้งหมด

    ```javascript
    var inputValue = '4';

    // ไม่ดี
    var val = new Number(inputValue);

    // ไม่ดี
    var val = +inputValue;

    // ไม่ดี
    var val = inputValue >> 0;

    // ไม่ดี
    var val = parseInt(inputValue);

    // ดี
    var val = Number(inputValue);

    // ดี
    var val = parseInt(inputValue, 10);
    ```

  - ในบางกรณีที่ต้องการให้ได้ประสิทธิภาพสูงสุดด้วยการใช้ Bitshift แทนการแปลงค่าโดยใช้ `parseInt` สามารถอ่านเพิ่มเติมได้ที่ [performance reasons](http://jsperf.com/coercion-vs-casting/3), มีคอมเม้นต์ต่างๆที่อธิบายถึงเรื่องประสิทธิภาพ

    ```javascript
    // ดี
    /**
     * ถ้า parseInt ทำให้โค้ดช้า ให้ใช้
     * Bitshifting เพื่อแปลงค่าเป็นตัวเลขแทน
     * ซึ่งทำให้โค้ดสามารถทำงานได้เร็วขึ้นอย่างมาก
     */
    var val = inputValue >> 0;
    ```

  - **หมายเหตุ:** ควรระวังการใช้งาน bitshift เพราะตัวเลขปกติจะเป็น [64-bit values](http://es5.github.io/#x4.3.19), แต่ Bitshift จะคืนค่าเป็น 32-bit เสมอ ([ที่มา](http://es5.github.io/#x11.7)) Bitshift อาจทำให้ค่าผิดแปลกไปถ้าค่าของตัวเลขใหญ่กว่า 32 bits. [ดูการพูดคุยในเรื่องนี้](https://github.com/airbnb/javascript/issues/109) ตัวเลขที่มากที่สุดของ 32-bit Int คือ 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648 เกินค่ามากที่สุดของ 32-bit Int จึงทำให้เกิดข้อผิดพลาด
    2147483649 >> 0 //=> -2147483647 เกินค่ามากที่สุดของ 32-bit Int จึงทำให้เกิดข้อผิดพลาด
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // ไม่ดี
    var hasAge = new Boolean(age);

    // ดี
    var hasAge = Boolean(age);

    // ดี
    var hasAge = !!age;
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Naming Conventions

  - ควรจะตั้งชื่อให้สื่อความหมาย

    ```javascript
    // ไม่ดี
    function q() {
      // ...stuff...
    }

    // ดี
    function query() {
      // ..stuff..
    }
    ```

  - ใช้ camelCase (ขึ้นต้นด้วยตัวเล็กและคำต่อไปขึ้นต้นด้วยตัวใหญ่) เมื่อต้องการตั้งชื่อออบเจ็กต์, ฟังก์ชัน, และ instance

    ```javascript
    // ไม่ดี
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // ดี
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - ใช้ PascalCase (ขึ้นต้นทุกคำด้วยตัวใหญ่) เมื่อต้องการตั้งชื่อ constructor หรือ class

    ```javascript
    // ไม่ดี
    function user(options) {
      this.name = options.name;
    }

    var ไม่ดี = new user({
      name: 'nope'
    });

    // ดี
    function User(options) {
      this.name = options.name;
    }

    var ดี = new User({
      name: 'yup'
    });
    ```

  - ขึ้นต้นด้วยขีดล่าง (`_`) เมื่อต้องการตั้งชื่อพรอพเพอร์ตี้ที่เป็น Private

    ```javascript
    // ไม่ดี
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // ดี
    this._firstName = 'Panda';
    ```

  - เมื่อต้องการบันทึกค่า `this` ไว้ใช้ ให้ใส่ไว้ในตัวแปรชื่อ `_this`

    ```javascript
    // ไม่ดี
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // ไม่ดี
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // ดี
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - ตั้งชื่อฟังก์ชันเสมอ ซึ่งจะเป็นประโยชน์เวลาดู Stack traces เมื่อทำการ Debug

    ```javascript
    // ไม่ดี
    var log = function(msg) {
      console.log(msg);
    };

    // ดี
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **หมายเหตุ** สำหรับบราวเซอร์ที่ต่ำกว่า IE8 อาจจะมีข้อผิดพลาดถ้าตั้งชื่อให้กับ Function expression อ่านเพิ่มเติมได้ที่  [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/)

  - ถ้าในไฟล์มีแค่หนึ่งคลาส ให้ตั้งชื่อไฟล์ให้เป็นชื่อเดียวกับชื่อคลาส

    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // in some other file
    // ไม่ดี
    var CheckBox = require('./checkBox');

    // ไม่ดี
    var CheckBox = require('./check_box');

    // ดี
    var CheckBox = require('./CheckBox');
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Accessors

  - Accessor functions (ฟังก์ชันที่ใช้ในการเข้าถึงพรอพเพอร์ตี้) ไม่จำเป็นต้องมีก็ได้
  - แต่ถ้ามีควรจะตั้งชื่อในรูปแบบ getVal() และ setVal('hello')

    ```javascript
    // ไม่ดี
    dragon.age();

    // ดี
    dragon.getAge();

    // ไม่ดี
    dragon.age(25);

    // ดี
    dragon.setAge(25);
    ```

  - ถ้าพรอพเพอร์ตี้เป็นค่าบูลีน (boolean) ให้ใช้ isVal() หรือ hasVal().

    ```javascript
    // ไม่ดี
    if (!dragon.age()) {
      return false;
    }

    // ดี
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - ความจริงแล้วตั้งชื่อ get() และ set() ก็ไม่เสียหายอะไร แต่ต้องตั้งให้เหมือนกันในทุกๆที่

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Constructors

  - ควรเพิ่มเมท็อตของออบเจ็คต์ ผ่านทาง Prototype ด้วยการใช้จุด `.` เพราะจะเป็นการเพิ่มพรอพเพอร์ตี้ ไม่ใช่การสร้างออบเจ็คต์ใหม่ ถ้าสร้างออบเจ็คต์ใหม่ จะไม่สามารถทำ Inheritance ได้อีก

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // ไม่ดี
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // ดี
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - เมท็อตควรคืนค่าเป็นออบเจ็ค `this` เพื่อช่วยให้สามารถทำ Method chaining.

    ```javascript
    // ไม่ดี
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // ดี
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - สามารถทำการ Overwrite เมท็อต toString() ได้แต่ควรจะตรวจสอบให้มั่นใจว่าจะไม่เกิดข้อผิดพลาดขึ้นได้ในอนาคต

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Events

  - เมื่อทำการเชื่อมต่ออีเว้นต์ ให้ส่งค่าที่เป็นออบเจ็คต์ไป ซึ่งจะดีกว่าการส่งค่าแบบธรรมดา เพราะจะช่วยให้ตัวเมท็อตที่รับค่าสามารถแก้ไขค่าและเพิ่มพรอพเพอร์ตี้ได้ง่ายขึ้น

    ```js
    // ไม่ดี
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    ```js
    // ดี
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[[⬆ กลับไปด้านบน]](#TOC)**


## Modules

  - โมดูล (Module) ควรเริ่มต้นไฟล์ด้วยเครื่องหมายอัศเจรีย์ `!` เพื่อให้แน่ใจว่าถ้ามีโมดูลอื่นที่ลืมใส่ semicolon `;` ในบรรทัดสุดท้าย และนำไฟล์มาต่อกับโมดูลนี้็จะไม่ทำให้เกิดข้อผิดพลาดขึ้น (ปกติถ้าใช้ uglify ในการต่อไฟล์  จะใส่ ! ให้อัตโนมัติ)  [คำอธิบาย](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - ไฟล์ขึ้นจะตั้งชื่อแบบ camelCase และใส่ไว้ในโฟลเดอร์ชื่อเดียวกัน นอกจากนั้นฟังก์ชันที่นำออกมาจากไฟล์ควรจะเป็นชื่อเดียวกับชื่อไฟล์
  - เพิ่มเมท็อตชื่อ `noConflict()` เพื่อใช้ในการนำออกโมดูลเวอร์ชันก่อนหน้าที่จะทำการเปลี่ยนแปลง
  - ใส่ `'use strict';` ใช้ที่บรรทัดบนสุดของโมดูลเสมอ

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## jQuery

  - ใส่สัญลักษณ์ `$` ไว้ด้านหน้าตัวแปรทุกตัวที่เป็น jQuery Object

    ```javascript
    // ไม่ดี
    var sidebar = $('.sidebar');

    // ดี
    var $sidebar = $('.sidebar');
    ```

  - ในกรณีที่ต้องค้นหา DOM โดยใช้ jQuery ควรจะเก็บแคช (Cache) ไว้เสมอ เพราะการค้นหา DOM ซ้ำๆหลายรอบจะส่งผลต่อประสิทธิภาพของโค้ด

    ```javascript
    // ไม่ดี
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // ดี
    function setSidebar() {
      var $sidebar = $('.sidebar'); // เก็บแคชในการค้นหาไว้ในตัวแปร เพื่อนำไปใช้ต่อไป
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - เวลาค้นหา DOM ให้ใช้รูปแบบของ Cascading เช่น  `$('.sidebar ul')` หรือ parent > child `$('.sidebar > ul')` -  [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - ใช้ `find` ร่วมกับ jQuery object (ที่เราแคชไว้ก่อนหน้านี้)

    ```javascript
    // ไม่ดี
    $('ul', '.sidebar').hide();

    // ไม่ดี
    $('.sidebar').find('ul').hide();

    // ดี
    $('.sidebar ul').hide();

    // ดี
    $('.sidebar > ul').hide();

    // ดี
    $sidebar.find('ul').hide();
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## ECMAScript 5 Compatibility

  - อ่านเพิ่มเติมได้ที่ [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/).

**[[⬆ กลับไปด้านบน]](#TOC)**


## Testing

  - **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Performance

**อ่านเพิ่มเติมจากข้อมูลต่อไปนี้**

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[[⬆ กลับไปด้านบน]](#TOC)**


## Resources

**อ่านเพิ่มเติม**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**เครื่องมือต่างๆ**

  - Code Style Linters
    + [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**ข้อมูลแนะนำการเขียนจาวาสคริปต์อื่นๆ**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)
  - [JavaScript Standard Style](https://github.com/feross/standard)

**ข้อมูลแนะนำสไตล์อื่นๆ**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**อ่านเพิ่มเติม**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**หนังสือ**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net) - Marijn Haverbeke
  - [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) - Kyle Simpson

**บล็อก**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**พอดคาสต์ (Podcasts)**

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)


**[[⬆ กลับไปด้านบน]](#TOC)**

## In the Wild

  รายชื่อองกรค์ที่ทำตามคู่มือแนะนำการเขียนจาวาสคริปต์นี้ ถ้าองค์กรของคุณทำตามคู่มือนี้เช่นกัน กรุณาส่ง Pull request หรือเปิด Issue แล้วเราจะเพิ่มคุณเข้าไปในรายชื่อต่อไปนี้

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Nordic Venture Family**: [CodeDistillery/javascript](https://github.com/CodeDistillery/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## Translation

คู่มือแนะนำการเขียนจาวาสคริปต์นี้ได้ถูกแปลเป็นภาษาต่างๆมากมายดังต่อไปนี้:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese(Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese(Simplified)**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

## คู่มือแนะนำการเขียนจาวาสคริปต์

  - [อ้างอิง](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## พูดคุยกับพวกเราเกี่ยวกับจาวาสคริปต์

  - ติดต่อเราบน [gitter](https://gitter.im/airbnb/javascript).

## ผู้ที่มีส่วนช่วย

  - [ดูรายชื่อผู้ที่มีส่วนช่วย](https://github.com/airbnb/javascript/graphs/contributors)


## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆ กลับไปด้านบน]](#TOC)**

# };
