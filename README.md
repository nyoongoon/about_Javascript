# about_Javascript
자바스크립트를 공부하며 알게 된 것을 기록하는 저장소입니다.

# null vs undefined

: undefined -> 아직 존재하지 않거나 더 이상 존재하지 않는 것을 의미. <br/>
: null -> 존재하지, 비어 있는 것을 의미.

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

# Offset() (오프셋 함수)
- 선택한 요소의 좌표를 가져오거나 특정 좌표로 이동시키는 메서드. 

# Serialization (직렬화)
- Ajax에서는 서버와의 비동기식 통신을 위해 form요소를 통해 입력받은 데이터를 직렬화 하여 전송.
- 직렬화란 입력받은 여러 데이터를 하나의 쿼리 문자열로 만드는 것을 의미. 이렇게 함으로써 form요소를 통해 입력받은 데이터를 한번에 서버에 보낼 수 있게 됨.

# 문자열 포함 여부
- indexOf() -> 포함하는 문자의 인덱스 반환, 없으면 -1 반환.


