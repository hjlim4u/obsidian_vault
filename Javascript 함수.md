**함수 = (callable)객체**
```javascript
function sayHi() {
	alert("HI");
}

sayHi.name; //sayHi

let sayHi = function() {
	alert("HI");
};
sayHi.name(); //sayHi
```

### contextual name
익명함수 이름 <-컨텍스트
객체 메서드 이름 

### 프로퍼티
#### length
함수이름.length = 매개변수 개수
다른 함수 안에 동작하는 함수의 타입을 검사

#### custom
함수명.만들프로퍼티 = 초기값;
※클로저와의 차이
* 클로저: 외부 코드에서 내부 함수 변수값에 접근 불가
* 프로퍼티: 함수와 바인딩 시킨 후 외부에서 접근 가능
```javascript
function makeCounter() {
	function counter() {
		return counter.count++;
	};
	counter.count = 0;
	return coutner;
}

let counter = makeCounter();
counter(); //0
counter(); //1
counter.count=10;
```


### 기명 함수 표현식
효과
* 이름 사용하여 표현식 내부에서 자기 자신 참조 가능
* 기명함수 표현식 외부에서 해당 이름 사용 X
```javascript
let sayHi = function(who) { //function func(who) func 이름은 
	if (who) {
		alert(`Hello, ${who}`)	
	} else {
		sayHi("Guest"); //sayHi값은 언제든지 변경 될 수 있음. func("Guest")로 변경해야 안전
	}
};

let welcome = sayHi;
sayHi = null;

welcome(); // function func(who)-> Hello, Guest
```
func라는 이름은 함수 지역 수준에 존재(외부 렉시컬 환경X)
현재 함수만 참조 가능

### new Function

#### 문법
```javascript
let func = new Function ([arg1, arg2, ... argN], functionBody);
```
일반 함수와는 다르게 런타임에 받은 문자열을 사용해 함수 생성 가능

```javascript
let str = ... 동적으로 전달 받은 문자열 ....

let func = new Func(str);
func();
```



### 렉시컬 환경
실행 중인 함수, 코드 블록, 스크립트 전체는 렉시컬 환경이라 불리는 내부 숨김 연관 객체 보유

렉시컬 환경 객체 구성
1. _환경 레코드(Environment Record)_ – 모든 지역 변수를 프로퍼티로 저장하고 있는 객체입니다. `this` 값과 같은 기타 정보도 여기에 저장됩니다.
2. _외부 렉시컬 환경(Outer Lexical Environment)_ 에 대한 참조 – 외부 코드와 연관됨

![[Pasted image 20230921153342.png]]

변수
1. 스크립트가 시작되면 스크립트 내에서 선언한 변수 전체가 렉시컬 환경에 올라갑니다(pre-populated).
    - 이때 변수의 상태는 특수 내부 상태(special internal state)인 'uninitialized’가 됩니다. 자바스크립트 엔진은 uninitialized 상태의 변수를 인지하긴 하지만, `let`을 만나기 전까진 이 변수를 참조할 수 없습니다.
2. `let phrase`가 나타났네요. 아직 값을 할당하기 전이기 때문에 프로퍼티 값은 `undefined`입니다. `phrase`는 이 시점 이후부터 사용할 수 있습니다.
3. `phrase`에 값이 할당되었습니다.
4. `phrase`의 값이 변경되었습니다.

함수
**다만 함수 선언문(function declaration)으로 선언한 함수는 일반 변수와는 달리 바로 초기화된다는 점에서 차이가 있습니다.**

**코드에서 변수에 접근할 땐, 먼저 내부 렉시컬 환경을 검색 범위로 잡습니다. 내부 렉시컬 환경에서 원하는 변수를 찾지 못하면 검색 범위를 내부 렉시컬 환경이 참조하는 외부 렉시컬 환경으로 확장합니다. 이 과정은 검색 범위가 전역 렉시컬 환경으로 확장될 때까지 반복됩니다.**

함수의 숨김 프로퍼티  [[[[Environment]]]]
![[Pasted image 20231004104905.png]]
[[[[Environment]]]]에는 함수를 생성한 렉시컬 환경에 대한 참조가 저장/ 딱 한 번 세팅 후 변경 X

### 최적화
자바스크립트 엔진은 변수 사용 분석하고 외부 변수 사용하지 않는다고 판단되면 제거
**디버깅 시, 최적화 과정에서 제거된 변수를 사용할 수 없다는 점은 V8 엔진(Chrome, Opera에서 쓰임)의 주요 부작용입니다.**
```javascript
let value = "이름이 같은 다른 변수";

function f() {
  let value = "가장 가까운 변수";
  
  function g() {
    debugger; // 콘솔에 alert(value);를 입력하면 '이름이 같은 다른 변수'가 출력됩니다.
  }
  
  return g;
}

let g = f();
g();
```

