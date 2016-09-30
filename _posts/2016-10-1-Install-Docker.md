---
layout: post
title: 반복주기 1 - Slipp 강의를 들으며 Docker로 배포해보자!
---

<pre>
본 포스팅은 박재성 NEXT 교수님의 <a href="https://slipp.net/wiki/pages/viewpage.action?pageId=25529113">Spring-Boot, JPA로 질문/답변 게시판 구현 과정</a>을 공부한 내용입니다.
전체 개발 과정은 Mac OS에서 진행됐습니다.
</pre>

<br />
<pre>
Spring-boot를 공부하고 싶었는데 마침! JPA도 몰랐는데 마침! 딱 맞는 강의를 페이스북에서 보게 되서 공부하게됐다.
반복주기라는 개념도 정말 그럴듯해서 교수님 말씀대로 그대로 따라해 보기로 했다.
하지만 나에겐 AWS가 없었다.. Azure도 없다.. 다 무료기간이 끝나버렸다.. 배포 서버가 꼭 필요한건 아닌거 같은데 그래도 있으면했다..
처음엔 로컬에서 그냥 하려고 했다.. 근데 유저도 추가하고.. JAVA_HOME도 설정하고.. jar로 배포하고.. 뭔가 반쪽짜리 공부가 될 것 같았다..
그래서! Docker를 써보려고 한다. 물론 한번도 안써봤다. 그냥 늘 그래왔듯이 맨바닥에 헤딩하면서 해보려고한다.
</pre>
<br />

# Boot2Docker를 써봤다.
<pre>
설치법이 아니라 삽질 이야기라서 따라하시려는 분들은 여기는 넘어가면 됩니다.
</pre>
<br />
<pre>
Docker에 대해 아무것도 모르기 때문에 구글에 Docker Mac을 써봤다. 링크가 하나 나왔다.
<a href="http://www.devblog.kr/r/8y0gFPAvJ2Y9JCGYZ9qoZY8my3GyFTYJ42IJjcgOEPF">http://www.devblog.kr/r/8y0gFPAvJ2Y9JCGYZ9qoZY8my3GyFTYJ42IJjcgOEPF</a>
Docker에 관한 기본 개념과 시작하기에 아주 읽기 편한 포스팅 내용이랑 쭈우욱 읽어봤다.

Docker를 조금은 알고 있었다.. 다양한 OS를 쓸 수 있는 가상화 환경을 제공해준다..?? 정도만 알고 있었다. Virtual Box랑 비슷한가? 싶었다.
아직도 정확히는 모른다. 하지만 일단 써보자. 써보면서 이해해보자 해서 시작해봤다.
위 링크의 블로그대로 따라해보니 너무 쉽게 고래(Docker 로고)가 나왔다. 하지만 Boot2Docker는 더이상 지원하지 않는 버전이라고 한다. 
그리고 Boot2Docker는 VirtualBox를 사용한다. 뭐지? Docker 맞아? 뭔가 이상했다. 
그리고 배포를 하면 포트를 열어야 되는데 포트를 못열겠다..
이거하려고 brew를 지웠다 다시 깔았다. 안그래도 똥컴인데 Virtual Box도 돌리니까 노트북이 그만하라고 난리다.

</pre>

# 진짜 Docker를 써봤다.

<pre>
그래 뭔가 이상했다.
구글에서 다시 검색해보니 Mac용 Docker가 있었다. 현재 1.1x.x 버전인거 보니 아직 배포된지 얼마 안된것 같다.
<a href="https://docs.docker.com/docker-for-mac/">Mac용 Docker</a>
Window나 Linux용 도커도 있으니 그 언저리 주변에서 찾아보면 보일겁니다..
dmg 파일을 받고 설치해보자.

혹시, 도커에 대해 튜토리얼부터 시작하고 싶으면 아래 링크를 따라하면 됩니다.
<a href="https://docs.docker.com/engine/getstarted/">https://docs.docker.com/engine/getstarted/</a>
</pre>
![_config.yml]({{ site.baseurl }}/images/slipp-1/docker-running.png) <br />
<pre>
docker를 실행하면 위에 저런 고래모양이 뜨고 docker가 실행되면 노란불에서 초록불로 바뀐다.(시간이 조금 걸릴 수 도 있다.)
</pre>

