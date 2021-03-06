# 09. 데이터 조직화
## 09.3 파생 변수를 질의 함수로 바꾸기
```JS
get discountedTotal(){return this._discountedTotal}
set discount(aNumber){
  const old = this._discount;
  this._discount = aNumber;
  this.discountedTotal += old - aNumber;
}
```
->
```JS
get discountedTotal(){return this._baseTotal - this._discount}
set discount(aNumber){this._discount = aNumber;}
```

### 배경
가변 데이터는 서로 다른 두 코드를 이상한 방식으로 결합하기도 한다.  
가변데이터를 완전히 배제하기란 현실적으로 불가능할 때가 많지만, 가변 데이터의 유효 범위를 가능한 좁혀야한다고 힘주어 주장한다.  


### 절차
1. 값이 갱신되는 지점을 모두 찾는다. 필요하면 변수 쪼개기를 활용해 각 갱신 지점에서 변수를 분리한다.
2. 해당 변수의 값을 계산해주는 함수를 만든다.
3. 해당 변수가 사용되는 모든 곳에 어서션을 추가하여 함수의 계산 결과가 변수의 값과 같은지 확인한다.
4. 테스트한다.
5. 변수를 읽는 코드를 모두 함수 호출로 대체한다.
6. 테스트한다.
7. 변수를 선언하고 갱신하는 코드를 죽은코드 제거하기로 없앤다.

### 예시
```JS
//ProductionPlan 클래스
get production() {return this._production;}
applyAdjustment(anAdjustment){
  this._adjustment.push(anAdjustment);
  this._production += adnAdjustment.amount;
}
```
이 코드는 조정값 adjustment를 적용하는 과정에서 직접 관련이 없는 누적 값 production까지 갱신했다.  
이 누적값을 매번 갱신하지 않고도 계산할수 있도록 수정해보자

```JS
//ProductionPlan 클래스
get production() {
  assert(this._production === this.calculatedProduction);   //어서션을 추가하여 테스트해본다.
  return this._production;
}

get calculatedProduction(){
  return this._adjustments
    .reduce((sum, a) => sum + a.amount, 0);
}
```
어서션이 실패하지 않으면 코드를 수정하여 계산 결과를 직접 반환하도록 한다.
```JS
//ProductionPlan 클래스
get production() {
  return this.calculatedProduction;
}
```
그 후 calculatedProduction() 메서드를 인라인 한다.
```JS
//ProductionPlan 클래스
get production() {
  return this._adjustments
    .reduce((sum, a) => sum + a.amount, 0);
}
```
마지막으로 옛 변수를 참조하는 모든 코드를 제거하여 정리한다.
```JS
//ProductionPlan 클래스
get production() {
  return this._adjustments
    .reduce((sum, a) => sum + a.amount, 0);
}

applyAdjustment(anAdjustment){
  this._adjustment.push(anAdjustment);
  //this._production += adnAdjustment.amount;   //제거해준다.
}
```






























