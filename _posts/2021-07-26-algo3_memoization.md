---
title:  "Algorithm 메모이제이션"
excerpt: "Memoization"

categories:
  - Algorithm
tags:
  - [Algorithm]

date: 2021-07-26
last_modified_at: 2021-07-26
---

# 메모이제이션 Memoization
##### 용어
> 메모이제이션은 **-비싼 함수 호출-** 결과를 저장하고 동일한 입력이 다시 제공될 때!  
> **-캐시-** 된 결과를 반환하여 응용 프로그램의 속도를 높이는 최적화 기술
<br/>

##### 비싼 함수 호출 Expensive function calls
> 프로그램 관점에서의 두가지 주요 자원은 시간과 메모리이다
> 비싼 함수 호출은 무거운 계산 실행중에 이 두 리소스의 큰 덩어리를 소비하는 함수 호출
> 메모이제이션은 캐싱(메모리)을 사용하여 시간을 아낌
<br/>

##### 캐시 cache
> 해당 데이터에 대한 향후요청에 빠르게 처리하기 위해 데이터를 임시 보관하는 저장소
<br/>

## 만드는 예시
자바스크립트 메모이제이션 컨셉은 두가지 개념으로 만들어진다.  
- 클로저 = 외부함수안의 내포되어 있는 내부함수는 외부함수의 변수를 사용할수 있다 => **외부함수가 내부함수 범위에 폐쇄(closure) 되어있다**
- 고차함수 = 다른 함수에서 작동하는 함수를 인수로 사용하거나 반환하는 짓거리
  
<br/>
<br/>
<br/>
<br/>
  
#### 피보나치 수열
  
**무지성 재귀와 함께하는 피보나치**
```js
function fibo(n) {
    if (n <= 1) {
        return 1
    }
    return fibo(n - 1) + fibo(n - 2);
}
```
이런식으로 짜면 n이 40 정도만 되어도 속도가 박살나기 시작하는데  
<br/>
<br/>

```
fibo(5) = fibo(4)                                         + fibo(3)
        = fibo(3)                     + fibo(2)           + fibo(2)           + fibo(1)
        = fibo(2)           + fibo(1) + fibo(1) + fibo(0) + fibo(1) + fibo(0) + 1
        = fibo(1) + fibo(0) + 1 + 1 + 1 + 1 + 1 + 1
        = 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1
        = 8
```
위의 동작방식을 보면 5정도 구하는데도 답답하다  
  
n이 10일경우 호출은 177번이나 하게된다  
불쌍한 컴퓨터를 위해 메모이제이션을 적용해보면  
<br/>
<br/>

---
**메모이제이션을 적용할 경우**
```js
const cache = { 1: 1 };

function fibonacci(n) {
    if (cache[n]) {
        return cache[n];
    }
    if (n <= 1) {
        return cache[1];
    }
    return cache[n] = fibo(n - 1) + fibo(n - 2);
}
```
캐시에 없는 값을 캐시에 계속 저장하고 불러오므로  
엄청나게 빨라지고 콜스택도 안폭발한다  
<br/>
<br/>

---
#### 함수형 접근
  
<br/>
  
```js
// memoizer라고 불리는 새로운 함수를 만들어 func 함수를 받아들여 매개변수로 기억  
const memoizer = function (func) {

    // 함수 내에서 나중에 사용할 수 있도록 함수 실행 결과를 저장하기 위한 cache 오브젝트를 생성한다  
    let memo = {};

    // memoizer 함수로부터 위에서 설명한 클로저의 원리로 인해 캐시(cache)가 어디서 실행되든 
    // 캐시에 접근할 수 있는 새로운 함수를 반환
    return function (...arg) {
        
        // 반환 된 함수 내에서 if..else 문을 사용하여 argumnets(arg)에 대해 이미 캐시 된 값이 있는지 확인 
        if (memo[arg] !== undefined) {

            // 있다면 반환  
            return memo[arg];
        } else {

            // 없는 경우에는 func 으로 메모화 할(memoized) 함수를 사용하여 결과를 계산  
            // cache에다가 결과(result)를 추가하여 나중에 액세스 할 수 있도록 한다  
            return memo[arg] = func(...arg);
        }
    }
};
```
  
<br/>
<br/>
  
memoizer를 적용해보면..
```js
function fibo(n) {
    if (n <= 1) {
        return 1
    }
    return fibo(n - 1) + fibo(n - 2);
}

const fiboMemo = memoizer(fibo)
```
  
<br/>
<br/>
  
무지성 재귀 피보나치함수 fibo 를 메모이저에 넣은 fiboMemo 와  
캐시를 활용한 피보나치함수 fibonacci 의 속도를 비교해보면  
fiboMemo 가 훨씬 빠른걸 알 수 있다.
  
| name               | test          |    ops/sec |
| :----------------- | :------------ | ---------: |
| 무지성재귀 fibo    | fibo(20)      |      1,745 |
| 캐시활용 fibonacci | fibonacci(20) |    127,508 |
| 메모이저 fiboMemo  | fiboMemo(20)  | 42,982,762 |
  
[테스트 환경](https://www.devh.kr/assets/images/post/Understanding-Memoization-In-JavaScript/s_663C19FBEBF5C40174705FFED0DE5FBF14BF1F3E2DE251F46C6FC51CD989D79A_1548735781269_image.png) : chrome 69.0.3497
  
---
#### 메모이제이션이 도움이되는 네가지 경우
- 비싼 함수 호출
- 캐시된 값이 아무것도 하지 않는 것처럼 제한적이고 매우 반복적인 입력 범위를 가진 함수
- 반복 입력값이 있는 재귀함수의 경우
- 순수한 함수, 특정 입력으로 호출 될 때마다 동일한 출력을 반환하는 함수
<br/>

귀중한 자원인 **메모리**를 사용하므로 시간과 메모리중 잘 선택해서 사용해야 한다.
  
<br/>

\+  
메모이제이션은 다이나믹 프로그래밍 기법중의 하나인데  
다이나믹 프로그래밍이란 하나의 문제를 단 한번만 풀도록 하는 알고리즘이다  
  
상당수 분할정복 기법은 동일한 문제를 다시 푼다는 단점을 가지고있는데  
단순 분할 정복으로 풀게 되면 심각한 비효율성을 낳게된다  
이를 해결하기위해 동적 프로그래밍 기법중 하나인 메모이제이션을 활용하면  
이미 계산된 결과는 캐시에 저장함으로써 나중에 동일한 계산을 할 필요가 없어져서  
효율성이 엄청나게 개선되게 된다  