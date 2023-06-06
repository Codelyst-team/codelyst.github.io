---
layout: post
title: "Synchronous/Asynchronous"
categories: javascript
author: kkiyya
excerpt: 동기, 비동기에 대한 기본 개념에 대해서 그림과 함께 정리해봤습니다. 
---

## 1.1 동기 vs 비동기

자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖습니다. 

⇒ 동시에 2개 이상의 함수를 동시에 실행할 수 없습니다.

<br/>

### 1.1.1 동기

현재 실행 중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식.

⇒ 태스크를 순서대로 처리한다는 장점이있지만, 앞의 태스크가 종료할 때까지 이후 태스크들이 블로킹되는 특징이 있음.


```javascript
    const currentTime = Date.now();

    while (true) {
      const now = Date.now();
      if (now - currentTime > 3000) break;
    }

    this.count++;
``` 

<br/>

### 1.1.2 비동기

현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식

주로 콜백 패턴을 사용합니다.

```javascript
    setTimeout(() => {
      this.count++;
      callback();
    }, 3000);
		console.log('뒤에 실행');
``` 

<br/>

## 1.2 실행 컨텍스트 키워드

### 1.2.1 콜 스택

소스코드 평가 과정에서 생성된 실행 컨텍스트가 추가되고 제거되는 **스택 자료구조**인 실행 컨텍스트 스택

### 1.2.2 힙

객체가 저장되는 메모리 공간, 콜 스택의 요소인 실행 컨텍스트는 힙에 저장된 객체를 참조합니다. 


### 1.2.3 태스크 큐

비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역입니다. 


### 1.2.4 이벤트 루프

콜 스택에 현재 실행 중인 실행 컨텍스트가 있는지, 태스크 큐에 대기 중인 함수가 있는지 반복해서 확인합니다. 

만약 콜 스택이 비어있고, 태스크 큐에 대기 중인 함수가 있다면 이벤트 루프는 순차적으로 태스크 큐에 대기 중인 함수를 콜 스택으로 이동시킵니다.


<br/>

## 1.3 동기 실행 컨텍스트
<img src="https://github.com/codelyst-blog/codelyst-blog.github.io/assets/55094745/41731cf5-4072-4c13-9da7-81c63efacf83" width="800"/>

<br/>

## 1.4 비동기 실행 컨텍스트
<img src="https://github.com/codelyst-blog/codelyst-blog.github.io/assets/55094745/19feea9b-3300-449b-956e-2a7cd1903f2f" width="800"/>

