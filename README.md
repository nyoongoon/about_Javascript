# about_Javascript
자바스크립트를 공부하며 알게 된 것을 기록하는 저장소입니다.
<br/><br/>

# AJAX : dataType vs contentType
- contentType은 보내는 데이터의 타입.
- 디폴트는 'application/x-www-form-urlencoded; charset=utf-8'
- 주로 'application/json; charset=utf-8'
- dataType은 서버에서 어떤 타입을 받을 것인지.
<br/><br/>

# AJAX vs fetch api
- AJAX란 서버와 비동기적으로 통신하기 위해 XMLHttpRequest 객체를 사용하는 것. 
``` javascript
window.XMLHttpRequest
```
- 우리가 알고 있는 위의 객체를 이용해 구현한 $.ajax()는 제이쿼리 라이브러리 함수. 
- fetch()는 Promise 객체를 반환
cf) Promise 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타냅니다.
- fetch는 자바스크립트 내장 함수. 
<br/><br/>

# Ajax & JSON
- datetype : "json" -> 서버에서 보내준 response의 타입을 명시. 서버로 데이터를 보내기만하고 받지 않는 경우 명시하면 안된다. 
<br/><br/>

# Blob(Binary Large Object)
- 파일류의 미가공 불변 데이터를 나타냄. 텍스트와 바이너리 형태로 읽을 수도 있으며, ReadableStream으로 변환한 후 그 메서드를 사용해 데이터를 처리할 수도 있음. 
- File인터페이스는 사용자 시스템의 파일을 지원하기 위해 Blob인터페이스를 상속함.

# Bubbling - 버블링이란?
- 중첩된 요소에서 이벤트가 발생할 때, HTML DOM APLI의 이벤트 전파(Event Propagation)은 두가지 방식으로 구분.  -> 버블링 vs 캡처링
- 캡처링 : window로부터 이벤트 발생 요소까지 이벤트 전파
- 버블링 : 이벤트 발생 요소부터 window까지 이벤트 전파
<br/><br/>

# Closure 클로저
- 내부함수가 외부함수의 지역변수에 접근할 수 있는 것이 클로저. 보통 함수를 리턴하는 꼴로 자주 사용 됨.
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

# Dataset (attribute)
- dataset 속성은 read-only(수정불가)
- custom data attributes에 read/write 가능하게 해줌 (data-\*)
- HTML의 data-\* 속성은 DOM의 dataset.property와 일치됨.
- javascript로 접근하게 될 경우 HTML의 -문자가 지워지고 카멜케이스로 접근가능.
```javascript
<div id="user" data-id="1234567890" data-user="johndoe" data-date-of-birth>John Doe</div>

const el = document.querySelector('#user');

// el.id === 'user'
// el.dataset.id === '1234567890'
// el.dataset.user === 'johndoe'
// el.dataset.dateOfBirth === ''

// set a data attribute
el.dataset.dateOfBirth = '1960-10-03';
// Result: el.dataset.dateOfBirth === '1960-10-03'

delete el.dataset.dateOfBirth;
// Result: el.dataset.dateOfBirth === undefined

if ('someDataAttr' in el.dataset === false) {
  el.dataset.someDataAttr = 'mydata';
  // Result: 'someDataAttr' in el.dataset === true
}
```
<br/><br/>

# Event Delegation 이벤트 위임
- 비슷한 방식으로 여러 요소를 다룰 때
- 이벤트 위임이란 동적으로 노드를 생성하고 삭제할 때 각 노드에 대해 이벤트를 추가하지 않고, 상위 노드에서 하위 노드의 이벤트를 제어하는 방식 

```javascript
table.onclick = function(event) {
  let td = event.target.closest('td'); // (1)

  if (!td) return; // (2)

  if (!table.contains(td)) return; // (3)

  highlight(td); // (4)
};
```
- 1 elem.closest(selector)메서드는 elem의 상위 요소 중 selector와 일치하면서 가장 가까운 부모 요소를 반환함. 
- event.target.closest('td');는 이벤트가 발생한 요소로부터 시작해 위로 올라가며 가장 가까운 td요소를 찾음.
- 3 중첩 테이블이 있는 경우 event.target은 현재 테이블 바깥의 td가 될수도 잇음. 이런 경우를 처리하기 위해 td가 table 안에 있는지를 확인.
<br/><br/>

