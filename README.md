# about_Javascript
자바스크립트를 공부하며 알게 된 것을 기록하는 저장소입니다.
<br/><br/>

# Ajax & JSON
- datetype : "json" -> 서버에서 보내준 response의 타입을 명시. 서버로 데이터를 보내기만하고 받지 않는 경우 명시하면 안된다. 
<br/><br/>

# Bubbling - 버블링이란?
- 중첩된 요소에서 이벤트가 발생할 때, HTML DOM APLI의 이벤트 전파(Event Propagation)은 두가지 방식으로 구분.  -> 버블링 vs 캡처링
- 캡처링 : window로부터 이벤트 발생 요소까지 이벤트 전파
- 버블링 : 이벤트 발생 요소부터 window까지 이벤트 전파
<br/><br/>

# Comma operator 쉼표 연산자
- 쉼표 연산자는 각각의 피연산자를 왼쪽에서 오른쪽 순서로 평가하고, 마지막 연산자의 값을 반환. 
- 콤마를 분리자와 연사자 두 가지로 표현 가능.
```javascript
for(var i=0,j=10;i<=j;i++,j--) {
    console.log(i*j); 
}
// i=0,j=10 -> 분리자, i++,j-- -> 연산자
```
```javascript
function nextFibonacci() {
    next = a + b; 
    a = b; 
    b = next; 
    return next; 
}

// 쉼표 연산자를 이용해서 깔끔하게 표현 가능
function nextFibonacci() {
    next = a + b; 
    return b = (a = b, next); 
}
// a = b를 먼저 실행, next를 리턴.

function fn() {
    if(x){ 
        foo(); 
        return bar(); 
    }else{ 
        return 1; 
    } 
}

// 쉼표 연산자를 이용해서 깔끔하게 표현 가능
function fn() { 
  return x ? (foo(), bar()) : 1; 
}
// x가 참일 경우, foo()를 실행 한 뒤 bar()를 리턴.
```
- 출처 : https://mygumi.tistory.com/50
<br/><br/>

# null vs undefined
: undefined -> 아직 존재하지 않거나 더 이상 존재하지 않는 것을 의미. <br/>
: null -> 존재하지, 비어 있는 것을 의미.

<br/><br/>
# Event Binding(이벤트 바인딩)
- 이벤트 바인딩이란, 발생하는 이벤트와 그 후에 어떤 일이 벌어질지 알려주는 함수(콜백함수)와 묶어서 연결해준다는 의미. 여기서의 콜백함수를 이벤트 핸들러라고 한다. 
- 1. HTML 이벤트 핸들러
- 2. DOM 이벤트 핸들러
- 3. Event Listener를 이용한 이벤트 핸들러

### 1. HTML 이벤트 핸들러
``` javascript
//<button onclick="clickBtn()">Click me</button>

function clickBtn() {
  alert('Button clicked!');
   console.log(this); // window
  console.log(event.currentTarget); // <button onclick="clickBtn()">Click me</button>
}
```
- 옛날 코드. 현재 이 방식은 사용되지 않는다. HTML과 Javascript가 혼용이 되는데, 이 둘은 관심사가 다르기 때문에 같이 사용하는 것을 피해야한다. 

### 2. DOM 이벤트 핸들러
```javascript
//<button id="myBtn">Click me</button>

var myBtn = document.getElementById('myBtn');

// 첫번째 바인딩된 이벤트 핸들러 => 실행되지 않는다.
myBtn.onclick = function () {
  alert('Button clicked 1');
};

// 두번째 바인딩된 이벤트 핸들러
myBtn.onclick = function () {
  alert('Button clicked 2');

  console.log(this); // <button id="myBtn">Click me!!!</button>
  console.log(event.currentTarget); // <button id="myBtn">Click me!!!</button>
  console.log(this === event.currentTarget); // true
  // this는 이벤트에 바인딩된 요소를 가리킨다. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.
  //myBtn이란 id를 가진 button 요소(myBtn)가 이벤트에 바인딩된 요소를 말한다. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.
};
```