# Docker 명령어를 써보자.
<pre>
깔긴했는데 어떻게 확인할까?
참고로 docker 명령어는 위에 그림처럼 docker가 실행된 상태여야 한다.
Docker 명령어를 몇개 쳐보자.
</pre>
- `docker --version`    : docker 버전 확인
- `docker images`       : docker에 설치된 image 목록, 설치한게 없기 때문에 아무것도 안떠야된다.
(* image라는건 OS를 압축해놓은 형태라고 생각하면 된다. image 파일을 통해 운영체제를 설치한다.)
- `docker ps -a`        : image로 만든 container 목록.

<pre>
명령어나 docker 관한 개념을 알고 싶다면
<a href="https://gist.github.com/nacyot/8366310">https://gist.github.com/nacyot/8366310</a>
를 참조하면 된다. 설명하고 싶지만 틀린 내용을 설명하는 것보단 참조하는게 괜한 오해를 안남길 수 있기 때문에..
</pre>

# Docker에 ubuntu를 올려보자.
<pre>
이제 Docker에 ubuntu를 올려서 실행시켜 볼건데 엄청 쉽다.
Virtual Box로 하던 것에 비하면 진짜 레알 겁나 쉽다.
Docker에 ubuntu를 올리는 법은 여러가지가 있다고 하는데 이게 하루만에 공부해서 뭔가 해보려니 전부 익히기는 무리였다.
첫 번째 방법은 Docker hub에 배포되있는 ubunut 이미지를 쓰는 방법이다.
</pre>
```sh
docker search ubuntu
```
<pre>
위 명령어를 쳐보면 바로 얻을 수 있는 ubuntu 이미지들이 나온다. 이를 통해 ubuntu를 설치할 수 도 있다.
처음엔 이걸로해봤다. 하지만 포트포워딩에서 문제점이 생겼다.
Docker로 설치된 OS의 포트를 노출시키는 것은 docker 이미지에 run을 하면서 해야한다.
그니깐 다 깔고 ubuntu 안에서 포트 노출을 못한다고 한다.(물론 아닐 수 도 있다. 하루만에 해본거라서..)
Docker에서 받는 ubuntu 이미지로도 ubuntu를 설치하면서 포트 해제가 가능할 것 같지만
Docker의 가장 큰장점은 DockerFile만 있으면 원하는 설정이 된 OS를 어디에서나 받을 수 있는 것이라고 한다.
예를 들면 nginx 설정이 다 된 ubuntu를 바로 설치해서 사용할 수 있다.
그래서 나도 DockerFile로 ubuntu를 설치해볼 거다.
</pre>

# DockerFile 만들기
<pre>
DockerFile을 처음부터 설정해보는 것도 도움이 되겠지만.. 그건 위험부담이 너무 크다.
그래서 docker에서 제공해주는 ubuntu DockerFile을 수정해서 쓸거다.
</pre>
```sh
git clone https://github.com/dockerfile/ubuntu.git
```
<pre>
위 명령어로 원하는 위치에 ubuntu docker를 받아보자.
받으면 파일 몇개랑 Dockerfile이 있다.
여기서 사용할 파일은 오로지 Dockerfile 뿐이다.
</pre>

[DockerFile]
<script src="https://gist.github.com/fisache/317ce0cb79a2cad5b8ceb82a79afb6f0.js"></script>
<pre>
생각보다 Dockerfile 문법이 어렵지는 않다.
FROM은 기본 OS를 설정한다. 여기서는 ubuntu:14.04를 사용한다.
RUN으로 시작되는 명령어는 OS가 설치하는 명령어들을 나타낸다.
주석에 써있는 그대로다..
한글로 된 좀 더 자세한 설명을 보고 싶으면 역시 아래 링크를..
</pre>
<a href="http://blog.nacyot.com/articles/2014-01-27-easy-deploy-with-docker/">http://blog.nacyot.com/articles/2014-01-27-easy-deploy-with-docker/</a> <br />

