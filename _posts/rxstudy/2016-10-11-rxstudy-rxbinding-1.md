---
layout: post
title: RxStudy - RxBinding 따라해보기 -1 RxView를 만들어보자.
---

<pre>
본 포스팅은 누구나 다 아는 Jake Wharton의 <a href="https://github.com/JakeWharton/RxBinding">RxBinding</a>을 따라 코드를 구현한 내용입니다.
</pre>

<a href="https://github.com/fisache/RxStudy">https://github.com/fisache/RxStudy</a>

# 뭘 따라해볼까

<pre>
RxBinding은 RxJava와 RxAndroid를 이용해 안드로이드의 위젯이나 View를 구독할 수 있게 해주는 오픈소스다.
버튼 클릭, 글 쓰기, 드래그 등등 굉장히 많은 것을 구독할 수 있도록 구현되있다.
오늘은 그 중 가장 기본적인 버튼 클릭을 구현해 볼 거다.    
</pre>

<script src="https://gist.github.com/fisache/305863057770e57005b0e9d1fab335db.js"></script>

<pre>
btn을 구독하다가 clicks 이벤트가 발생하면 tvText에 Rx Binding!을 쓰는 코드다.
물론 이걸 RxBinding을 받아서 하면 된다. 이걸 따라해볼거다.
*Subscription을 쓴 이유는 버튼 이벤트는 main thread에서 구독되야 한다. 다른 위젯이나 
view도 main thread에서 구독되는데 onStop에서 unsubscribe를 안해주게 되면 메모리 릭이 발생 할 수 있다고 한다.
</pre>
<br />
<pre>
위 코드를 위해서는 2개의 class를 구현해야 한다.
RxView : clicks 메소드를 통해 Observable을 반환한다.
ViewClickOnSubscribe : OnSubscriber를 implement한 클래스다.
Observable.create를 할 때 이 클래스를 인자로 넣어줘야 언제 구독자의 onNext, onCompleted, onError를
호출할지 설정할 수 있다.
</pre>

<script src="https://gist.github.com/fisache/bfe4ef4afb4794f3b97e4fd6e9232554.js"></script>

<pre>
주의 깊게 볼게 있다.
분명히 onNext, onCompleted, onError를 다 OnSubscriber에서 호출해줘야 한다고 했는데
onNext만 호출한다. onError는 버튼에러(view == null)는 애초에 checktNotNull에서 NullPointerException으로 처리한다.
onCompleted는 왜 호출하지 않을까?
onCompleted는 지금 구독자가 구독을 끝낸다는 것을 의미한다.
즉 만약 onNext를 호출하고 바로 onCompleted를 호출하면 다음에 버튼을 누르면 이벤트가 발생하지않는다.
onCompleted로 구독이 끝났기 때문에 다시 구독해야 한다.
이게 Hot과 Cold 의미인것 같은데 조금 더 공부해보고 다시 생각해보자 ㅎㅎ..
</pre>