# Event Loop(이벤트 루프) concurrency(동시성)
- 브라우저는 단일 쓰레드에서 이벤트 드리븐 방식으로 동작함. 
- 웹 애플리케이션은 단일 쓰레드임에도 많은 task가 동시에 처리되는 것처럼 느껴진다. 이처럼 자바스크립트의 동시성을 지원하는 것이 이벤트 루프.
- 대부분의 자바스크립트 엔진은 크게 2개의 영역으로 나뉨.
### Call Stack (호출 스택)
- 작업이 요청되면(함수가 호출되면) 스택에 쌓이게 되고 실행, 자스는 하나의 Call Stack을 사용하기 때문에 해당 task가 종료하기 전까지 어떤 task도 수행될 수 없다.
### Heap
- 동적으로 생성된 객체 인스턴스가 할당되는 영역.
- -> 자바스크립트 엔진은 call stack으로 요청된 작업을 순차적으로 실행한 뿐. 동시성을 지원하기 위해 필요한 비동기요청(이벤트 포함)처리는 자바스크립트 엔진을 구동하는 환경. 즉 브라우저, Node.js가 담

### Event Queue(Task Queue)
- 비동기 처리 함수의 콜백 함수, 비동기식 이벤트 핸들러, Timer함수(setTimeout(), setInterval()의 콜백함수가 보관되는 영역으로 이벤트 루프에 의해 특정 시점(Call Stack이 비어졌을 때 순차적으로 Call Stack으로 이동되어 실행 됨.))
### Event Loop
- Call Stack 내에서 현재 실행중인 task가 있는지 그리고 Event Queue에 task가 있는지 반복하여 확인한다. 만약 Call Stack이 비어있다면 Event Queue 내의 task가 Call Stack으로 이동하고 실행된다. 

``` javascript
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  setTimeout(function () {
    console.log('func2');
  }, 0);

  func3();
}

function func3() {
  console.log('func3');
}

func1();
```
- 함수 f1이 호출되면 함수 f1은 Call stack에 쌓인다 그리고 함수 f1은 함수 f2를 호출하므로 함수 f2가 call stack에 쌓이고 setTImeout가 호출된다. setTimeout의 콜백함수는 즉시 실행되지 않고 지정대기시간 만큼 기다리다가 "tick" 이벤트가 발생하면 태스크 큐로 이동한 후 Call Stack이 비어졌을 때 Call Stack으로 이동되어 실행된다. 

```javascript
console.log('Hi');
setTimeout(function() {
    console.log('callback');
}, 0);
console.log('Bye');
//결과 
//Hi
//Bye
//callback <- webapi가 보낸 틱을 받고 이벤트큐에 추가했다가 콜스택으로 이동하여 실행되므로 늦게 실행.
```
- -> callback 함수는 0초뒤에 실행시키겠다는 의미가 아니라, webApi의 응답을 0초 뒤에 받아 이벤트큐에 추가할 것이라는 의미 !!!


- 인라인 이벤트 핸들러 방식의 this -> window전역객체
- 이벤트 핸들러 프로퍼티 방식 -> 이벤트 핸들러 프로ㅓ티 방식에서 이벤트 핸들러는 메소드이므로 내부의 this는 이벤트에 바인딩 된 요소 == e.currentTarget
- addEventListener메소드 방식 -> addEventListener 메소드에서 지정한 이벤트 핸들러는 콜백함수이지만 이벤트 핸들러의 내부의 this는 이벤트 리스너에 바인딩된 요소(e.curentTarget)을 가리킨다. 

### 이벤트의 흐름
- 버블링 // 캡처링 -> 주의할 것은 버블링과 캡처링은 둘 중에 하나만 발생하는 것이 아니라 캡처링부터 시작하여 버블링으로 종료.(순차적 발생)

### 이벤트 객체
- 이벤트가 발생하면 event객체는 동적으로 생성되며 이벤트를 처리할 수 있는 이벤트 핸들러에 인자로 전달된다. 

