# about_Javascript
자바스크립트를 공부하며 알게 된 것을 기록하는 저장소입니다.
<br/><br/>

# Bubbling - 버블링이란?
- 중첩된 요소에서 이벤트가 발생할 때, HTML DOM APLI의 이벤트 전파(Event Propagation)은 두가지 방식으로 구분.  -> 버블링 vs 캡처링
- 캡처링 : window로부터 이벤트 발생 요소까지 이벤트 전파
- 버블링 : 이벤트 발생 요소부터 window까지 이벤트 전파
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
- <br/><br/>

# Offset() (오프셋 함수)
- 선택한 요소의 좌표를 가져오거나 특정 좌표로 이동시키는 메서드. 
<br/><br/>

# Serialization (직렬화)
- Ajax에서는 서버와의 비동기식 통신을 위해 form요소를 통해 입력받은 데이터를 직렬화 하여 전송.
- 직렬화란 입력받은 여러 데이터를 하나의 쿼리 문자열로 만드는 것을 의미. 이렇게 함으로써 form요소를 통해 입력받은 데이터를 한번에 서버에 보낼 수 있게 됨.
<br/><br/>

# 문자열 포함 여부
- indexOf() -> 포함하는 문자의 인덱스 반환, 없으면 -1 반환.
<br/><br/>

# CallBack - 콜백이란?
- 콜백은 다른 함수가 실행을 끝낸 뒤 실행되는 함수
- 자바스크립트에서 함수는 object임. 이 때문에 함수는 다른 함수의 인자로 쓰일 수도, 어떤 함수에 의해 리턴될 수도 있음. 이러한 함수를 고차함수라 부르고 인자로 넘겨지는 함수를 콜백함수라고 부름.
- 왜 콜백함수인가? -> 자바스크립트는 이벤트기반 언어이기 때문. 다음 명령어를 실행하기 전 이전 명령어의 응답을 기다리기보단, 다른 이벤트들을 기다리며 계속 명령을 수행한다. 
