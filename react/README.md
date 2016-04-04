# Airbnb React/JSX Style Guide

**คู่มือแนะนำการเขียน React และ JSX ที่เข้าท่ามากที่สุด** โดย [Airbnb](https://github.com/airbnb/javascript/)
> คู่มือนี้ผมแปลโดยใส่คำอธิบายและตัวอย่างเพิ่มเติม (ไม่แปลตรงตัว) เพื่อให้ผู้อ่านสามารถเข้าใจเนื้อหาต่างๆได้ดียิ่งขึ้น ในกรณีที่เจอข้อผิดพลาดใดๆ กรุณา Fork และ PR ถ้ามีคำถามสามารถเปิด Issue ได้เลยครับ หวังว่าคู่มือนี้จะมีประโยชน์ต่อผู้อ่านไม่มากก็น้อย :pray:

## Table of Contents

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
    - ยกเว้นคอมโพเน้นท์ประเภท [Stateless หรือ Pure Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) (คอมโพเน้นท์ที่ไม่มีการจัดการ State ในตัวเอง) สามารถที่จะเขียนรวมในไฟล์เดียวกันได้ อ่านเพิ่มเติมจากกฏของ Eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - ใช้ JSX ร่วมกับ React เสมอ
  - อย่าใช้ `React.createElement` ยกเว้นในการณีที่ต้องจัดการกับแอพพลิเคชั่นที่มีไฟล์ที่ไม่ได้ใช้ JSX เท่านั้น

## Class vs `React.createClass` vs stateless

  - ถ้าภายในคอมโพเน้นท์มีการจัดการ State และ refs ให้เลือกใช้ `class extends React.Component` แทนการใช้ `React.createClass` ยกเว้นเราต้องการใช้งาน Mixins (Mixins ใช้งานได้กับ React.createClass เท่านั้น ไม่สามารถใช้ร่วมกับคลาสของ ES6 ได้) อ่านเพิ่มเติมจากกฏของ Eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

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
  - **ชื่อตัวแปร**: ควรตั้งชื่อในรูปแบบของ PascalCase สำหรับคอมโพเน้นท์ของ React และ camelCase (ขึ้นต้นด้วยตัวเล็กและคำต่อไปขึ้นต้นด้วยตัวใหญ่) สำหรับ Instance อ่านเพิ่มเติมจากกฏของ Eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

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

  - การจัดรูปแบบโค้ดของ JSX นั้นให้ทำตามกฏของ Eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

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

**[⬆ กลับไปด้านบน](#table-of-contents)**