### 이벤트 객체의 프로퍼티
- 1 Event.target => 실제로 이벤트를 발생시킨 요소 
cf) this는 이벤트에 바인딩된 요소를 가리킴. e.target은 실제로 이벤트를 발생시킨 요소를 가키림. 둘은 항상 일치하진 않음.
ex)
```
<html>
<body>
  <div class="container">
    <button id="btn1">Hide me 1</button>
    <button id="btn2">Hide me 2</button>
  </div>

  <script>
    const container = document.querySelector('.container');

    function hide(e) {
      // e.target은 실제로 이벤트를 발생시킨 DOM 요소를 가리킨다.
      e.target.style.visibility = 'hidden';
      // this는 이벤트에 바인딩된 DOM 요소(.container)를 가리킨다. 따라서 .container 요소를 감춘다.
      // this.style.visibility = 'hidden';
    }

    container.addEventListener('click', hide);
  </script>
</body>
</html>
```
- 2 Event.currentTarget
- 이벤트에 바인딩된 DOM 요소를 가리킴. 
- addEventListener 메소드에서 지정한 이벤트 핸들러 내부의 this는 이벤트에 바인딩된 DOM요소를 가키리며 이것은 이벤트 객체의 currentTarget 프로퍼티와 같음. 이벤트 핸들러 함수 내에서 currentTarget과 this는 언제나 일치. 
- 3 Event.type
- 4 Event.cancelable 
- 5 Event.eventPhase -> 0-3

###  Event Delegation 이벤트 위임
- 이벤트 위임은 다수ㅢ 자식 요소에 각각 이벤트 핸들러를 바인딩 하는 대신 하나의 부모요소에 이벤트 핸들러를 바인딩함. 
- **동적으로 요소가 추가되는 경우, 아직 추가되지 않은 요소는 DOM에 존재하지 않으므로 이벤트 핸들러를 바인딩할 수 없기 때문에 이러한 경우도 이벤트 위임을 사용한다.**
- -> 이는 이벤트가 이벤트 흐름에 의해 이벤트를 발생시킨 요소의 부모 요소에도 영향(버블링)을 미치기 때문ㅇ ㅔ가능한 것이다. 

### 기본 동작의 변경
- 1 Event.preventDefault()
- 요소가 가지고 있는 기본 동작을 중단
- 2 Event.stopPropagation()
- 이벤트를 처리한 후 이벤트가 부모요소로 이벤트가 전파되는 것을 중단시키기 위한 메소드. -> 부모 요소에 동일한 이벤트에 대한 다른 핸들러가 지정되어 있을 경우 사용 됨. 
<br/><br/>

# Event
## inlineEvent vs addEventListenr 
```javascript
$a = document.querySelector();

//inline event
$a.setAttibute('onclick', 'f()');
// -> 하나의 이벤트만 바인딩 가능, 여러개를 쓰면 덮어 씌워진다.

//vs 

$a.onclick = f;
// 이 코드는 인라인 이벤트 방식과 비슷함. (그래도 html코드가 아닌 script를 사용하고 있기 떄문에 익명 함수, 함수 참조, 클로저 사용이 가능하다.)

//vs
$a.addEventListener('click', f, false);
// 다양한 이벤트 등록, 개별적으로 삭제 가능하므로 더 유연함.
// 세번째인자도 이벤트 버블링 제어 가능.
// 여러개의 이벤트 타입들을 쉽게 바인딩 할 수 있다.
['mouseover', 'click'].map(function(e) {
    obj.addEventListener(e, do1);
    // 한 배열 객체에서 이벤트 발생 type들을 관리할 수도 있다.

});
```
- 출처 : https://dillionmegida.com/p/inline-events-vs-add-event-listeners/


- 결론 : add eventListener를 쓰자 !
- add eventListener 장점
1. 여러 개의 이벤트를 overwrite할 수 있다.
2. 작성 중에 bubbling, capturing을 설정할 수 있다.
3. 여러개의 이벤트 타입들을 쉽게 바인딩 할 수 있다.
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

