# 배열 메서드

배열은 다양한 메서드를 제공합니다. 학습의 편의를 위해, 본 챕터에선 배열 메서드를 크게 두 그룹으로 나누어 소개하도록 하겠습니다.

## 요소를 추가/제거하는 메서드

배열의 처음이나 끝에 요소를 추가하거나 제거하는 메서드는 이미 학습한 바 있습니다.

- `arr.push(...items)` -- 배열의 끝에 요소를 추가,
- `arr.pop()` -- 배열의 끝 요소를 제거,
- `arr.shift()` -- 배열의 처음 요소를 제거,
- `arr.unshift(...items)` -- 배열의 처음에 요소를 추가.

이 외에 요소를 추가/제거하는 메서드를 알아보도록 하겠습니다.

### splice

배열에서 요소 하나를 지우고 싶다면 어떻게 해야할까요?

배열 역시 객체이므로, 객체 연산자인 `delete`를 사용해 볼 수 있을것입니다.

```js run
let arr = ["I", "go", "home"];

delete arr[1]; // remove "go"

alert( arr[1] ); // undefined

// now arr = ["I",  , "home"];
alert( arr.length ); // 3
```

원하는대로 요소를 지우긴 했지만, 배열은 여전히 세개의 요소가 남아있네요. `arr.length == 3`을 통해 이를 확인할 수 있습니다.

이는 자연스러운 현상입니다. `delete obj.key`는 `key`를 이용해 값을 지우기 때문입니다. `delete` 메서드는 제 역할을 다 한 것입니다. 객체엔 이렇게 해도 괜찮습니다. 하지만 나머지 요소들이 이동해 삭제된 요소의 공간을 채우길 기대하며 이 메서드를 썼을겁니다. 요소를 지운 만큼 배열의 길이가 더 짧아지길 기대하며 말이죠. 

이런 기대를 충족하려면 특별한 메서드를 사용해야 합니다.

[arr.splice(str)](mdn:js/Array/splice)메서드는 만능 스위스 맥가이버 칼 같습니다. 요소를 자유자재로 다룰 수 있게 해주죠. 이 메서드로 요소 추가, 삭제, 삽입이 모두 가능합니다.

문법은 다음과 같습니다.

```js
arr.splice(index[, deleteCount, elem1, ..., elemN])
```

첫 번째 매개변수는 수정을 시작할 `인덱스(index)`입니다. 그다음 매개변수는 `deleteCount`로, 제거하고자 하는 요소의 갯수를 나타냅니다. `elem1, ..., elemN`은 배열에 추가 될 요소입니다.

다양한 예제를 통해 splice 메서드를 이해해 보도록 하겠습니다.

먼저 요소 삭제에 관한 예제부터 살펴보도록 하겠습니다.

```js run
let arr = ["I", "study", "JavaScript"];

*!*
arr.splice(1, 1); // 인덱스 1 부터 요소 한개(1)를 제거합니다.
*/!*

alert( arr ); // ["I", "JavaScript"]
```

쉽죠? 인덱스 `1`부터 시작해 요소 한개(`1`)를 지웠습니다.

다음 코드에선 요소 세개(3)를 지우고, 그 자리를 다른 두개의 요소로 교체해 보도록 하겠습니다.

```js run
let arr = [*!*"I", "study", "JavaScript",*/!* "right", "now"];

// 처음 세개(3)의 요소를 지우고, 이 자리를 다른 요소로 대체합니다.
arr.splice(0, 3, "Let's", "dance");

alert( arr ) // now [*!*"Let's", "dance"*/!*, "right", "now"]
```

아래 코드를 통해 `splice` 메서드는 삭제된 요소로 구성된 배열을 반환한다는 것을 확인할 수 있습니다.

```js run
let arr = [*!*"I", "study",*/!* "JavaScript", "right", "now"];

// 처음 두개의 요소를 삭제함
let removed = arr.splice(0, 2);

alert( removed ); // "I", "study" <-- 삭제된 요소로 구성된 배열
```