- HTML과 Javascript가 혼용되는 문제는 해결
- 단점 
: 이벤트 핸들러에 하나의 함수만을 바인딩 가능. 
: 함수에 인수를 전달할 수 없음
: 바인딩된 이벤트 핸들러가 2개 이상인 경우, 제일 마지막에 추가된 코드의 바인딩된 이벤트 핸들러만 실행.

### 3. EventListener를 이용한 이벤트 핸들러
``` javascript
//target.addEventListener(type, listener[, useCapture]);

var el = document.getElementById("outside");
el.addEventListener("click", function(){modifyText("four")}, false);
```
- addEventListener 함수의 인수 
: type : 이벤트 타입
: listener : 이벤트 핸들러, 즉 이벤트가 발생했을 때, 실행될 콜백함수
: useCapture: true면 Capturing, false면 Bubbling(Default: false)

- Event Listener를 이용한 이벤트 핸들러의 장점
: 하나의 이벤트에 하나 이상의 핸들러를 추가
: 캡처링과 버블링 지원
: HTML요소 뿐만아니라 모든 DOM요소에 대해 동작

- Event Listener를 이용한 이벤트 핸들러 내부의 this
``` javascript
//<button id="myBtn">Click me!!!</button>

var myBtn = document.getElementById('myBtn');
myBtn.addEventListener('click', function (event) {
  console.log(this); // <button id="myBtn">Click me!!!</button>
  console.log(event.currentTarget); // <button id="myBtn">Click me!!!</button>
  console.log(this === event.currentTarget); // true
});

//addEventListener 함수에서 지정한 이벤트 핸들러 내부의 this는 Event Listener에 바인딩된 요소(currentTarget)를 가리킨다. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같다.
```
### 4. 동적으로 요소 생성시 이벤트 바인딩 이슈 - rebinding
- createTextNode() 메서드 또는 jQuery의 append(), after() 메서드와 같이 동적으로 요소를 생성할 경우에는 이벤트 바인딩이 적용되지 않습니다. -> 리바인딩 필요
<br/><br/>

# Event Bubbling Stop (이벤트 전파 막기)
1. e.preventDefault()
: 현재 이벤트의 기본 동작을 중단한다. (상위 DOM으로 이벤트 전파는 막지 않음)
2. e.stopPropagation()
: 현재 이벤트가 상위 DOM으로 전파되지 않도록 중단한다.
3. e.stopImmediatePropagation()
: 현재 이벤트가 상위뿐 아니라 현재 레벨에 걸린 다른 이벤트도 동작하지 않도록 중단한다.
4. return false
``` javascript
//DIV 영역에 클릭 이벤트 설정
$("#div_").on("click",function(event){
    $("#console").append("<br>DIV 클릭");
});

//P 영역에 클릭 이벤트 설정
$("#p_").on("click",function(event){
    $("#console").append("<br>P 클릭");
});

//A 영역에 클릭 이벤트 설정
$("#a_").on("click",function(event){
    $("#console").append("<br>A 클릭");
    
    //jQuery 이벤트의 경우,
    //return false는 event.stopPropagation()과 event.preventDefault() 를
    //모두 수행한 것과 같은 결과를 보인다.
    return false;
});
```
: jQuery를 사용할 때는 위의 두 개 모두를 수행한 것과 같고 <br/>
: jQuery를 사용하지 않을 떄는 event.preventDefault()와 같다. 


<br/><br/>

# focus event vs blur event
- 차이점은 버블링 여부. 
- focurs - 버블링O , blur - 버블링X.
<br/><br/>

# iFrame 
###  iFrame에서 부모의 함수 호출
``` jsp
<!-- parent.jsp-->

<script>
 function Msg(){
  alert("호출");
}
</script> 

<iframe name="ifrm" src="child.jsp">
```

