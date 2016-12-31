---
layout: post
title: KickStarter 따라해보기 - 1
---

<pre>
본 포스팅은 <a href="https://github.com/kickstarter/android-oss">KickStarter Android</a> 를 보며 공부한 내용입니다.
</pre>

# 오픈소스
<pre>
킥스타터 앱에는 굉장히 많은 오픈소스를 참조한다.
아직도 모르는게 대부분이다. 답이없다 하..
우선 전에 공부했던 Dagger, Rx 시리즈, Wharton 오픈소스, espresso 등은 역시 있다.
이 외에도 굉장히 많은데 하나하나 보면서 공부해봐야 겠다..
</pre>

# MVVM
<pre>
나는 여태껏 안드로이드 프로젝트를 하면서 MVP를 주로 사용해왔다.
MVP와 MVVM의 가장 큰 차이점은 View와의 종속관계 차이라고 알고있다.
하지만 내가봤던 MVVM 샘플코드들은 MVP와 크게 다르지 않게 View와 종속관계를 이루는 경우가 많았다.
그래서 계속 MVP를 사용해왔는데 킥스타터의 안드로이드 앱은 MVVM 아키텍처를 따른다.
나는 이걸 공부할거기 때문에 MVVM을 따라해봤다.
</pre>

# KS MVVM 아키텍처
![_config.yml]({{ site.baseurl }}/images/ks/mvvm.png)

<pre>
나름대로(?) 코드를 보면서 생각해본 구조다.
기존의 MVVM과 크게 다르지 않다.
물론 코드를 더 보다보면 굉장히 좋은 부분이 있을 것 같다. 그건 나중에ㅎㅎ..
VIEW가 이벤트를 받고 관련된 로직은 View Model이 수행한다.
View Model은 inputs, outputs, error 3개의 인터페이스를 구현해 데이터 송수신의 역할을 한다.
</pre>

# Activity 전환부터
<pre>
처음 코드를 보면서 화면 전환 부분을 먼저 확인했다.
내가 안일하게 생각했던 부분이있었다.
이 프로젝트에서는 requestCode와 resultCode 역시 ViewModel이 관리한다.
나는 화면 전환에 있어서는 너무너무 대충 처리했기 때문에 이런건 생각도 안해봤다.
그래서 이 코드를 뜯어 확인해보고 간단한 예제를 만들어봤다.
</pre>

![_config.yml]({{ site.baseurl }}/images/ks/startActivity.png)

<pre>
1. FirstActivity에서 버튼을 클릭하면
2. SecondActivity가 시작하면서 getIntent를 ViewModel에 넘긴다.
3. intent에 있던 email 데이터를 출력한다.
4. SecondActivity 버튼을 누르면 SecondActivity가 종료되고 FirstActivity에 intent를 전달한다.
5. request, result code와 SecondActivity로부터 전달받은 intent를 ViewModel에 전달한다.
6. 전달받은 intent 데이터를 출력한다.
</pre>

<pre>
로직은 매우 간단하다. 보통 화면전환에 ViewModel이 추가됐을 뿐이다.
여기서 확인해본건 ActivityResult라는 객체다.
</pre><br/>

<script src="https://gist.github.com/fisache/09f144c4cc407ed3cdc1273ec0991c63.js"></script>

<pre>
Activity에서 ViewModel에 requestCode, resultCode, intent를 넘겨줄 때 ActivityResult 객체를 이용한다.
근데 왜 AutoParcel을 이용했을까?
구현은 편한건 맞는데 Parcelable을 사용한 이유가 뭘까
ViewModel은 View와 1:1관계가 아니기 때문일까
ActivityResult 객체를 컴포넌트 간의 넘겨줄 일이 생기는 걸까
모르겠지만 나중에 보면 또 이게 겁나 쩌는거일수도 있지
</pre>

<a href="https://github.com/fisache-android-practice/ks-mvvm-activity/tree/basicmvvm">전체 코드 링크</a>