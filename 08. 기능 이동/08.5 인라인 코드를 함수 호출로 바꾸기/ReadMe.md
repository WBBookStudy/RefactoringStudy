# 08. 기능 이동
## 8.5 인라인 코드를 함수 호출로 바꾸기
```JS
let appliesToMass = false
for(const s of states) {
	if (s === "MA") appliesToMass = true
}
```
->
```JS
appliesToMass = states.includes("MA")
```

### 배경
함수는 여러 동작을 하나로 묶어주며 함수의 이름이 코드의 동작방식보다는 목적을 말해주기 때문에 코드를 이해하기 쉽게 만들어준다.  
함수는 중복을 없애는 데도 효과적이다. 똑같은 코드를 반복하는 대신 함수를 호출하면 된다.  
만약 기존에 이런 함수가 있다면 함수를 호출하는것으로 바꿔주자.  
6.1과 차이는 기존에 이런 함수가 있냐 없냐의 차이...

### 절차
1. 인라인 코드를 함수 호출로 대체한다.
2. 테스트한다.





