`splice` 메서드는 요소를 제거하지 않으면서 요소를 추가해 줄 수도 있습니다. `deleteCount`를 `0`으로 설정하기만 하면 됩니다.

```js run
let arr = ["I", "study", "JavaScript"];

// 인덱스 2 부터
// 0개의 요소를 삭제합니다.
// 그 후, "complex" 와 "language"를 추가합니다.
arr.splice(2, 0, "complex", "language");

alert( arr ); // "I", "study", "complex", "language", "JavaScript"
```

````smart header="음수 인덱스도 사용 가능합니다"
slice메서드와 다른 배열 관련 메서드에 음수 인덱스를 사용할 수 있습니다. 이때 마이너스 부호 앞의 숫자는 배열 끝에서부터 센 요소의 위치를 나타냅니다. 아래와 같이 말이죠.

```js run
let arr = [1, 2, 5];

// 인덱스 -1 부터 (배열 끝에서부터 첫번째 요소)
// 0 개의 요소를 삭제하고,
// 3 과 4를 추가해줍니다.
arr.splice(-1, 0, 3, 4);

alert( arr ); // 1,2,3,4,5
```
````

### slice

[arr.slice](mdn:js/Array/slice)메서드는 `arr.splice`와 비슷해보이지만, 훨씬 간단한 메서드입니다.

문법:

```js
arr.slice(start, end)
```

이 메서드는 `"start"` 인덱스부터 (`"end"`를 제외한) `"end"`인덱스 까지의 요소를 포함하는 메서드를 반환합니다. `start` 와 `end` 인덱스는 둘 다 음수일수도 있습니다. 인덱스가 음수일 땐 배열의 끝에서부터의 요소 갯수를 의미합니다.

`arr.slice`메서드의 동작은 문자열 메서드인 `str.slice`와 유사하지만, `arr.slice`메서드는 서브 문자열(substring) 대신 서브배열(subarray)를 반환한다는 점이 다릅니다.

예:

```js run
let str = "test";
let arr = ["t", "e", "s", "t"];

alert( str.slice(1, 3) ); // es
alert( arr.slice(1, 3) ); // e,s

alert( str.slice(-2) ); // st
alert( arr.slice(-2) ); // s,t
```

### concat

[arr.concat](mdn:js/Array/concat) 메서드는 배열의 끝에 다른 배열이나 요소를 추가해줍니다.

문법은 다음과 같습니다.

```js
arr.concat(arg1, arg2...)
```

인수의 갯수는 제한이 없고, 인수로 배열이나 값을 받을 수 있습니다.

메서드를 적용한 결과는 `arr`에 속한 모든 요소와, `arg1`, `arg2` 등에 속한 모든 요소를 합친 배열입니다.

인수가 배열이거나, 인수의 프로퍼티가 `Symbol.isConcatSpreadable`라면, 인수로 받은 배열의 모든 요소를 복사합니다. 이 외에는 인수 자체를 복사합니다.

예시:

```js run
let arr = [1, 2];

// 배열 arr을 배열 [3,4]와 병합함
alert( arr.concat([3, 4])); // 1,2,3,4

// 배열 arr을 배열 [3,4]와 배열 [5,6]과 병합함
alert( arr.concat([3, 4], [5, 6])); // 1,2,3,4,5,6

// 배열 arr을 배열 [3,4]와 병합하고, 값 5 와 6을 추가함
alert( arr.concat([3, 4], 5, 6)); // 1,2,3,4,5,6
```

일반적으로, concat 메서드는 제공받은 배열의 요소를 ("분해"해서) 복사합니다. 객체는 통으로 복사되어 더해집니다. 배열 처럼 보이는 객체도 마찬가지로 전체가 복사됩니다. 

```js run
let arr = [1, 2];

let arrayLike = {
  0: "something",
  length: 1
};

alert( arr.concat(arrayLike) ); // 1,2,[object Object]
//[1, 2, arrayLike]
```

하지만 유사배열 객체에 `Symbol.isConcatSpreadable` 프로퍼티가 있으면, 객체의 요소가 더해집니다.