# Event.preventDefault()
- preventDefault()는 정확하게 "태그"의 기본 동작은 막는다는 의미. 따라서 input태그에 preventDefault를 하면 input의 기능을 막게 되는 것. 
- form의 submit은 막고 input의 require를 살리려는 내 의도대로 한다면 form의 기능을 막아야 함. form 태그에 preventDefault를 사용해야한다. 
```javascript
//$submitBtn = document.querySelector('#submitBtn');
$submitForm = document.querySelector('form');
$submitForm.addEventListener('submit', sendData);
function sendData(e){
  e.preventDefault(); //-> submit은 input이 하는게 아니라, form이 한다 ! 
}
```
<br/><br/>

# Event.target
- Event interface의 target속성은 event가 전달한 객체에 대한 참조.
- event.target은 이벤트 버블링의 가장 마지막에 위치한 최하위 요소를 반환.
- 이벤트의 버블링 또는 캡처 단계에서 이벤트 핸들러를 호출하는 Event.currentTarget과 다름
- event.currentTarget의 경우 이벤트가 바인딩 된 요소를 반환
```javascript
<div onclick ="checkTarget();">
<span>text</span>
</div>
<script>
  function checkTarget(event){
    var ele = event.currentTarget;
    console.log(ele); //div반환
  }
</script>
// 사용자가 span을 클릭한 경우 
// event.target -> 클릭된 span 태그 반환
// event.currentTaget -> 이벤트가 바인딩된 div 요소를 반환.
```
<br/><br/>


# File API
```javascript
<input type = "file" id ="input">
```
- File API는 사용자에 의해 선택된 파일을 나타내는 객체인 File을 포함하는 FileList에 접근할 수 있게 해줌.
- input요소에 change 이벤트리스너를 붙여 파일의 변경사항을 다룰 수 있음

## File
## FileList
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

# iframe vs ajax
- iframe : 두개의 분리된 웹페이지를 보여줌 -> 제어권이 없는 페이지를 붙일 때 주로 사용. 

- ajax : 하나로 통합된 웹페이지를 만들어줌. -> CSS처리가능. 

- iframe은 외부의 페이지를 띄우기 쉬우나 xss공격에 취약, 외부스타일 적용이 어려운 점, 웹크롤링 문제, 접근성, 성능 저하 등의 이유로 사용을 자제하고 있다.

- ajax는 비동기 작업 가능, state/status 요청을 읽을 수도 있으며 헤더에 접근할 수도 있음. 많은 라이브러리. 

cf)<br/>
- ifream은 자동 스크롤 생성
- ajax는 div 스타일 값으로 스크롤 생성해주어야함
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

# Module
## 모듈의 특징
### 지연실행 
- 모듈 스크립트는 항상 지연 실행 됨. 
- 외부 모듈 스크립트를 다운로드 할 때, 브라우저의 HTML처리가 멈추지 않음. 브라우저는 리소스를 병렬적으로 불러옴.
- 모듈스크립트는 HTML문서가 완전히 준비될 때까지 대기 상태에 있다가 HTML문서가 완전히 만들어진 후 실행 됨.
- 스크립트의 상대적 순서가 위쪽부터 차례로 유지 실행됨.
- 이러한 특징 때문에 모듈 스크립트는 항상 완전한 HTML 페이지를 볼 수 있고 문서 내 요소에도 접근할 수 있음! 
```javascript
<script type="module">
  alert(typeof button); // 모듈 스크립트는 지연 실행되기 때문에 페이지가 모두 로드되고 난 다음에 alert 함수가 실행되므로
  // 얼럿창에 object가 정상적으로 출력됩니다. 모듈 스크립트는 아래쪽의 button 요소를 '볼 수' 있죠.
</script>

하단의 일반 스크립트와 비교해 봅시다.

<script>
  alert(typeof button); // 일반 스크립트는 페이지가 완전히 구성되기 전이라도 바로 실행됩니다.
  // 버튼 요소가 페이지에 만들어지기 전에 접근하였기 때문에 undefined가 출력되는 것을 확인할 수 있습니다.
</script>

<button id="button">Button</button>
```

### 인라인 스크립트의 비동기 처리
- 모듈 스크립트에선 async속성을 인라인 스크립트에도 적용할 수 있음.
- 다른 스크립트나 HTML이 처리되길 기다리지 않고 바로 실행됨.
- 광고나 문서레벨 이벤트 리스너, 카운터 같이 어디에도 종속되지 않는 기능을 구현할 때 유용하게 사용.
<br/><br/>


