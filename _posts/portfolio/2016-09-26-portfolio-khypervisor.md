---
layout: post
title: khypervisor
---

# 포트폴리오 제작 중 입니다.

# 프로젝트 설명
khypervisor는 ARM 아키텍처 board의 multi OS를 지원하기 위한 hypervisor 입니다. <br />
<a href="https://github.com/kesl-internal/khypervisor-v2">Github Repository</a>

# 개발 환경
- OS : Ubuntu 14.04
- Compiler : arm-linux-gnueabihf-
- Build System : Make
- Development Tool : vi

# 역할
- board의 BSP 작성과 빌드 시스템 리팩토링 역할을 맡았습니다.

# 사용한 기술
- DTC(device tree compiler) in linux : <br/>
board의 DTB(device tree blob)을 custom parsing하기 위해 사용했습니다.

- Kbuild in linux : <br/>
board의 configuration 설정을 위해 linux의 menuconfig 기능 구현을 목적으로 사용했습니다.

# 설명
- Device Tree Blob customizing
<pre>
DTB는 보드 별 각 디바이스의 메모리 mapping 정보를 담고 있는 바이너리 파일입니다.
DTC(device tree compiler)를 이용해 컴파일하게 되면 DTS 파일이 생성됩니다.
반대로 DTS를 DTC를 이용해 컴파일 하면 DTB가 생성됩니다.
이 DTC를 이용해 DTB 파일을 컴파일 했을 때 원하는 양식으로 DTS 파일을 만들었습니다.
</pre>
![_config.yml]({{ site.baseurl }}/images/khypervisor/dtb.png)

- Kbuild system

# 결론

