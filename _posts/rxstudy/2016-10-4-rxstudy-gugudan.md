---
layout: post
title: RxStudy - yudong님 구구단 코드 짜보기
---

<pre>
본 포스팅은 <a href="https://brunch.co.kr/@yudong/33">https://brunch.co.kr/@yudong/33</a>을 보고 공부한 내용입니다.
<a href="https://github.com/fisache/RxStudy/tree/rxstudy_yudong_gugudan">Github Repository</a>
</pre>

# 구구단 코드를 분석해보자.

<pre>
2단부터 9단까지 입력 받는다.
그 외를 입력받게 되면 에러 토스트를 띄운다.
정상적으로 입력받으면 구구단을 출력한다.
</pre>

<script src="https://gist.github.com/fisache/ce8b452bbf0e3e141d69c4e54bc922ef.js"></script>

<pre>
아~~~ 람다는 너무 어렵다. 설정도 1.8로 해줘야 한다.
에뮬레이터도 API 24 이상짜리로 켜줘야 한다.
하지만 코드를 어마하게 짧다..
</pre>
<pre>
etEdit  : 단수를 입력받는다.
btn     : 단수를 입력하고 버튼을 누른다.
etResult: 구구단을 출력한다.
</pre>
- 버튼 클릭 이벤트가 시작되면 구구단을 출력하는 Text를 초기화한다. <br/>
- Observable.just(...)을 이용해 몇단인지 읽어본다.
- map(dan ...)을 통해 읽은 내용을 Integer로 변환한다.
- filter(...)를 통해 제대로된 단수가 입려된지 확인한다.
- flatMap(...)을 이용해 Observable을 변환한다. 이게 조금 어렵네..
<pre>
dan을 입력받아 이걸 Observable.range(1,9)를 이용해 변환시키겠다.
dan과 row(1~9)를 이용해 문자열로 변환하고 이걸 리턴하겠다.
즉 결국 나오는 형태는 Observable.just(String s)가 하나씩 나온다.
여기서 말하는 하나씩은 onNext에서 하나씩 구독되는 걸 뜻한다.
그니까 Observable.from(List&#60;String&#62;) 이런 느낌?
</pre>
- map(row ...)을 통해 각각의 문자열에 '\n'를 추가해준다.
- subscribe()를 통해 구독을 시작한다.
<pre>
onNext가 되면 append로 구구단 출력 text에 붙이고
에러가 나오면 Toast로 에러 메시지를 출력한다.
에러가 되는경우는 Integer 파싱이 제대로 안됐을 경우,
filter에서 알맞은 단수가 아닐 경우다.
</pre>

<pre>
취향이 특이해서 이걸 꼭 람다가 아닌 코드로 바꿔봐야 직성이 풀린다..
그래서 바꿔보겠다.
</pre>

<script src="https://gist.github.com/fisache/ce1c665d9eabff75cdf63e873d48e14a.js"></script>

<pre>
확실히 람다를 써야겠다라는게 느껴지지만 ㅎㅎ..
</pre>