```jsp
<!-- child.jsp-->

<script>
function sub(){
  parent.Msg(); //여기서 parent로 부모 참조
}
</script>

<input type="button" value="submit" onclick="sub();"/>

<!-- 반대로 부모에서 iFrame함수를 호출할 경우  -->
<!-- ifrm(name값).sub();-->
```
<br/><br/>

# Logical Operator 자바스크립트 논리연산자(||)
- 자바스크립트 논리 연산자는 참과 거짓을 판단해주는 게 아니라, 피연사자 중 하나를 반환해주는 연산자입니다. 
- 왼쪽부터 진행하여 가장 먼저 참이 나오는 형태를 가진 value가 나오는 경우, 그 피연산자를 반환해버리고 연산을 끝냅니다.
``` javascript
const n1 = true;
3 || 4     // 3
n1 || 8    // true
false || 4 // 4
0 || 9     // 9
```
### 논리연산자의 활용
``` javascript
function foo(num) {
  const n = num || 99; //만약 num값이 들어오지 않는다면, undefined가 됩니다.
  console.log(n);
}

foo(3); // 3
foo();  // 99
```
- 출처 : https://mynameisdabin.tistory.com/10?category=786517
<br/><br/>

# Offset() (오프셋 함수)
- 선택한 요소의 좌표를 가져오거나 특정 좌표로 이동시키는 메서드. 
<br/><br/>

# Self-invoking anonymous function (자기 호출 익명 함수)
- 익명함수인 경우 아래와 같이 사용될 순 없지만 다른 함수의 매개변수로 쓰일 때는 정상 동작
```javascript
function () {
...
}
```
- 자기호출 익명함수는 스스로 동작할 수 있음 
### 1. !(느낌표) 사용
```javascript
!function () {
...
}('Hello')
```
- 느낌표를 통해 함수는 호출되면서 바로 실행됨. 뒤의 ()를 이용해 함수에 이용될 매개변수도 전달할 수 있다.
- !를 사용하면 반환되는 return 값돠 반대값을 호출하니 때문에, 원래 위의 코드를 실행하면 return이 없으므로 false를 반환하나, !연산자 때문에 true를 반환함.
### 2. 소괄호를 사용한 자기호출 익명함수 (두 가지 방식)

```javascript
// 1번 타입
(function (msg) {
...
})('Hello')

// 2번 타입
(function (sg) {
...
}('Hello'))

//출처: https://emong.tistory.com/226 [에몽이]
```
<br/><br/>


# Serialization (직렬화)
- Ajax에서는 서버와의 비동기식 통신을 위해 form요소를 통해 입력받은 데이터를 직렬화 하여 전송.
- 직렬화란 입력받은 여러 데이터를 하나의 쿼리 문자열로 만드는 것을 의미. 이렇게 함으로써 form요소를 통해 입력받은 데이터를 한번에 서버에 보낼 수 있게 됨.
<br/><br/>

# this(자바스크립트의 this)
- this가 다르게 동작하는 네 가지 상황
- 1 메소드 안에서의 this
- 2 메소드가 아닌 독립적인 함수 안에서의 this
- 3 생성자로 인해 호출된 this
- 4 화살표 함수 안에서의 this

### 1. 메소드 안에서의 this (객체 안에서의 this)
- 객체가 가지고 있는 함수를 메소드라고 부름. 
### 전역함수의 경우
```javascript
function beaver() {
  console.log(this);
  //Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}
};
```
- 이렇게 전역함수를 선언하면 실제로는 전역 객체(window 혹은 global)안에 함수가 선언되는 것이기 때문에 전역 함수도 결국에는 메소드
- 위 함수를 호출하면 전역 객체인 Window가 출력됨. 메소드 호출할 시 메소드 안에서 this는 함수를 소유하고 있는 객체가 됨.

### 사용자 생성 객체 안에서의 함수(메소드)인 경우
```javscript
var beaver = {
  foo : function() {
    console.log(this);
    }
}
beaver.foo(); // 여기서 this 는 beaver가 된다.
```