```js run
let arr = [1, 2];

let arrayLike = {
  0: "something",
  1: "else",
*!*
  [Symbol.isConcatSpreadable]: true,
*/!*
  length: 2
};

alert( arr.concat(arrayLike) ); // 1,2,something,else
```

## Iterate: forEach

[arr.forEach](mdn:js/Array/forEach) 메서드는 주어진 함수를 배열 요소 각각에 대해 실행할 수 있게 해줍니다.

문법은 다음과 같습니다.
```js
arr.forEach(function(item, index, array) {
  // 요소(item)에 무언갈 할 수 있습니다.
});
```

아래는 배열의 각 요소를 보여주는 코드입니다.

```js run
// for each element call alert
["Bilbo", "Gandalf", "Nazgul"].forEach(alert);
```

아래는 타깃 배열의 인덱스(index)까지 출력해주는 좀 더 정교한 코드입니다.

```js run
["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
  alert(`${item} is at index ${index} in ${array}`);
});
```

함수의 반환값은 (반환값을 어떻게 명시해 줬던 간에) 무시됩니다(역주: 결국 forEach 메서드의 반환값은 undefined 가 됩니다).


## 배열 탐색하기

배열 내에서 뭔가를 찾을 때 쓰이는 메서드가 있습니다. 

### indexOf/lastIndexOf and includes

배열의 [arr.indexOf](mdn:js/Array/indexOf), [arr.lastIndexOf](mdn:js/Array/lastIndexOf), [arr.includes](mdn:js/Array/includes) 같은 이름을 가진 문자열 메서드와 동일한 문법을 사용하고 하는일도 본질적으로 같지만, 연산 대상이 문자열이 아닌 배열의 요소라는 점만 다릅니다.

- `arr.indexOf(item, from)`는 인덱스 `from`부터 시작해 해당하는 `요소(item)`을 찾습니다. 일치하는 요소를 발견하면 해당하는 요소의 인덱스를 반환하고 그렇지 않다면 `-1`을 반환합니다.
- `arr.lastIndexOf(item, from)`는 위 메서드와 동일한 기능을 하는 메서드이나, 검색을 끝에서 부터 시작한다는 점만 다릅니다.
- `arr.includes(item, from)`는 인덱스 `from`부터 시작해 배열에 해당하는 `요소(item)`가 있는지를 검색하는데, 해당하는 요소를 발견하면 `true` 를 반환합니다.

예시 코드를 살펴보겠습니다.

```js run
let arr = [1, 0, false];

alert( arr.indexOf(0) ); // 1
alert( arr.indexOf(false) ); // 2
alert( arr.indexOf(null) ); // -1

alert( arr.includes(1) ); // true
```
위 메서드들은 요소를 찾을 때 완전 항등 연산자인 `===` 를 사용한다는 점에 유의하시기 바랍니다. 보시는 바와 같이 `false`를 검색하면 정확히 `false`만을 검색하지, 0을 검색하진 않습니다.

요소의 위치를 정확히 알고 싶은게 아니고, 배열내 존재 여부만 확인하고 싶다면 `arr.includes`를 사용하는게 좋습니다.

`includes` 메서드는 `NaN`도 찾아낼 수 있다는 점에서 `indexOf/lastIndexOf`메서드와 약간의 차이가 있습니다.

```js run
const arr = [NaN];
alert( arr.indexOf(NaN) ); // -1 (0이 출력되길 기대하지만, 완전 항등 비교 === 는 NaN엔 작동하지 않습니다.)
alert( arr.includes(NaN) );// true (기대하는 대로 NaN의 여부를 확인하였습니다.)
```

### find 와 findIndex

객체로 이루어진 배열이 있다고 가정해 봅시다. 특정 조건을 가진 객체를 배열 내에서 어떻게 찾아낼 수 있을까요?

[arr.find](mdn:js/Array/find)메서드는 이럴 때 쓸모가 있습니다.

문법은 다음과 같습니다.
```js
let result = arr.find(function(item, index, array) {
  // true가 반환되면, 반복이 멈추고 해당 요소(item)을 반환합니다.
  // 거짓 같은 값(falsy)일 경우는 undefined를 반환합니다.
});
```

