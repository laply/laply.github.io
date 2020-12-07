---
category : 
  - javascript
author_profile : true 
toc : true
toc_sticky: true
comments: true
---
front-end 2020 load map javascript 첫 번째

dom 조작 방법 

## 0. dom 이란?

문서 객체 모델 (The Document Object model)는 
HTML. XML 문서 프로그래밍 인터페이스 이다.   
웹페이지에 대한 인터페이스 이다.

dom은 문서의 구조화 된 표현을 제공하며 프로그래밍 언어가 DOM구조에 접근할 수 있는
방법을 제공하여 그들이 문서 구조 스타일, 내용 등을 변경 할 수 있게 돕는다.

DOM 은 구조화된 nodes, property, method 를 갖고 있는 object로 문서를 표현한다.


## 1. dom과 자바스크립트

dom은 프로그래밍 언어는 아니지만 dom이 없다면, 자바스크립트 언어는 웹 페이지 또는 
xml 페이지 및 요소 들과 관련된 모델이나 개념들에 대한 정보를 갖지 못하게 된다.

문서의 모든 element - 전체 문서, 헤드. 문서안의 table, table header, table cell 안의 text - 는 문서를 위한 document object model의 한 부분이다. 때문에 이러햔 요소들을 dom과 자바스크립트 같은 스크립팅 언어를 통해 접근하고 조작할 수 있는 것이다.

API (web or XML page) = DOM + JS (Scripting language)

Dom은 프로그래밍 언어와 독립적으로 디자인 되었다. 
javascript가 아니어도 구현이 가능하다.


참고
https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/%EC%86%8C%EA%B0%9C
https://wit.nts-corp.com/2019/02/14/5522

