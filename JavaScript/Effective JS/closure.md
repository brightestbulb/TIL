# 클로저

클로저를 이해하기 위해서는 단지 세가지 기본적인 사실만 배우면 된다.   

1. **자바스크립트는 현재 함수 외부에서 선언된 변수를 참조할 수 있다.**   

```js
function makePizza(){
    var source = "tomato source";
    
    function make(topping){
        return source + " and " + topping;
    }
    
    return make("cheeze");
}

makePizza();  // tomato source and cheeze
```

make() 함수가 바깥의 source 변수를 참조할 수 있다.


2. **함수는 외부 함수가 무엇인가를 리턴한 이후에도 이 외부 함수에 선언된 변수를 참조할 수 있다.**   

```js
function makePizza(){
    var source = "tomato source";
    
    function make(topping){
        return source + " and " + topping;
    }
    
    return make;
    
}

var func = makePizza();
func("cheeze");  // tomato source and cheeze
func("ham");     // tomato source and ham
```

이 예제는 외부 함수 안에서 make('cheeze') 를 곧바로 호출하는 대신에 makePizza가 make 함수 자체를 리턴한다는 점을 빼고는 첫번째 예제와 거의 동일하다.

makePizza()가 이미 리턴되었더라도 make()는 어쨌든 source 값을 기억하고 있다.   
자바스크립트 함수는 해당 스코프에서 선언되어 참조할 수 있는 어떤 변수라도 내부적으로 보관한다.   
**함수 자신이 포함하는 스코프의 변수들을 추적하는 함수를 클로저라고 한다.**   
make()는 source와 topping 두 개의 외부 변수를 참조하는 클로저다.    
언제든 make 함수가 호출되면, 이 두 변수가 클로저에 저장되어 있기 때문에 참조할 수 있다.   
함수는 파라미터와 외부 함수의 변수뿐만 아니라 해당 스코프 내에 포함된 어떤 변수라도 참조할 수 있다.   
이를 활용하면 좀 더 보편적으로 사용할 수 있는 makePizza()를 만들 수 있다.

```js
function makePizza(source){
    function make(topping){
        return source + " and " + topping;
    }
    return make;
}
```

```js
var tomatoPizza = makePizza("tomato");
tomatoPizza("cheeze");  // tomato source and cheeze

var tomatoPizza = makePizza("garlic");
tomatoPizza("cheeze");  //  garlic and cheeze
```
 
 
자바스크립트는 클로저를 생성하기 위한 더 편리하고 일반적인 문법을 제공하는데, 함수 표현식이 바로 그것이다.   
```js
function makePizza(source){
    return function(topping){
        return source + " source and " + topping;
    }
    
}
```

함수 표현식이 익명인 사실에 주목하라. 이 함수는 새로운 함수 값을 만들기 위해 평가하는 용도로만 만들어졌다.


3. **클로저는 외부 변수의 값을 변경할 수 있다.**   
클로저는 실제로 외부 변수의 값을 변경할 수 있다. 클로저는 실제로 외부 변수의 값을 복사하지 않고 참조를 저장한다.    
따라서 그들에게 접근하는 어떤 클로저도 변경사항을 볼 수 있다. 다음 예제를 보자.   
box 객체는 내부의 값을 가지며, 그 값을 읽고 변경할 수 있는 객체다.   

```js
function box(){
    var val = "";
    return {
        set : function(newVal) { val = newVal; },
        get : function() { return val; },
        type : function() { return typeof val; }
    };
}

var b = box();
b.tpye();   // ""
b.set(100);
b.get();    // 100
b.type();   // "number"
```

이 예제는 세 개의 클로저(set, get, type) 프로퍼티들을 포함하는 객체를 생성한다.   
각 클로저는 val 변수를 공유하여 접근한다.    

