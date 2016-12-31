---
layout: post
title: KickStarter 따라해보기 - 2
---

본 포스팅은 <a href="https://github.com/kickstarter/android-oss">KickStarter Android</a> 를 보며 공부한 내용입니다.

# ViewModel 붙이기

저번 포스팅에서는 Activity가 ViewModel을 사용할 때 객체를 onCreate에서 직접 생성해주는 방식으로 ViewModel을 사용했다.

하지만 직접 생성하는 것보단 역시 annotation을 이용하는 방식이 가독성이나 유지보수나 여러여러여러 측면에서 좋기 때문에 킥스타터 역시 annotation을 이용했다.

나는 이전까지는 dagger를 이용해 MVP에서 presenter를 할당했다. 하지만 여기선 그 방식이 아닌 직접 annotaion을 제작해 할당해준다. 이 방식을 따라가보자.

구현되는 기능은 이전과 똑같다.

FirstActivity의 버튼을 누른다 -> SecondActivity가 실행된다 -> 

SecondActivity의 버튼을 누른다 -> SecondActivity가 종료된다.

![_config.yml]({{ site.baseurl }}/images/ks/AssignViewModel.png)

BaseActivity와 ActivityViewModel이 부모 클래스로 추가됐다.

BaseActivity는 onCreate에서 ActivityViewModelManager에 해당되는 ViewModel을 요청한다. 어떤 ViewModel을 받을지는 Activity의 annotation을 이용해 클래스로 명시해준다. ActivityViewModelManager는 전달받은 클래스를 reflect를 이용해 할당해준다.

원본 코드를 조금 쉽게 변형했다. 원래는 재사용되는 ViewModel을 따로 저장해 사용하지만 이부분은 다음에 해보자. 이거 말고도 몇개 더 있다.. ㅎㅎ..

<script src="https://gist.github.com/fisache/8c5a1593a5098c07240885dbaadbeac6.js"></script>

onCreate에서 assignViewModel을 호출한다.

assignViewModel은 annotaion을 이용해 받은 ViewModel 클래스를 이용해 ActivityViewModelManager에 해당 ViewModel을 fetch해줄 것을 요청한다.

<script src="https://gist.github.com/fisache/faddff4d6cb7780a4a5d11be6605e09c.js"></script>

BaseActivity에서 manager의 fetch를 호출할 때 전달되는 인자는 context, viewmodel 클래스다. 이를 create 함수에 전달해 알맞은 ViewModel 클래스를 반환해준다.



<a href="https://github.com/fisache-android-practice/ks-mvvm-activity/tree/16-12-31">전체 코드 링크</a>