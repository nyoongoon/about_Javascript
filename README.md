# about_Javascript
자바스크립트를 공부하며 알게 된 것을 기록하는 저장소입니다.

# null vs undefined

: undefined -> 아직 존재하지 않거나 더 이상 존재하지 않는 것을 의미.
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