<pre>
이제 Dockerfile에 내용을 추가해보자.
우선 가장 중요한 포트 해제다. Spring-boot로 배포하면 Tomcat 컨테이너를 쓰기 때문에 8080 포트가 해제되야한다.
</pre>
```sh
34 # Expos port
35 EXPOSE 8080
```
<pre>
EXPOSE 명령어는 말그대로 포트를 노출시키는 명령어다. 일단은 이것만 추가해서 해도될거 같긴한데..
여기서 조금만 더 추가해보자.
사실 저 ubuntu를 실행해보면 알겠지만 sudo도 없고, vim도 없고, git은 당연히 없고, wget도 없고..
없는게 굉장히 많다. 그래서 걔네를 미리 추가해보자.(그래서 Docker 쓴다고하니까)
</pre>

[Dockerfile] 이거 복붙하면되요
<script src="https://gist.github.com/fisache/b1c9bb7338f7b10d5f294ca5d0f3b6a0.js"></script>
<pre>
MAINTAINER는 Dockerfile을 관리하는 사람이고, RUN 부분에 apt-get이 추가됐다.
물론 포트도 역시 추가됐다.
이제 설정은 끝났다. Dockerfile로 이미지를 만들어보자.
</pre>

```sh
$ cd 'DockerFile이 있는 경로'
$ docker build -t slipp . 
```
<pre>
*docker 명령어는 docker가 실행 중이여야 됩니다.*
위 명령어는 docker 이미지를 만드는데 이미지의 Repository 이름을 'slipp'으로 하고 
'.'은 현재 경로에 있는 Dockerfile을 쓰겠다는 걸 의미한다.
이 명령어는 ubuntu 이미지를 다운받고 RUN 명령을 수행하느라 시간이 꽤 걸린다.
차분히 다른거 하다 오는게 낫다고 본다.
</pre>

![_config.yml]({{ site.baseurl }}/images/slipp-1/docker-images.png) <br />
<pre>
빌드가 끝나고 docker images를 다시 쳐보면 위 사진처럼 slipp이라는 이미지가 만들어진걸 볼 수 있다.
네트워크 상태가 불안하면 빌드가 실패할 수 있으니 다시 해보길 바랍니다.
여기서 docker가 좋은 점은 빌드가 실패했어도 이전까지 받은 내용은 기억하고 있다.
docker는 dockerfile에서 설정한 RUN 명령어 마다 image를 생성해서 보관하기 때문에 가능하다.
그래서 docker images -a를 써보면 그때 받은 이미지들은 모두 확인 해 볼 수 있다.
</pre>

# Ubuntu에 들어가보자.

<pre>
이제 빌드한 이미지를 이용해 Ubuntu에 접속해보자.
</pre>
```sh
$ docker run -it -p 9999:8080 --name=test slipp
```
![_config.yml]({{ site.baseurl }}/images/slipp-1/docker-run.png) <br />

<pre>
run을 해도 터미널에 아무것도 안나올 거다. 그럴 때 엔터를 한 번 치면 root가 뜬다.
run 명령어는 이미지를 이용해 처음에 접속할 때 사용하는 명령어다. 
즉, 다음번에 접속할 때는 저 명령어 쓰면 안된다. run 명령어에서 -p는 포트를 의미한다.
Mac의 9999번 포트를 docker ubuntu의 8080포트랑 매칭시킨다는 의미다.
ubuntu에서 톰캣으로 된 프로젝트를 배포시키면 localhost:9999로 접근할 수 있다는 뜻이다.
name은 이미지를 이용해 만든 컨테이너의 이름을 의미한다.
마지막 slipp은 아까 만든 이미지 이름을 뜻한다.
</pre>