배열 내 모든 요소에 대하여 함수가 호출됩니다. 이때,

- `item` 은 요소를 의미합니다.
- `index` 는 인덱스를 의미합니다.
- `array` 는 배열 그 자체를 의미합니다.

find 메서드가 참을 반환하면, 탐색은 중단되고 해당 `요소(item)`가 반환됩니다. 아무것도 찾지 못했을 경우는 `undefined`가 반환됩니다.

`id`와 `name`이 있는 사용자 객체가 들어있는 배열을 예로 들어보도록 하겠습니다. 배열 내에서 `id == 1` 조건을 충족하는 사용자 객체를 찾아봅시다.

```js run
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

let user = users.find(item => item.id == 1);

alert(user.name); // John
```

객체로 구성된 배열은 실 생활에서 아주 흔한 경우이기 때문에, `find` 메서드는 아주 유용합니다.

위 예제에선 인수가 하나만 있는 함수인 `item => item.id == 1`을 `find` 메서드에 넘겨주었다는 점에 유의하시기 바랍니다. 다른 인수는 거의 쓰이지 않고 있습니다.

[arr.findIndex](mdn:js/Array/findIndex) 메서드는 find 메서드와 동일한 일을 하나, 조건에 맞는 요소를 반환하지 않고, 해당 요소의 인덱스만 반환한다는 점에서 차이가 있습니다. 아무것도 찾지 못한 경우는 `-1`을 반환합니다. 

### filter

`find` 메서드는 함수의 반환값을 `true`로 만드는 단 하나의 요소를 찾습니다. 요소를 찾게 되면 탐색이 중단되기 때문에, 조건에 맞는 첫 번 째 요소만 반환됩니다.

만약 조건을 충족하는 요소가 여러개라면, [arr.filter(fn)](mdn:js/Array/filter)를 사용해 해당하는 객체를 찾을 수 있습니다.

문법은 `find`와 비슷합니다. 하지만 filter 메서드는 `true`가 이미 반환된 경우에도 탐색을 멈추지 않기 때문에 배열의 모든 요소를 검색한다는 점에서 차이가 있습니다.

```js
let results = arr.filter(function(item, index, array) {
  // 조건을 만족하는 요소가 반환되더라도 탐색은 멈추지 않습니다.
  // 모든 요소가 조건을 충족하지 않으면, 빈 배열이 반환됩니다.
});
```

예시:

```js run
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

// 앞쪽 두명의 사용자를 반환합니다.
let someUsers = users.filter(item => item.id < 3);

alert(someUsers.length); // 2
```

## 배열을 변형시키는 메서드

This section is about the methods transforming or reordering the array.
배열을 변형하거나 재 정렬하는 메서드에 대해 다뤄보도록 하겠습니다.


### map

[arr.map](mdn:js/Array/map) 메서드는 유용성과 사용 빈도가 아주 높은 메서드 중 하나입니다.

문법은 다음과 같습니다.

```js
let result = arr.map(function(item, index, array) {
  // 요소가 아닌 새로운 값을 반환합니다
})
```

map메서드는 배열의 각 요소에 함수를 호출하고, 결과를 배열로 받아 반환합니다.   

아래는 map 메서드를 활용해 각 요소의 길이를 출력해주는 예시입니다.

```js run
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
alert(lengths); // 5,7,6
```

### sort(fn)

[arr.sort](mdn:js/Array/sort) 메서드는 배열의 요소를 *제 자리에서* 정렬해줍니다.

예시:

```js run
let arr = [ 1, 2, 15 ];

// sort 메서드는 arr의 내용을 재정렬 하고, 재정렬된 배열을 반환합니다.
arr.sort();

alert( arr );  // *!*1, 15, 2*/!*
```

결과에서 뭔가 이상한 걸 발견하지 않으셨나요?

순서가 `1, 15, 2`가 되었습니다. 기대하던 결과와는 다르네요. 왜 이런 결과가 나왔을까요?