### 2. 메소드가 아닌 독립적인 함수 안에서의 this (함수 안의 함수에서의 this)
- 다음과 같이 함수 안에 함수가 선언되는 경우
```javascript
function beaver() {
  function raccoon() {
    console.log(this);
    }
  raccoon();
}
beaver(); // Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, parent: Window, …}
```
- 특정 객체의 함수를 선언한 것이 아닌 독립적인 함수로 선언되고 이런식으로 **객체에 속해있지 않은 함수의 경우** this는 전역객체가 됨.


### 3. 생성자로 인해 호출된 this
```javascript
function Beaver(name) {
  this.name = name;
}
Beaver("foo");
console.log(window.name); // "foo"
```
- 위 함수에서 this는 전역객체가 되기 떄문에 foo가 출력됨
``` javascript
function Beaver(name) {
  this.name = name;
}

var foo = new Beaver("foo"); // 이 생성자로 만들어진 객체 this 를 반환
var bar = new Beaver("bar"); // 이 생성자로 만들어진 객체 this 를 반환

console.log(window.name); // 빈 문자열 "" 이 출력
console.log(foo.name); // "foo"
console.log(bar.name); // "bar"
```
- new로 인해서 함수를 생성자로 호출하는 경우 함수 안에서 this가 새롭게 만들어진 객체를 가리키게 됨. 
- new 연산자로 호출한 함수의 생성자는 반환할 때 자기 자신(this)를 반환. 

### 4. 화살표 함수 안에서의 this
```javascript
var foo = () => {
  console.log(this);
};

var beaver = {
  foo : foo
};

foo(); // Window
beaver.foo(); //Window
```
- 원래라면 메소드로 호출된 beacer.foo함수의 결과는 함수를 소유하고 있는 beaver가 this가 되어야 하지만, **화살표 함수**는 함수를 호출하는 영역의 this를 그대로 가져옴.

### 그 외
### this 정해주기 (call, apply, bind)
- call과 apply함수를 이용해서 직접 this를 명시적으로 정해줄 수 있음.
```javascript
function foo() {
  console.log(this.name);
}

var beaver = {
  name : "dorothy"
};

foo.call(beaver); // "dorothy"
```
- call과 apply는 동일한 결과를 가지지만 사용법에 있어 차이가 있음.

``` javascript
function profile(age, weight) {
  console.log(this.name);
  console.log("age : " + age);
  console.log("weight : " + weight);
}

var beaver = {
  name : "dorothy"
};

profile.apply(beaver, [4, "2kg"]);
//profile.call(beaver, 4, "2kg");
```
- apply는 매개변수로 전달할 값들ㅇ르 배열로 묶어서 전달, call은 각각의 값을 따로 넣어 전달.

```javascript
function profile(age, weight) {
  console.log(this.name);
  console.log("age : " + age);
  console.log("weight : " + weight);
}

var beaver = {
  name : "dorothy"
};

profile.bind(beaver)(4, "2kg");
```
- bind를 이용해서도 this를 변경할 수 있음. -> 커링(currying)에 대한 이해가 필요. 
<br/><br/>

# typeof
- typeof 연산자는 피연산자의 평가 전 자료형을 나타냄. 
- <br/><br/>


# 문자열 포함 여부
- indexOf() -> 포함하는 문자의 인덱스 반환, 없으면 -1 반환.
<br/><br/>

# CallBack - 콜백이란?
- 콜백은 다른 함수가 실행을 끝낸 뒤 실행되는 함수
- 자바스크립트에서 함수는 object임. 이 때문에 함수는 다른 함수의 인자로 쓰일 수도, 어떤 함수에 의해 리턴될 수도 있음. 이러한 함수를 고차함수라 부르고 인자로 넘겨지는 함수를 콜백함수라고 부름.
- 왜 콜백함수인가? -> 자바스크립트는 이벤트기반 언어이기 때문. 다음 명령어를 실행하기 전 이전 명령어의 응답을 기다리기보단, 다른 이벤트들을 기다리며 계속 명령을 수행한다. 
