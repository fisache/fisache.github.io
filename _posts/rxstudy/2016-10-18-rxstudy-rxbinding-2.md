---
layout: post
title: RxStudy - RxBinding 따라해보기 -2 RxView click을 테스트해보자
---

<pre>
본 포스팅은 누구나 다 아는 Jake Wharton의 <a href="https://github.com/JakeWharton/RxBinding">RxBinding</a>을 따라 코드를 구현한 내용입니다.
</pre>

<a href="https://github.com/fisache/RxStudy">https://github.com/fisache/RxStudy</a>

<pre>
오늘은 1편에서 만들었던 RxView의 클릭을 테스트해볼거다.
그런데 테스트 코드가 원래 코드보다 더 어렵다ㅎㅎ...
JakeWharton은 androidTest를 이용해서 테스트했길래 나도 역시 따라서 해볼거다.
testInstrumentationRunner을 설정하는 부분은 생략하겠다. 이게 주체가 아니니까..
</pre> <br />
<pre>
androidTest를 사용할 때 3가지의 rule을 이용해서 테스트할 수 있다.
</pre>

- ActivityTestRule : 
<pre>
single activity를 테스트할 때 사용한다. 보통 espresso를 이용해 화면 테스트할 때 사용한다.
</pre>

- ServiceTestRule :
<pre>
사용 안해봤지만 Service 테스트할 때 사용하는건 분명한듯 싶다 ㅎㅎ..
</pre>

- UiThreadRule :
<pre>
여기서 사용할 rule이다. 화면 테스트는 아니지만 application 또는 UiThread에서 테스트 코드가 돌아간다.
나는 버튼 클릭을 테스트할거기 때문에 ActivityTestRule 또는 UiThreadRule에서 테스트해야 한다.
하지만 화면은 없기 때문에 UiThreadRule를 이용하겠다.
</pre><br />
<script src="https://gist.github.com/fisache/091cd54dfd339c1968c35a9acd98fb0a.js"></script>

<pre>
일단 테스트코드를 보고 간단하게 어떻게 동작되지는 확인해보자..
RecordingObserver라는 객체를 생성하고, RxView click을 구독할 때 이 객체를 넣는다.
그리고 이 객체가 받은 이벤트가 없다는 걸 검증하는 부분이 있고..
view 클릭을 수행해보고, next가 들어왔는지 확인한다. isNull인 이유는 이벤트가 발생했을 때 null을 넘겨주기 때문이다.
그리고 다시 눌러본다. 이건 onCompleted가 실행되서 구독이 끝났나 확인하기 위해 한번 더 눌러보는거다.
그리고 구독을 취소한다.
그리고 버튼을 눌러보면 RecordingObserver 객체에 이벤트가 전달되지 않았다는걸 마지막으로 확인한다.
가독성 굳인 테스트 코드다.. 여기서 RecordingObserver는 구현되있는 코드다. 이걸 이제 뜯어보자.
</pre><br/>

<pre>
기본적으로 Observer는 구독시에 넣어주는 객체다. Action을 이용할 수도 있다.
여기서는 Observer를 넣어줬고 이를 RecordingObserver에서 구현했다.
</pre><br/>

<pre>
BlockingDeque : 멀티 Thread 구조에서 다른 thread의 방해를 받지 않기 위해 사용되는 객체다.
이 객체에 method를 수행할 때는 다른 thread에 의해서 침범받지 않는다. 이를 Blocking이라 한다.
Deque는 내가 아는 그 deque가 맞다. tail과 head 모두 접근이 가능한 자료구조다.
</pre>
<br/>
<script src="https://gist.github.com/fisache/86547d9f30190ca1f34ccdd6837b9454.js"></script>

<pre>
테스트 코드는 확실히 코드를 이해하는 많은 도움을 준다.
</pre>