**요소는 문자열로 취급되어 정렬됩니다.**

모든 요소는 문자열로 변환되고 나서 비교됩니다. 사전순으로 재정렬 되기 때문에, `"2" > "15"`라는 결과가 도출되었습니다.

문자열 기준이 아닌, 다른 기준을 만들어 정렬하고 싶다면 인수 두개를 받는 함수를 만들어 `arr.sort()`의 인수로 전달해 주면 됩니다.

인수 두개를 받는 함수는 아래와 같은 형태이어야 합니다.
```js
function compare(a, b) {
  if (a > b) return 1;
  if (a == b) return 0;
  if (a < b) return -1;
}
```

예시:

```js run
function compareNumeric(a, b) {
  if (a > b) return 1;
  if (a == b) return 0;
  if (a < b) return -1;
}

let arr = [ 1, 2, 15 ];

*!*
arr.sort(compareNumeric);
*/!*

alert(arr);  // *!*1, 2, 15*/!*
```

이제 의도한 대로 숫자가 오름차순으로 정렬되었습니다.

읽는 걸 멈추고 이 메서드가 어떻게 동작하는지 잠시 생각해 보도록 합시다. `arr`의 요소는 어떤 형태의 값이든 가능합니다. 숫자, 문자열, html 요소 등 모든것이 가능하죠. *무언가*로 구성된 집합이 있는 상황입니다. 이 집합을 정렬하려면 *순서를 매겨주는 함수*가 필요합니다. 요소를 어떤 기준으로 비교하고 정렬할지를 이 함수에서 정의합니다. sort 메서드는 기본적으로 문자열 집합을 가정하고 요소를 비교, 정렬합니다.