# Node 노드 다루기 
### 특정 태그 이름 지닌 노드들 찾기
```javascript
var nodes = document.getElementsByTagName('div');
for (var i = 0; i < nodes.length; i++) {
    var node = nodes.item(i);
}
```

### 특정 노드의 자식노드에서 특정 태그이름을 지닌 노드들 찾기
```javascript
var nodes = document.getElementsByTagName('div');
var node2 = nodes[2];
var node2Childs = node2.getElementsByTagName('div');
for (var i = 0; i < node2Childs.length; i++) {
    var node2Child = node2Childs.item(i);
}
```

### 문서에서 특정 클래스가 적용된 노드들 찾기
```javascript
var contentDatas = document.getElementsByClassName('content_data');
for(var i = 0; i < contentDatas.length; i++) {
    contentDatas[i].style.border = "4px solid #ff0000";
}

```

### 문서에서 특정 ID를 지닌 노드 찾기
``` javascript
var header = window.document.getElementById("header");
header.style.border ="4px solid #ff0000";
```

### 자식 노드 찾기
- 자식 노드를 모두 구할 때
```javascript
var node = window.document.getElementById("node");
var childNodes = node.childNodes;
```
- 자식 노드 중 N번째 노드에 접근하고 싶을 때
```javascript
var childNode = node.childNodes[n];
var childNode = node.childNodes.item(n);
```
- 첫 번째 자식 노드에 바로 접근
```javascript
var firstChild = node.firstChild;
```
- 마지막 자식 노드에 바로 접근
```javascript
var lastChild = node.lastChild;
```

### 부모 노드 찾기
- 특정 엘리먼트의 부모노드에 접근하고 싶을 때는 Node객체의 기본 프로퍼티인 parentNode를 사용.
```javascript
var header = document.getElementById("header");
var parentNode = header.parentNode;
```

### 형제 노드 찾기
- Node 객체의 프로퍼티인 previousSibling과 nextSibling을 이용하면 앞뒤로 인접한 형제노드에 각각 접근.
```javascript
var content = document.getElementById("content");
var psps = content.previousSibling.previousSibling;
var nsns = content.nextSibling.nextSibling;
```

### 노드 생성, 추가 방법 세가지
- Document.createElement()메서드 사용
- HTMLElement.innerHTML 프로퍼티 사용.
- Node.cloneNode() 메서드 하용. 

### 노드 삭제
- removeChild()

### 텍스트 노드 생성 및 추가
- Document.createTextNode()

### 텍스트 노드 내용 변경
- Node.nodeValue = "변경할 값";
<br/><br/>

# Offset() (오프셋 함수)
- 선택한 요소의 좌표를 가져오거나 특정 좌표로 이동시키는 메서드. 
<br/><br/>

# ReadableStream
- 바이트 데이터를 읽을 수 있는 스트림을 제공. Fetch API는 Response 객체의 body 속성을 통해서 ReadableStream의 구체적인 인스턴스를 제공. 
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

# \<script\> 태그로 도배하면 생기는 문제.
  1. 스코프 문제 -> 변수 충돌
  2. 의존성 관리 문제 -> 복잡 순서 고려
  3. 로드 시간의 문제 
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

# # Toggle 토글 
- DOMTokenList.toggle()
- The toggle() method of the DOMTokenList interface removes a given token from the list and returns false. If token doesn't exist it's added and the function returns true.
- 토글이란 스위치를 on/off하는 듯한 기능을 가짐
- toggle()뿐만 아니라, add(), remove()메소드를 통해서도 구현 가능
```javascript
변수명.addEventListener('click', function(){
    변수명.classList.toggle('클래스 명');
//toggle('className') -> 변수에 className이 없을 경우 true반환하고 className 추가 -> 있을 경우 false반환 후 제거
})
//ㄴ> classList의 toggle메소드를 통해 해당 클래스의 기능을 껐다 켰다 하는 토글로 구현할 수 있게 된다. 
```
- 어떤 조건을 만족할 때 토글 active, 아닐 때 inactive하기
```javascript
  function 함수명(){
    if(조건){
      변수명.classList.add('클래스 명');
    }else{
      변수명.classList.remove('클래스 명');
    }
  };

  변수명.addEventListener('click', 함수명);
```
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
