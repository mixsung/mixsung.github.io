---
title: "Learn React 읽기 - (1) immutable과 mutable의 차이를 State와 연관지어 설명하시오"
excerpt: "Learn React를 읽다가 궁금해진 Javascript의 immutable, primitive, 재할당을 알아보았습니다."
categories: react
tags: react javascript immutable mutable auto-box
---

<br>

```
이 글은 Learn React의 Adding Interactivity - Updating Objects in State를 읽고 작성한 글입니다.
```

<br>

Learn React <https://react.dev/learn/updating-objects-in-state>

Learn React를 원문으로 처음부터 쭉 읽고 있습니다. Adding Interactivity 챕터는 Describing the UI 챕터에서 아쉬웠던 깊이를 채워주는 챕터였던 것 같아요. 나름 재미있게 읽었습니다. 한 챕터가 끝날 때마다 연습문제가 있어서 좋았습니다.

> So far you’ve been working with numbers, strings, and booleans. These kinds of JavaScript values are “immutable”, meaning unchangeable or “read-only”.  
> Technically, it is possible to change the contents of the object itself. This is called a mutation.  
> However, although objects in React state are technically mutable, you should treat them as if they were immutable—like numbers, booleans, and strings. Instead of mutating them, you should always replace them.

오랜만에 보는 immutable과 mutable의 개념이 등장하자 갑자기 헷갈렸습니다.  
immutable한 것의 특징이 뭐였지? mutable은 주소값만 바뀌는거였나? 그래서 이번 기회에 개념을 제대로 잡고 가기 위해 이 글을 작성합니다.

<br>

## Immutable의 뜻

*immutable*은 "변경할 수 없는, 불변의" 뜻을 가지고 있는데 프로그래밍에서의 *immutabl*e은 아래와 같습니다.

1. 한 번 생성된 이후 변경할 수 없는 값
2. 변경을 위해서는 원래 메모리는 그대로 두고 다른 메모리 주소에 새 값을 만들어야 함

2번에 대해서는 더 아래에서 재할당과 관련해 자세히 얘기해보겠습니다.

보통 모든 _primitive_(원시값)은 *immutable*하다고 말합니다.  
*primitive*는 프로그래밍 언어에서 가장 기본적인 데이터 유형이며 단순한 값을 나타냅니다.

C에서는 int, char, float, void가 있고  
Java에서는 int, float, double, char, boolean, long, short, byte  
Python에서는 int, float, bool, str  
Javascript에서는 number, string, boolean, undefined, null, symbol, bigint가 있습니다.

<details>
<summary>C언어에서는 primitive가 immutable하지 않다?</summary>
<div markdown="1">
<br>int a = 42;<br>a = 0;<br>
<br>int형 변수 a의 주소값은 재할당 이후 바뀔까?<br>
<br>실제로 주소값을 찍어보면 바뀌지 않는 것을 확인할 수 있습니다.
<br>주소값이 바뀌지 않는다는 것은 새로운 주소에 새 값을 생성하지 않고 실제 값을 덮어씌운다는 의미입니다.<br>
<br>C언어의 primitive 유형은 const로 immutable을 설정할 수 있으며 기본적으로 mutable합니다.
<br>다른 언어의 immutable 데이터 유형들은 재할당시 새로운 주소에 새로운 값을 생성합니다.
</div>
</details>
<br>

Javascript에서 데이터 유형은 크게 2가지로 구분합니다.

1. _primitive type_(원시 타입)
2. _object/reference type_(객체 타입)

*primitive type*은 메모리에 직접적으로 저장된다는 특징이 있고 이 특징은 *object/reference type*과 반대입니다.  
*object/reference type*은 데이터 자체가 아닌 **실제 데이터가 저장된 메모리 위치**를 가리킵니다.

사실 Javascript에서 메모리에 직접적으로 저장된다는 설명은 *dynamic typing*이라는 부가설명이 필요한데  
조금 더 알고 싶다면 제가 쓴 "C와 Javascript의 차이는 dynamic typing" 를 읽어주세요.

<br>

## immutable한 것은 재할당이 불가능한 게 아니다

위에서 나왔던 *immutable*의 2번째 뜻은 실제 값을 덮어씌울 수 없는 대신 새로운 메모리 주소에 값이 생성된다는 뜻입니다.  
값 자체가 변할 수 없다는 것이지 변수는 변할 수 있습니다.  
변수를 재할당 할 수 있지만 새 값의 메모리 주소는 달라지게 됩니다.

<br>

```
// Javascript

var age = 100; // 변수의 선언과 값의 할당
age = 15;      // 값의 재할당
```

var 키워드로 선언한 변수는 값을 재할당할 수 있습니다.  
var 변수는 선언과 동시에 undefined로 초기화됩니다.  
그래서 엄밀히 말하면 변수에 처음으로 값을 할당하는 것도 사실은 재할당이라고 합니다.  
100이 저장되어있던 메모리 공간을 지우고 그 메모리 공간에 재할당 값 15를 새로 저장하는 게 아니라  
새로운 메모리 공간을 확보하고 그 메모리 공간에 숫자 값 15를 저장합니다.

변수는 이제 새로운 메모리 공간을 가리키게 됩니다.  
메모리 어딘가에 저장되어있는 값 undefined과 100은 아무도 가리키고 있지 않아서 더 이상 필요하지 않습니다.  
이러한 불필요한 값들은 _garbage collector_ (가비지 콜렉터)에 의해 메모리에서 자동 해제됩니다.