# 다시 들어가보자.
<pre>
말했다시피 run 명령어는 처음 만들 때만 쓴다.
exit명령어를 쓰면 docker에서 나올 수 있다. 나온 후, docker ps -a 명령어를 써보자.
</pre>
![_config.yml]({{ site.baseurl }}/images/slipp-1/docker-ps.png) <br />
<pre>
나는 컨테이너를 많이 만들어놨기 때문에 저렇게 나오지만 하나만 만들었다면 하나의 컨테이너만 보일 것이다.
이미 만들어진 컨테이너로 다시 들어가보자.
</pre>
```sh
$ docker start test
$ docker attach test
```
<pre>
test는 컨테이너의 이름을 뜻한다. start는 컨테이너를 실행시키고 attach는 컨테이너에 접속을 의미한다.
반대로 dettach 하고 stop 하면 컨테이너에서 나오고 종료를 할 수 있다.
(하지만 나올 때는 exit으로 나온다.)
매번 쓰기 귀찮다면 alias 이용해 명령어를 추가해보자.
</pre>

[~/.bash_profile] <br />

<script src="https://gist.github.com/fisache/89a458f4823a2fb9c9d48b6c2958aabd.js"></script>

<pre>
물론, 안쓰고 직접 명령어를 써도 괜찮습니다만..
alias로 명령어를 등록할 수 있다. 터미널에서 slipp 명령어를 쓰면 ' '안에 있는 명령을 수행한다는 의미다.
드디어!! Docker를 설치하고 우분투를 올리는 것까지 했다!!!!
끝난걸까? 아니다 이제 우분투 설정해야지 ㅎㅎ
</pre>

<pre>
그전에!! 하나만 확인해보자. 포트가 제대로 열렸을까??
</pre>

```sh
$ docker port test
```
<pre>
docker ubuntu가 아닌 MAC 터미널에서 위 명령어를 써보자. test는 컨테이너 이름이다.
이 명령어를 쓸 때는 docker ubuntu에 접속해 있어야한다. 나가게 되면 알아서 docker stop test가 되는것 같다.
그래서 한 터미널로 ubuntu에 접속해 있고 터미널을 하나 더 켜서 확인해보자.
</pre>
![_config.yml]({{ site.baseurl }}/images/slipp-1/docker-port.png) <br />
<pre>
9999 포트가 test 컨테이너의 8080과 제대로 연결된걸 볼 수 있다.
</pre>

# Docker Ubuntu를 설정해보자.

<pre>
Docker로 접속할 때 귀찮은 점이 하나 있다. 바로 무조건 root 유저로 들어가야 한다는 것이다.
AWS를 쓰게 되면 ssh user@xxx.xxx.xxx 로 하기 때문에 유저로 바로 들어갈 수 있지만 도커는 그런게 없어서 아쉽다.
(있을 수도..?)
</pre>
<pre>
이제부터는 교수님께서 하신 방법 그대로 따라하면 된다.
adduser를 통해 user를 등록하고, jdk를 깔아보자.
docker ubuntu는 다른 ubuntu와 약간 다른점이 있으니 참고해보는게 좋다.
아래부터는 다른점 위주로 설명하도록 하겠다.
</pre>

# root -> user
<pre>
Docker를 쓰면 root 계정으로만 들어가야 한다. adduser로 user를 추가는 했지만 어떻게 들어갈까?
ubuntu에서 su - 'username' 명령어를 쓰면된다. 다시 root로 돌아갈 때는 exit을 쓰면된다.
</pre>
![_config.yml]({{ site.baseurl }}/images/slipp-1/docker-su.png) <br />

# env
<pre>
강의에서는 LANG과 LANGUAGE를 설정하고 env를 통해서 확인하지만 얘네는 그걸로는 안나오더라..
echo $LANG 명령어를 쓰면 나온다.
</pre>

# 나머지는 똑같다. 배포해보자.
<pre>
이제 배포해보자. java -jar를 통해 배포하고 접속해보자.
8080포트를 9999포트랑 매칭시켰으니 9999 포트로 접속해야한다.
</pre>

`localhost:9999/form.html`
![_config.yml]({{ site.baseurl }}/images/slipp-1/docker-localhost.png) <br />

# 진짜 끝난줄 알았는데..

<pre>
처음에 접속하면 .bash_profile이 실행이 안된다.. 아마 어디서 추가해줘야 하나보다..
이건 천천히 해야겠다. 일단 source .bash_profile을 한다음 배포하자 ㅎㅎ..
</pre>