`arr.sort(fn)` 메서드의 정렬 알고리즘은 내부에 구현되어 있습니다. (대부분 최적화된 [quicksort](https://en.wikipedia.org/wiki/Quicksort)를 사용하는데) 내부 알고리즘이 어떻게 작동하는지는 상관할 필요가 없습니다. 내부에 구현된 알고리즘은 배열내를 돌아다니며 요소를 비교하고, 제공된 함수를 기준으로 요소를 재 정렬합니다. 개발자는 비교에 쓰이는 `fn` 만 제공해 주면 됩니다.  

정렬 과정에서 어떤 요소끼리 비교가 일어났는지 확인하고 싶다면 아래 코드를 통해 확인하면 됩니다.

```js run
[1, -2, 15, 2, 0, 8].sort(function(a, b) {
  alert( a + " <> " + b );
});
```

정렬에 쓰이는 알고리즘은 정렬 과정에서 요소를 여러번 비교합니다. 하지만 비교를 가능한 한 적게 하는 방식으로 구현되어 있습니다.


````smart header="비교에 쓰이는 함수는 어떤 숫자던 반환할 수 있습니다."
Actually, a comparison function is only required to return a positive number to say "greater" and a negative number to say "less".

That allows to write shorter functions:

```js run
let arr = [ 1, 2, 15 ];

arr.sort(function(a, b) { return a - b; });

alert(arr);  // *!*1, 2, 15*/!*
```
````

````smart header="Arrow functions for the best"
Remember [arrow functions](info:function-expressions-arrows#arrow-functions)? We can use them here for neater sorting:

```js
arr.sort( (a, b) => a - b );
```

This works exactly the same as the other, longer, version above.
````

### reverse

The method [arr.reverse](mdn:js/Array/reverse) reverses the order of elements in `arr`.

For instance:

```js run
let arr = [1, 2, 3, 4, 5];
arr.reverse();

alert( arr ); // 5,4,3,2,1
```

It also returns the array `arr` after the reversal.

### split and join

Here's the situation from the real life. We are writing a messaging app, and the person enters the comma-delimited list of receivers: `John, Pete, Mary`. But for us an array of names would be much more comfortable than a single string. How to get it?

The [str.split(delim)](mdn:js/String/split) method does exactly that. It splits the string into an array by the given delimiter `delim`.

In the example below, we split by a comma followed by space:

```js run
let names = 'Bilbo, Gandalf, Nazgul';

let arr = names.split(', ');

for (let name of arr) {
  alert( `A message to ${name}.` ); // A message to Bilbo  (and other names)
}
```

The `split` method has an optional second numeric argument -- a limit on the array length. If it is provided, then the extra elements are ignored. In practice it is rarely used though:

```js run
let arr = 'Bilbo, Gandalf, Nazgul, Saruman'.split(', ', 2);

alert(arr); // Bilbo, Gandalf
```

````smart header="Split into letters"
The call to `split(s)` with an empty `s` would split the string into an array of letters:

```js run
let str = "test";

alert( str.split('') ); // t,e,s,t
```
````

The call [arr.join(separator)](mdn:js/Array/join) does the reverse to `split`. It creates a string of `arr` items glued by `separator` between them.

For instance:

```js run
let arr = ['Bilbo', 'Gandalf', 'Nazgul'];

let str = arr.join(';');

alert( str ); // Bilbo;Gandalf;Nazgul
```

### reduce/reduceRight

When we need to iterate over an array -- we can use `forEach`, `for` or `for..of`.

When we need to iterate and return the data for each element -- we can use `map`.

The methods [arr.reduce](mdn:js/Array/reduce) and [arr.reduceRight](mdn:js/Array/reduceRight) also belong to that breed, but are a little bit more intricate. They are used to calculate a single value based on the array.

The syntax is:

```js
let value = arr.reduce(function(previousValue, item, index, array) {
  // ...
}, initial);
```

The function is applied to the elements. You may notice the familiar arguments, starting from the 2nd:

- `item` -- is the current array item.
- `index` -- is its position.
- `array` -- is the array.

So far, like `forEach/map`. But there's one more argument:

- `previousValue` -- is the result of the previous function call, `initial` for the first call.

The easiest way to grasp that is by example.

Here we get a sum of array in one line:

```js run
let arr = [1, 2, 3, 4, 5];

let result = arr.reduce((sum, current) => sum + current, 0);

alert(result); // 15
```

Here we used the most common variant of `reduce` which uses only 2 arguments.

Let's see the details of what's going on.

1. On the first run, `sum` is the initial value (the last argument of `reduce`), equals `0`, and `current` is the first array element, equals `1`. So the result is `1`.
2. On the second run, `sum = 1`, we add the second array element (`2`) to it and return.
3. On the 3rd run, `sum = 3` and we add one more element to it, and so on...

The calculation flow:

![](reduce.png)

Or in the form of a table, where each row represents a function call on the next array element:

|   |`sum`|`current`|`result`|
|---|-----|---------|---------|
|the first call|`0`|`1`|`1`|
|the second call|`1`|`2`|`3`|
|the third call|`3`|`3`|`6`|
|the fourth call|`6`|`4`|`10`|
|the fifth call|`10`|`5`|`15`|


As we can see, the result of the previous call becomes the first argument of the next one.

We also can omit the initial value:

```js run
let arr = [1, 2, 3, 4, 5];

// removed initial value from reduce (no 0)
let result = arr.reduce((sum, current) => sum + current);

alert( result ); // 15
```

The result is the same. That's because if there's no initial, then `reduce` takes the first element of the array as the initial value and starts the iteration from the 2nd element.

The calculation table is the same as above, minus the first row.

But such use requires an extreme care. If the array is empty, then `reduce` call without initial value gives an error.

Here's an example:

```js run
let arr = [];

// Error: Reduce of empty array with no initial value
// if the initial value existed, reduce would return it for the empty arr.
arr.reduce((sum, current) => sum + current);
```


So it's advised to always specify the initial value.

The method [arr.reduceRight](mdn:js/Array/reduceRight) does the same, but goes from right to left.


## Array.isArray

Arrays do not form a separate language type. They are based on objects.

So `typeof` does not help to distinguish a plain object from an array:

```js run
alert(typeof {}); // object
alert(typeof []); // same
```

...But arrays are used so often that there's a special method for that: [Array.isArray(value)](mdn:js/Array/isArray). It returns `true` if the `value` is an array, and `false` otherwise.

```js run
alert(Array.isArray({})); // false

alert(Array.isArray([])); // true
```

## Most methods support "thisArg"

Almost all array methods that call functions -- like `find`, `filter`, `map`, with a notable exception of `sort`, accept an optional additional parameter `thisArg`.

That parameter is not explained in the sections above, because it's rarely used. But for completeness we have to cover it.

Here's the full syntax of these methods:

```js
arr.find(func, thisArg);
arr.filter(func, thisArg);
arr.map(func, thisArg);
// ...
// thisArg is the optional last argument
```

The value of `thisArg` parameter becomes `this` for `func`.

For instance, here we use an object method as a filter and `thisArg` comes in handy:

```js run
let user = {
  age: 18,
  younger(otherUser) {
    return otherUser.age < this.age;
  }
};

let users = [
  {age: 12},
  {age: 16},
  {age: 32}
];

*!*
// find all users younger than user
let youngerUsers = users.filter(user.younger, user);
*/!*

alert(youngerUsers.length); // 2
```

In the call above, we use `user.younger` as a filter and also provide `user` as the context for it. If we didn't provide the context, `users.filter(user.younger)` would call `user.younger` as a standalone function, with `this=undefined`. That would mean an instant error.

## Summary

A cheatsheet of array methods:

- To add/remove elements:
  - `push(...items)` -- adds items to the end,
  - `pop()` -- extracts an item from the end,
  - `shift()` -- extracts an item from the beginning,
  - `unshift(...items)` -- adds items to the beginning.
  - `splice(pos, deleteCount, ...items)` -- at index `pos` delete `deleteCount` elements and insert `items`.
  - `slice(start, end)` -- creates a new array, copies elements from position `start` till `end` (not inclusive) into it.
  - `concat(...items)` -- returns a new array: copies all members of the current one and adds `items` to it. If any of `items` is an array, then its elements are taken.

- To search among elements:
  - `indexOf/lastIndexOf(item, pos)` -- look for `item` starting from position `pos`, return the index or `-1` if not found.
  - `includes(value)` -- returns `true` if the array has `value`, otherwise `false`.
  - `find/filter(func)` -- filter elements through the function, return first/all values that make it return `true`.
  - `findIndex` is like `find`, but returns the index instead of a value.
  
- To iterate over elements:
  - `forEach(func)` -- calls `func` for every element, does not return anything.

- To transform the array:
  - `map(func)` -- creates a new array from results of calling `func` for every element.
  - `sort(func)` -- sorts the array in-place, then returns it.
  - `reverse()` -- reverses the array in-place, then returns it.
  - `split/join` -- convert a string to array and back.
  - `reduce(func, initial)` -- calculate a single value over the array by calling `func` for each element and passing an intermediate result between the calls.

- Additionally:
  - `Array.isArray(arr)` checks `arr` for being an array.

Please note that methods `sort`, `reverse` and `splice` modify the array itself.

These methods are the most used ones, they cover 99% of use cases. But there are few others:

- [arr.some(fn)](mdn:js/Array/some)/[arr.every(fn)](mdn:js/Array/every) checks the array.

  The function `fn` is called on each element of the array similar to `map`. If any/all results are `true`, returns `true`, otherwise `false`.

- [arr.fill(value, start, end)](mdn:js/Array/fill) -- fills the array with repeating `value` from index `start` to `end`.

- [arr.copyWithin(target, start, end)](mdn:js/Array/copyWithin) -- copies its elements from position `start` till position `end` into *itself*, at position `target` (overwrites existing).

For the full list, see the [manual](mdn:js/Array).

From the first sight it may seem that there are so many methods, quite difficult to remember. But actually that's much easier than it seems.

Look through the cheatsheet just to be aware of them. Then solve the tasks of this chapter to practice, so that you have experience with array methods.

Afterwards whenever you need to do something with an array, and you don't know how -- come here, look at the cheatsheet and find the right method. Examples will help you to write it correctly. Soon you'll automatically remember the methods, without specific efforts from your side.