_garbage collector_ 덕분에 자동으로 메모리 누수를 방지할 수 있습니다.

<br>

## mutable의 뜻

immutable을 알아봤으니 mutable에 대해서도 알아보겠습니다.  
immutable과 반대인 mutable은 Javascript 기준으로 설명하면 object, array, function이 있습니다.  
함수가 왜 mutable하냐면, Javascript에서는 함수는 객체이기 때문입니다.  
객체이지만 더 유연하게 동작하는 [_First-class Objects_ (일급 객체)](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function "MDN First-class Function")에 해당됩니다. 다른 함수로 전달되거나 반환받을 수 있고, 변수와 속성을 할당받을 수 있습니다.  
함수는 객체이기때문에 앞으로 생략하고 설명하겠습니다.

immutable은 값 자체가 바뀔 수 없고, 바꾸고 싶다면 새 메모리 공간에 생성하면 된다고 했습니다.  
mutable인 객체, 배열은 _Reference_ 방식으로 저장됩니다. 처음부터 값 자체를 저장하는 게 아니라 값이 저장된 메모리 주소에 대한 *Reference*를 저장합니다.

```
var customer = {
    name: 'Kim'
};

console.log(customer); // {name: 'Kim'}
customer.name = 'Park'; // 값 갱신
```

customer 변수는 객체 {name: 'Kim'}을 가리키고 있습니다. (=참조)  
primitive 값은 immutable하므로 값을 변경하려면 재할당이 필요했습니다.  
객체, 배열은 재할당 없이 **직접 변경할 수 있습니다.**  
재할당하지 않기때문에 변수의 reference 값은 변경되지않습니다.

<br>

## 객체, 배열, 함수는 mutable하지만 React State에서는 immutable하다고 생각하자

> However, although objects in React state are technically mutable, you should treat them **as if** they were immutable—like numbers, booleans, and strings.  
> Instead of mutating them, you should always replace them.  
> In other words, you should **treat any JavaScript object that you put into state as read-only.**

immutable과 mutable에 대해 정리하고 다시 읽어보니 "immutable-like numbers, booleans, and strings" 이런 문장이 훨씬 이해가 잘 됩니다.  
immutable하게 취급하라는 것은 재할당이 필요하고 주소값이 바뀌겠다 라는 생각이 자연스럽게 떠오릅니다.  
React State에서 객체를 변경할 때 왜 원본 값을 복사를 하고 변경을 하는지 알게 됩니다.

Javascript의 객체는 mutable하기 때문에 직접 수정할 수 있습니다.  
그리고 React State에서도 그렇게 할 수는 있습니다.  
하지만 에러를 일으킬 가능성이 높습니다.

React는 State를 업데이트 할 때 **새 객체를 생성할 것을 예상하고** State에 대한 포인터가 변경되었는지 확인하기때문입니다.  
기존 State를 **직접 변경하면 포인터가 동일하니** React는 변경사항을 모를 수 있고 이로 인해 **렌더링이 되지 않습니다.**  
*Triggering a re-render*는 포인터가 달라질 때입니다.

<br>

```
const [person, setPerson] = useState({
  name: 'Niki de Saint Phalle',
  artwork: {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  }
});

const nextArtwork = { ...person.artwork, city: 'New Delhi' }; // artwork에서 city만 변경할 때
const nextPerson = { ...person, artwork: nextArtwork };
setPerson(nextPerson);
```

React State에서 객체 state를 업데이트 할 때, 새 객체를 생성하는데 기존 객체를 *spread operator*로 복사하고 이는 _Shallow copy_ (얕은 복사)입니다.  
얕은 복사는 최상위 속성만 복사하며, 중첩된 객체나 배열은 원래 객체의 참조로 유지됩니다.  
React는 컴포넌트를 다시 렌더링할지 여부를 포인터로 확인하기 때문에 얕은 복사만으로 상태관리를 할 수 있습니다.  
React에서는 객체뿐만 아니라 state를 immutable로 취급해야합니다.

<br>

<p align="center" style="color:gray; text-align:center;">
  <!-- 마진은 위아래만 조절하는 것이 정신건강에 좋을 듯 하다. 이미지가 커지면 깨지는 경우가 있는 듯 하다.-->
  <img src="https://github.com/user-attachments/assets/9a63cc15-ce53-44fb-9b45-9fd7c980b4db" alt="updating-state-array" width="600"/>
  <br>
  출처: React Learn 'Updating Arrays in State'
</p>

React 상태 관리에서 배열도 immutable로 취급해야합니다.  
배열의 요소를 변경하는 방법은 위의 표처럼 2가지가 있습니다.  
배열은 직접 변경하거나 업데이트된 값으로 새 배열을 생성합니다.

state에 저장된 배열은 객체처럼 새 배열을 생성하여 새로운 참조를 React에게 제공해야합니다.  
<br>

#### 참고

---

- 이웅모 (2020) _모던 자바스크립트 Deep Dive : 자바스크립트의 기본 개념과 동작 원리_. 위키북스.
- MDN Primitive https://developer.mozilla.org/en-US/docs/Glossary/Primitive
- Learn React https://react.dev/learn/updating-objects-in-state
