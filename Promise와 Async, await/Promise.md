```javascript
let promise = new Promise(function(resolve, reject) {
	//executer (제작 코드, '가수')
});
```
executer
	Promise 생성 시 전달되는 함수
	Promise 생성 시 자동 실행
resove, reject(자바스크립트 자체 제공 콜백)
	executer의 결과에 상관없이 상황에 따라 인자로 넘겨준 콜백 중 하나를 반드시 호출
		resolve(value)  - 성공적으로 처리 시 결과를 나타내는 value를 넘겨주며 호출
		reject(error) - 에러 발생 시 에러 객체를 나타내는 error와 함께 호출

![[Pasted image 20230920094840.png]]


#### then
```javascript
promise.then(
	function(result), {/*결과를 다룹니다.*/}
	function(error) {/* 에러를 다룹니다.*/ }
)
```

#### finally(then(f,f)와 유사)
then(f,f)와 차이점
	finally 핸들러 인수 X 
	다음 핸들러에 결과와 에러 전달

## 체이닝
```javascript
new Promise(function(resolve, reject) {  
  setTimeout(() => resolve(1), 1000);  
}).then(function(result) {
  alert(result); // 1

  return new Promise((resolve, reject) => { // (*)
    setTimeout(() => resolve(result * 2), 1000);
  });
}).then(function(result) { // (**)
  alert(result); // 2

  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(result * 2), 1000);
  });

}).then(function(result) {

  alert(result); // 4
});
```

#### 암시적 try...catch
프로미스 executor와 프로미스 핸들러 코드 주위엔 암시적 try...catch 문 존재
executer 주위 암시적 try..catch 문 에러 -> reject로 변경


#### 에러 다시 던지기
```javascript
new Promise((resolve, reject) => {

  throw new Error("에러 발생!");
}).catch(function(error) { // (*)

  if (error instanceof URIError) {
    // 에러 처리
  } else {
    alert("처리할 수 없는 에러");
    throw error; // 에러 다시 던지기
  }

}).then(function() {

  /* 여기는 실행되지 않습니다. */
}).catch(error => { // (**)

  alert(`알 수 없는 에러가 발생함: ${error}`);
  // 반환값이 없음 => 실행이 계속됨
});
```


## API

### Promise .all
```javascript
let promise = Promise.all([...promises...]);
```

배열 안 프라미스의 결과값을 담은 배열 -> 다음 promise의 인자
```javascript
let names = ['iliakan', 'Violet-Bora-Lee', 'jeresig'];
let requests = names.map(name => fetch(`https://api.github.com/users/${name}`));

Promise.all(requests)
  .then(responses => {
    // 모든 응답이 성공적으로 이행되었습니다.

    for(let response of responses) {
      alert(`${response.url}: ${response.status}`); // 모든 url의 응답코드가 200입니다.
    }

    return responses;
  })
  // 응답 메시지가 담긴 배열을 response.json()로 매핑해, 내용을 읽습니다.
  .then(responses => Promise.all(responses.map(r => r.json())))
  // JSON 형태의 응답 메시지는 파싱 되어 배열 'users'에 저장됩니다.
  .then(users => users.forEach(user => alert(user.name)));
```

전달되는 프라미스 중 하나라도 거부 -> 에러와 함께 바로 거부

### Promise.race
```javascript
let promise = Promise.race(iterable);
Promise.race([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("에러 발생!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).then(alert); // 1
```


## 프라미스화
콜백을 받는 함수 -> 프라미스 반환 함수
