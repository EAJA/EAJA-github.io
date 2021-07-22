---
title:  "Algorithm 기본"
excerpt: "스택 큐 데크 해시"

categories:
  - Algorithm
tags:
  - [Algorithm]

date: 2021-07-22
last_modified_at: 2021-07-22
---

# 스택
---
Stack은 쌓다, 쌓이다, 포개지다  
편의점 담배 판매대같은 구조  
앞으로 넣고 마지막에 넣은 담배가 팔린다  
**선입후출** 구조  
  
<br/>  
<br/>  
  
## 스택 예시
### 프로그래머스 [짝지어 제거하기](https://programmers.co.kr/learn/courses/30/lessons/12973)
```js
function solution(s){
    if(s.length % 2 === 1){
        return 0;
    }
    
    let arr = new Array();
    // 스택으로 사용할 배열 생성

    for(let i = 0 ; i < s.length ; i++){
        if(arr.length === 0 || arr[arr.length-1] !== s[i]){
            arr.push(s[i]);
        // 현재값이 스택 마지막과 다르면 스택에 저장
        }else arr.pop();
        // 현재값이 스택의 마지막과 같으면 스택 마지막 방출
    }
    
    return arr.length === 0 ? 1 : 0;
    // 스택이 비었으면 1 : 스택이 안비었으면 0 리턴
}
```
  
### 브라우저의 뒤로가기 앞으로가기  
1. 새로운 페이지로 접속할때, 현재 페이지를 prev 스택에 보관
   - next 스택 초기화
2. 뒤로가기 버튼을 누를경우
   1. 현재페이지를 next 스택에 보관
   2. prev 스택 pop 값을 현재페이지에 렌더링
3. 앞으로가기 버튼을 누를경우
   1. 현재페이지를 prev 스택에 보관
   2. next 스택 pop 값을 현재페이지에 렌더링
  
<br/>  
<br/>  

# 큐
---
Queue는 대기 행렬  
편의점 음료 판매대, 톨게이트와 같은 구조  
먼저온 차가 계산하기 전까지 queue가 쌓이고 계산하면서 나가는 구조  
**선입선출** 구조  
  
<br/>  
<br/>  
  
## 큐 예시
### 프로그래머스 [다리를 지나는 트럭](https://programmers.co.kr/learn/courses/30/lessons/42583)
```js
function solution(bridge_length, weight, truck_weights) {
    var answer = 0;
    if (truck_weights.length === 0) return answer;
    let on_bridge = [];
    // 큐로 사용할 배열 생성

    let bridge_weight = 0;

    while (truck_weights.length !== 0 || on_bridge.length !== 0) {
        if (weight >= bridge_weight + truck_weights[0]) {
            on_bridge.push([truck_weights.shift(), 0]);
            // 큐에 트럭 집어넣기
            bridge_weight += on_bridge[on_bridge.length-1][0];
        }

        if (on_bridge.length !== 0) on_bridge.map(a => a[1]++);
        // 큐에 있는 트럭 시간 증가

        if (on_bridge[0][1] === bridge_length){
            bridge_weight -= on_bridge[0][0];
            on_bridge.shift();
            // 큐 안 가장 앞 트럭 시간이 다리길이와 같아지면 맨앞 트럭 방출
        }

        answer++;
        // 시간의 흐름
    }
    return answer + 1;
}
```
  
<br/>  
<br/>  
  
### 프린터

1. PC 에서 출력버튼 누름
2. 문서가 queue 로 들어감
3. 프린터는 queue 들어온 순서대로 출력

```js
let doc = ['a','e','z','g','q','r'];

let queue = new Array();

//컴퓨터
while(doc.length){
    // 문서 전체 정방향 인쇄
    queue.push(doc.pop());
    
    // 문서 전체 역방향 인쇄
    // queue.push(doc.shift());
}

//프린터
while(queue.length){
    let print = queue.shift();
    console.log(print);
}
```
  
<br/>  
<br/>  
  
# 데크
스택과 큐의 융합형태  
양쪽 끝에서 삽입과 삭제가 모두 가능  
보통 입력이나 출력 중 하나를 한 쪽 입구만 가능하게 제한하는 형태가 많이 쓰인다  
  
<br/>  
<br/>  
  
# 해시  
해시는 해시 함수로 구현한다. 해시 함수는 마치 배열처럼, 어떤 자료를 찾을 때 의 O(1)시간만을 소요한다.  
해시함수에 키 값을 넣어 주소값을 얻고, 그 주소에 위치한 버킷에 저장된 데이터를 가져오는 방식을 사용한다.  
하지만, 언제나 O(1)이 보장되는 것은 아니다.  
  
객체와 동일  
```js
const hash = {
    1 : "one",
    apple : "사과",
    list : [1, 2, 3]
}
```

  
<br/>  
<br/>  
  
### 참조
[프로그래머스](https://programmers.co.kr/)  
[리브레위키](https://librewiki.net/wiki/%EC%8B%9C%EB%A6%AC%EC%A6%88:%EC%88%98%ED%95%99%EC%9D%B8%EB%93%AF_%EA%B3%BC%ED%95%99%EC%95%84%EB%8B%8C_%EA%B3%B5%ED%95%99%EA%B0%99%EC%9D%80_%EC%BB%B4%ED%93%A8%ED%84%B0%EA%B3%BC%ED%95%99/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98_%EA%B8%B0%EC%B4%88)