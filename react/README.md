# Airbnb React/JSX Style Guide

**คู่มือแนะนำการเขียน React และ JSX ที่เข้าท่ามากที่สุด** โดย [Airbnb](https://github.com/airbnb/javascript/)
> คู่มือนี้ผมแปลโดยใส่คำอธิบายและตัวอย่างเพิ่มเติม (ไม่แปลตรงตัว) เพื่อให้ผู้อ่านสามารถเข้าใจเนื้อหาต่างๆได้ดียิ่งขึ้น ในกรณีที่เจอข้อผิดพลาดใด ๆ กรุณา Fork และ PR ถ้ามีคำถามสามารถเปิด Issue ได้เลยครับ หวังว่าคู่มือนี้จะมีประโยชน์ต่อผู้อ่านไม่มากก็น้อย :pray:

## <a name='TOC'>สารบัญ</a>

  1. [Basic Rules](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)
  1. [`isMounted`](#ismounted)

## Basic Rules

  - หนึ่งคอมโพเน้นท์ต่อหนึ่งไฟล์เท่านั้น
    - ยกเว้นคอมโพเน้นท์ประเภท [Stateless หรือ Pure Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) (คอมโพเน้นท์ที่ไม่มีการจัดการ State ในตัวเอง) สามารถที่จะเขียนรวมในไฟล์เดียวกันได้ อ่านเพิ่มเติมจากกฎของ Eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - ใช้ JSX ร่วมกับ React เสมอ
  - อย่าใช้ `React.createElement` ยกเว้นในการณีที่ต้องจัดการกับแอพพลิเคชั่นที่มีไฟล์ที่ไม่ได้ใช้ JSX เท่านั้น

## Class vs `React.createClass` vs stateless

  - ถ้าภายในคอมโพเน้นท์มีการจัดการ State และ refs ให้เลือกใช้ `class extends React.Component` แทนการใช้ `React.createClass` ยกเว้นเราต้องการใช้งาน Mixins (Mixins ใช้งานได้กับ React.createClass เท่านั้น ไม่สามารถใช้ร่วมกับคลาสของ ES6 ได้) อ่านเพิ่มเติมจากกฎของ Eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```javascript
    // ไม่ดี
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // ดี
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    แต่ถ้าภายในคอมโพเน้นท์ไม่มีการจัดการ State หรือ refs เราควรจะใช้ Function declaration แทนการสร้างคลาส

    ```javascript

    // แย่
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // แย่ (เพราะ Arrow function ไม่สามารถกำหนดชื่อฟังก์ชันได้ (Arrow function เป็นการเขียนแบบย่อของ Anonymous function จึงไม่สามารถกำหนดชื่อได้))
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // ดี
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Naming

  - **นามสกุลไฟล์**: ใช้ `.jsx` เสมอ สำหรับคอมโพเน้นท์ของ React.
  - **ชื่อไฟล์**: ควรตั้งชื่อในรูปแบบของ PascalCase (ขึ้นต้นทุกคำด้วยตัวใหญ่) ตัวอย่างเช่น `ReservationCard.jsx`
  - **ชื่อตัวแปร**: ควรตั้งชื่อในรูปแบบของ PascalCase สำหรับคอมโพเน้นท์ของ React และ camelCase (ขึ้นต้นด้วยตัวเล็กและคำต่อไปขึ้นต้นด้วยตัวใหญ่) สำหรับ Instance อ่านเพิ่มเติมจากกฎของ Eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```javascript
    // ไม่ดี
    import reservationCard from './ReservationCard'; // ควรใช้ PascalCase

    // ดี
    import ReservationCard from './ReservationCard';

    // ไม่ดี
    const ReservationItem = <ReservationCard />; // ควรใช้ camelCase

    // ดี
    const reservationItem = <ReservationCard />;
    ```

  - **ชื่อคอมโพเน้นท์**: ควรตั้งให้เหมือนชื่อไฟล์ ตัวอย่างเช่น ไฟล์ชื่อ `ReservationCard.jsx` ควรตั้งชื่อคอมโพเน้นท์เป็น `ReservationCard` อย่างไรก็ตาม คอมโพเน้นท์ที่เป็นคอมโพเน้นท์หลักของแต่ละโฟเดอร์ให้ตั้งชื่อไฟล์ว่า `index.jsx` และตั้งชื่อคอมโพเน้นท์เป็นชื่อของโฟเดอร์แทน (เพราะสะดวกต่อการอิมพอร์ต เนื่องจากสามารถอิมพอร์ตจากชื่อโฟเดอร์ได้เลย)

    ```javascript
    // ไม่ดี
    import Footer from './Footer/Footer';

    // ไม่ดี
    import Footer from './Footer/index';

    // ดี
    import Footer from './Footer'; // ระบบจะทำการอิมพอร์ตไฟล์ ./Footer/index.js ให้อัตโนมัติ
    ```

## Declaration

  - ไม่ควรใช้พรอพเพอร์ตี้ `displayName` ในการตั้งชื่อคอมโพเน้นท์ แต่ควรตั้งชื่อคอมโพเน้นท์ตอนที่ทำการเอ็กพอร์ตแทน

    ```javascript
    // ไม่ดี
    export default React.createClass({
      displayName: 'ReservationCard',
      // โค้ดอื่นๆ
    });

    // ดี
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment

  - การจัดรูปแบบโค้ดของ JSX นั้นให้ทำตามกฎของ Eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```javascript
    // ไม่ดี
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // ดี
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // ถ้าพรอพเพอร์ตี้ไม่ยาวเกินไปสามารถใส่ไว้ในบรรทัดเดียวกันได้
    <Foo bar="bar" />

    // อิลิเม้นท์ภายในคอมโพเน้นท์ให้จัดย่อหน้าตามปกติ
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux /> // อิลิเม้นท์ย่อหน้าเข้ามาตามปกติ
    </Foo>
    ```

## Quotes

  - ใช้เขี้ยวคู่ (Double quotes) `""` สำหรับ JSX เสมอ แต่สำหรับโค้ดจาวาสคริปต์ทั่วไปให้ใช้เขี้ยวเดี่ยว (Single quotes) `''`  อ่านเพิ่มเติมจากกฎของ Eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > - ทำไม? ในกรณีที่มีอักขระพิเศษภายในแอททริบิวต์ของ JSX [จะไม่สามารถใส่ Escaped quotes ได้](http://eslint.org/docs/rules/jsx-quotes) (ปกติในภาษาจาวาสคริปต์จะสามารถใส่สัญลักษณ์ \\ เพื่อทำการ Escape อักขระพิเศษนั้นๆ แต่ใน JSX ไม่สามารถใช้ได้) และในภาษาอังกฤษมีคำที่มีอักขระพิเศษเขี้ยวเดี่ยวอยู่เยอะพอสมควร จึงควรใช้เขี้ยวคู่เพื่อให้ง่ายต่อการพิมพ์ ตัวอย่างเช่น `"don't"`
  > - ปกติแล้วแอททริบิวต์ของ HTML จะใช้เขี้ยวคู่เสมอ ดังนั้น JSX ควรจะทำตามกฎนั้นเช่นกัน

    ```javascript
    // ไม่ดี
    <Foo bar='bar' />

    // ดี
    <Foo bar="bar" />

    // ไม่ดี
    <Foo style={{ left: "20px" }} />

    // ดี
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - ควรเว้นวรรคหนึ่งทีก่อนทำการปิดแท็ก (Tag)เสมอ

    ```javascript
    // ไม่ดี
    <Foo/>

    // แย่
    <Foo                 />

    // ไม่ดี
    <Foo
     />

    // ดี
    <Foo />
    ```

## Props

  - ควรตั้งชื่อพรอพเพอร์ตี้ในรูปแบบ camelCase เสมอ

    ```javascript
    // ไม่ดี
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // ดี
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - พรอพเพอร์ตี้ที่มีค่าเป็น `true` ควรใส่แค่ชื่อพรอพเพอร์ตี้อย่างเดียวโดยไม่ต้องระบุค่า (React จะใส่ค่า `true` ให้อัตโนมัติ) อ่านเพิ่มเติมจากกฎของ Eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```javascript
    // ไม่ดี
    <Foo
      hidden={true}
    />

    // ดี
    <Foo
      hidden
    />
    ```

  ## Parentheses

  - ควรใส่วงเล็บครอบ JSX ไว้ ในกรณีที่โค้ดมีมากกว่าหนึ่งบรรทัด อ่านเพิ่มเติมจากกฎของ Eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```javascript
    // ไม่ดี
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // ดี
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // ดี
    render() {
      const body = <div>hello</div>; // มีแค่บรรทัดเดียว ไม่ใส่วงเล็บจะทำให้อ่านง่ายขึ้น
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags

  - ควรปิดแท็ก (Tag) ในตัวเองโดยใช้ `/>` ในกรณีที่ภายในแท็กนั้นไม่ประกอบไปด้วยแท็กอื่น อ่านเพิ่มเติมจากกฎของ Eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```javascript
    // ไม่ดี
    <Foo className="stuff"></Foo> // ไม่มีแท็กอื่นภายใน Foo เพราะฉะนั้นควรจะปิดแท็กในตัวมันเอง

    // ดี
    <Foo className="stuff" />
    ```

  - ถ้าคอมโพเน้นท์มีพรอพเพอร์ตี้หลายบรรทัด ควรจะปิดแท็กในบรรทัดใหม่เสมอ อ่านเพิ่มเติมจากกฎของ Eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```javascript
    // ไม่ดี
    <Foo
      bar="bar"
      baz="baz" />

    // ดี
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods

  - ใช้ Arrow function เพื่อครอบตัวแปรภายใน (Local variable)

    ```javascript
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - เมื่อต้องการใช้ฟังก์ชัน `bind()` ในการผูกอีเว้นท์ ควรทำการเรียกใช้ฟังก์ชันในคอนสตรัคเตอร์เสมอ อ่านเพิ่มเติมจากกฎของ Eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > ทำไม? การเรียกฟังก์ชัน bind() ภายในเมท็อต `render()`จะสร้างฟังชันท์ใหม่ทุกครั้งเมื่อมีการเรียกใช้เมท็อต ซึ่งส่งผลกระทบต่อประสิทธิภาพของแอพพลิเคชั่น

    ```javascript
    // ไม่ดี
    class extends React.Component {
      onClickDiv() {
        // โค้ดอื่นๆ
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // ดี
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // โค้ดอื่นๆ
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - อย่าใส่ขีดล่างนำหน้าเมท็อตในคอมโพเน้นท์ของ React

    ```javascript
    // ไม่ดี
    React.createClass({
      _onClickSubmit() {
        // โค้ดอื่นๆ
      },

      // โค้ดอื่นๆ
    });

    // ดี
    class extends React.Component {
      onClickSubmit() {
        // โค้ดอื่นๆ
      }

      // โค้ดอื่นๆ
    }
    ```

## Ordering

  - ควรเรียงลำดับเมท็อตภายใน `class extends React.Component` ดังต่อไปนี้:

  1. optional `static` methods
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - วิธีการประกาศ `propTypes`, `defaultProps`, `contextTypes` และอื่นๆ

    ```javascript
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - หากสร้างคอมโพนเน้นท์ด้วย `React.createClass` ควรเรียงลำดับพรอพเพอร์ตี้ดังต่อไปนี้ อ่านเพิ่มเติมจากกฎของ Eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

## `isMounted`

  - อย่าใช้ฟังก์ชัน `isMounted` อ่านเพิ่มเติมจากกฎของ Eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > ทำไม? เพราะว่า [`isMounted` เป็นแพทเทิร์นที่ควรหลีกเลี่ยง][anti-pattern] ซึ่งมันไม่สามารถใช้ได้ในคลาสของ ES6 นอกจากนั้นฟังก์ชันนี้จะถูกลบในอนาคต

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[[⬆ กลับไปด้านบน]](#TOC)**
