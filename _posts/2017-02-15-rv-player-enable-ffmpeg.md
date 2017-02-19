---
layout:     post
title:      ProRes for RV Player
date:       2017-02-15 01:22:23
summary:    RV플레이어에서 ProRes 영상을 플레이 하는 방법 
categories: Test
tags: [github]
---

Tweak software의 __Rv Player__(이하 RV)에서는 기본적으로 -저작권 때문인지- Apple의 ProRes 코덱으로 인코딩된 영상의 플레이가 불가능하다. 아니, 불가능하다기보다는 활성화시켜놓지 않았다는 게 맞는 말 이겠다.



실제로 ProRes로 인코딩된 영상을 플레이해보면 아래와 같이 ProRes코덱이 허용되지 않았다는 에러가 발생한다.

![screenshot-rv](https://cloud.githubusercontent.com/assets/25483610/23003530/af051ade-f434-11e6-8526-57250014fd2b.png)





### ProRes 활성화

이 문제를 해결하기 위해서는 RV에서 미리 준비해둔 FFMPEG 관련 파일을 약간만 수정해주고 컴파일을 해야 한다.
아래는 Linux CentOS6을 기준으로 하는 내용이지만 macOS나 Windows에서도 마찬가지로 코드 수정과 컴파일에 각각 Xcode, Visual Studio를 사용하는 것을 제외하고는 과정은 동일하다.





먼저, RV가 설치된 디렉토리 하위에 __{RV home}/src/mio_ffmpeg/init.cpp__ 파일을 열어서 아래 prores 관련 부분을 삭제하거나 주석처리 해주고 저장한다.

```c++
static const char* disallowedCodecsArray[] = {
    "mp3",
    "ac3",
    "hevc",
    "mpeg2video",
    "svq1",
    "svq3",
//    "prores",
//    "prores_aw",
//    "prores_ks",
//    "prores_lgpl",
    0 };
```





벌써 마지막 단계이다. 같은 폴더에서 sudo 권한으로 컴파일을 실행한다.

```shell
sudo make
sudo make install
```





위 두 단계가 성공적으로 실행됐다면 __{RV home}/plugins/MovieFormats/mio_ffmpeg.so__ 파일이 새로 덮어씌워진다.
실제로 ProRes코덱은 이 mio_ffmpeg.so 파일로 활성화가 되기 때문에 다른 PC의 RV에도 마찬가지로 설정이 필요하다면 해당 파일만 같은 위치로 덮어씌우면 동일하게 사용할 수 있다.





### ProRes 플레이 확인

RV를 재실행 후 ProRes 영상을 플레이해보면 아래와 같이 정상적으로 플레이 되는 것을 확인할 수 있다.

![screenshot-1](https://cloud.githubusercontent.com/assets/25483610/23003508/8f383e5c-f434-11e6-99e9-8027f6ef37e1.png)
