---
layout:     post
title:      Nuke를 최적화하고 효율적인 작업공정을 위한 10가지 팁
date:       2017-04-01 06:12:42
summary:    지나치기 쉽지만 알아두면 퇴근을 앞당기는 Nuke 최적화 팁
category: translate
tags: [nuke,compositing]
---

영어공부도 할 겸 앞으로 시간이 허락 될 때마다 제 분야와 관련된 유익한 글들을 번역해보려고 합니다.
처음해보는 작업이라서 오역이 많을 수 있습니다. 잘못된 내용은 지적해주시면 수정하겠습니다.

아래 내용은 2010년 6월 29일에 Scott Chambers에 의해 작성된 글로 당시 Nuke 6.0 버전을 기준으로 작성되었습니다.
번역 전 원작자의 허락을 위해 연락을 취했지만 끝내 회신을 받지 못했습니다. 혹시 원작자를 아시거나 관련된 분이시라면 코멘트를 남겨주시면 감사드리겠습니다.
아울러서 전해질지는 모르겠지만 훌륭한 글을 작성해주신 Scott님에게 감사의 말씀을 전합니다.

[원문: 10 tips to optimising Nuke and creating efficient workflows](http://www.nukepedia.com/written-tutorials/10-tips-to-optimising-nuke-and-creating-efficient-workflows)



...

### 1. B PIPE

모든 Merge단계의 메인을 B input에 연결하세요 - 이는 노드 중간에 있는 Merge를 disable하더라도 이미지가 끊기지 않고 그대로 유지되는 장점도 있습니다. 이를 위해 Shake의 Mask 'inside'/'outside'처럼 Merge노드의 'mask'와 'stencil' 옵션을 사용할 수 있습니다.

_![B PIPE](https://cloud.githubusercontent.com/assets/25483610/24147296/ea5fd972-0e7c-11e7-8b1b-aff04b5cd72c.png)

### 2. BBOX

작업하는 모든 요소의 bbox(bounding box)를 최적화해야 합니다. 만약 풀프레임 이미지라면 bbox가 (blur나 transform 등으로 인해) 전체 포맷보다 커지지 않게 주의해야 하고, 풀프레임보다 작다면 bbox가 타이트하게 이미지 영역을 감싸고 있어야 합니다.
Merge 노드를 사용할 때는 최소한의 bbox 영역을 고려하여 적절한 'set bbox to' 옵션을 선택하는 것이 중요합니다.

3D파트에서는 EXR CG pass를 렌더할 때 바운딩박스(역주: autocrop)를 함께 렌더링해줘야 하지만, 그렇지 않다면 직접 만들어줄 수도 있습니다. Curve 노드의 AutoCrop 기능을 사용하면 pixel값이 0인 zero data 영역을 기준으로 바운딩박스 데이터를 측정할 수 있습니다. 그런 다음, 해당 데이터를 복사해서 Crop 노드 영역으로 사용합니다.

exr 시퀀스를 렌더할 때, Write 노드에서 'autocrop' 옵션을 체크할 수 있습니다. 해당 옵션은 exr 전용 옵션입니다. 이는 꽤 많은 메모리를 사용하므로 렌더속도가 느릴 수 있지만, 최초 한번의 렌더링으로 bbox가 포함된 시퀀스를 사용할 수 있습니다.

_![BBOX](https://cloud.githubusercontent.com/assets/25483610/24147298/ed4c2960-0e7c-11e7-9e27-d37db0feb95a.png)

### 3. Transform 노드의 연속성

Transform 노드는 연속으로 연결해서 플레이트나 각 요소들의 무결성(퀄리티)을 유지시켜야합니다. 왜냐하면, 근본적으로 픽셀은 필터링(transform, convolves, blur 등), 즉 변화를 줄때마다 시각적 cheat인 필터 알고리즘으로 인해 근사치의 새로운 픽셀이 나타납니다. 이는 이미지 퀄리티를 저하시키고 - 일반적으로 그 정도는 미세하지만 - 이러한 문제가 쌓이다 보면 플레이트나 각각의 요소에 원치 않은 이미지 결함이 나타나기 시작합니다. 역속된 다수의 Transform 노드들은 수학적으로 한 번의 연산만을 합니다. 이로 인해 Transform 작업을 여러개의 노드로 나눠 효율적으로 컨트롤 할 수 있고 Nuke의 3d 환경은 2d transform 노드와도 연결됩니다. 한 요소를 움직이는데, 각각의 X/Y, scale, rotate의 독립적으로 제어할 수 있습니다. 이렇게 3개의 노드로 분리함으로써 transform을 조절하거나 제거 또는 빠르게 비활성화할 수 있습니다. 만약 Nuke가 3개의 Transform 노드를 한번에 처리하지 않았다면, 이미지는 각 단계마다 손실될 것 입니다.
다행히도 Nuke는 이 문제에 대해서 자유롭습니다. 단, 한가지 중요한 규칙만 따른다면 말이죠. Transform 노드 사이를 Color correct나 Merge 노드로 연결을 끊으면 안됩니다. Shake에서는 편리하게도 Transform 노드의 연속성을 시각적인 초록색 라인으로 표시해주는 피드백이 있었지만, Nuke는 그런 표시가 없습니다.(아직이요? 제발!)

_![Transform](https://cloud.githubusercontent.com/assets/25483610/24147301/f04f0768-0e7c-11e7-990c-320da097983b.png)

### 4. CARD3D

Scanline render의 3D 환경에서 Card 노드 대신 Card3D 노드를 사용하세요. 렌더링 속도가 훨씬 빠르고 Card 노드와 동일하게 작업 할 수 있습니다.

### 5. DEFOCUS 대신 BLUR

Nuke의 Defocus 노드는 꽤 빠르지만, Blur 노드가 더 빠릅니다. 'Bokeh' 효과(블루밍된 하이라이트 같은)가 필요한 경우에만 defocus를 사용하세요. 매트(마스크) 채널이나 optical 효과가 필요 없는 블러 이미지에는 사용하지마세요.

### 6. EXPOSURE = MULT

엄밀히 말하면 이 부분은 최적화라고 할 수는 없지만, exposure 노드는 옵션이 노출 단위로 표시되는 것을 제외하고는 Grade 노드의 multiply 옵션과 동일합니다. 만약 아티스트가 카메라 stop이나 빛 노출 단위로 조절하거나, 슈퍼바이저나 감독에게 stop 단위로 업/다운 수정을 받았을 때는 유용합니다. 이 노드는 어떤 특별한 기능을 하는게 아니니, 보통의 컬러 작업은 Grade 노드를 사용하는게 좋습니다.

### 7. LOWER RES / LESS FRAMES PREVIEW

낮은 해상도 / 프레임 프리뷰를 사용합니다. Shot 작업을 진행할 때 항상 모든 프레임을 Full resolution으로 볼 필요는 없습니다. 프록시 모드에서 작업을 할 수 있고, 작업중인 화면을 빠르게 프록시 모드로 렌더링할 수도 있습니다. 물론 파이널 렌더나, Full resolution이 필요할때도 있지만, 간단한 comp 에러나 solving 에러 확인을 할 때는 프록시 사이즈 작업이 더 효과적입니다. 프레임도 마찬가지죠. 초기 컴프 작업 단계에서 모든 프레임 대신 5 프레임 단위로 렌더링할 수 있습니다. 렌더링 시간은 5배 빨라지고(예를 들어 50분에서 10분으로) 대부분의 에러를 발견할 수 있습니다. 보통은 모든 프레임을 보며 작업하기 때문에 이 방식을 항상 사용할 수는 없지만, 필요한 경우에는 렌더 속도를 빠르게 해서 당신과 당신의 팀의 렌더 시간을 절약할 수 있습니다. Flipbook 뷰어는 다양한 프레임 레이트로 재생이 가능하기 때문에 5 프레임 단위로 렌더링 한 경우 원래 속도의 20%로 재생해야 할겁니다. 당연히 이상하게 보이겠지만 you will get the idea of timing. 만약 2 프레임 단위로 렌더링을 해서 2배가 빨라지더라도, 이 방식은 한번 고려해볼만한 가치가 있습니다.

### 8. PRECOMP

더 이상 수정하지 않을 부분이나 작업이 종료된 부분은 precomp 하세요. 멈춰있는 '스틸 프레임'이더라도 실제로는 각 프레임마다 꽤 많은 연산을 합니다. 그렇기 때문에 단순히 멈춰있는 프레임이나 Nuke에서 많은 추가 작업을 진행한 매트페인트는 pre render하는 것이 가장 좋습니다. 최종 렌더 전에도 precomp 할 수 있습니다. write 노드의 'render orders' 옵션을 이용하여 노드 트리에 있는 write 노드들을 먼저 렌더링 한 후 곧바로 Read노드를 통해 해당 시퀀스를 불러와 최종 렌더링 합니다. precomp 시퀀스를 아직 렌더링 하지 않은 경우, 수동으로 write 노드의 렌더 경로를 Read노드의 file path에 입력하고, Frame range 옵션을 설정해야 합니다. 이는 Nuke의 Read 노드는 항상 Frame range 기본값을 1로 설정하기 때문입니다. 처음에는 Read 노드에서 파일을 찾을 수 없다는 에러가 나타날 것입니다. 그 다음, 노드 그래프의 마지막 write 노드에 있는 order number를 가장 높은 숫자로 입력합니다. 만약 노드상에 precomp용 write노드가 2개라면 각각 1과 2의 order number 값을 입력하고 최종 메인 렌더용 write노드는 3을 입력합니다.
현재 버전의 Nuke는 write 노드 내에 'read' 체크 박스를 가지고 있기 때문에 더 이상 precomp를 위해 read와 write 노드를 따로 만들지 않아도 됩니다. 단, 렌더링 전에 'read' 옵션을 체크하면 에러가 발생하는 것은 참고로 알아두세요.

또 Nuke에는 스크립트 파일에서 선택한 노드들을 새로운 스크립트로 저장하는 'precomp' 노드가 있고, write 노드도 함께 포함되어 있습니다. 또한 precomp된 스크립트 버전 관리를 할 수 있고, 아웃풋을 메인 comp로 연결할 수도 있습니다. precomp 스크립트에서 exr 시퀀스가 새로 렌더링 됐다면, Nuke는 해당 시퀀스가 예전 버전임을 자동으로 감지합니다(Nuke는 이를 위해 exr 데이터에 내부에 있는 노드 hash 정보를 이용합니다). 자신만의 방법으로 precomp 노드를 사용할 수 있지만, 필자는 메인 스크립트에서 read/write로 사용하는 것을 선호합니다. 주로 협업(예를 들어 라이팅 아티스트 또는 다른 컴포지터)을 위해 사용합니다. 보다 자세한 내용은 Nuke user manual을 참고하세요.

_![PRECOMP](https://cloud.githubusercontent.com/assets/25483610/24147303/f346890a-0e7c-11e7-8a6f-b8d871dc1698.png)

### 9. RENDER LOCALLY

백그라운드 로컬렌더 사용합니다. 대부분의 워크스테이션은 많은 RAM와 여러개의 프로세서가 있습니다. command line 명령을 통해 백그라운드 렌더링을 하고 다른 작업은 계속 이어나갈 수 있습니다. 특히 프레임이 길지 않거나, 팜이 막히거나 느려지는 경우 등에 사용할 수 있습니다.(역주: 현재는 Nuke write노드의 렌더 창에서 Background render 옵션을 통해 켤 수 있습니다.)

### 10. VECTOR BLURS

멀티샘플 블러(Motionblur 3D나 Motionblur2D) 대신 vector blur 노드를 사용하세요. 참고로 Transform 노드는 현재 자체 속성창에서 Motionblur2D 옵션을 사용할 수 있습니다.

### Nuke의 느린 속도 및 렌더 실패에 관한 추가 정보:

Nuke가 느릴 때:

- Nuke가 멈추거나 느릴 때는 항상 터미널 창에서 해당 정보/에러/주의 메시지를 확인하세요.
- Input resolution 확인: 포맷&bbox, 컬러 해상도(예를 들어 16bit 대신 32bit exr 인지)
Tiff파일은 무조건 피하세요. 그 '메모리 돼지'는 인쇄용이지 합성 파이프라인에는 아무런 도움이 되지 않습니다. - 매트 페인터에게 한번 얘기해보세요 :)
- Channel output: 몇개의 채널이 디스크에 쓰여지고 있는지, 꼭 모든 채널이 진짜로 필요한지? Write 노드의 채널 세팅을 All로 사용해야 한다면 Remove 노드로 불필요한 채널을 조절하세요.
3D Scene의 크기를 확인하세요(있는 경우) -> geo building은 단일 쓰레드며 해당 프레임에 대해 generate가 끝날 때까지 당신은 scanline 결과물을 보지 못할겁니다.

렌더에 실패할 때:

- RGBA 채널이 Cineon 포맷으로 들어가는 경우(DPX와 CINEON파일 포맷은 공식적으로 alpha 채널을 지원하지 않습니다. - 이는 심각한 문제를 야기할 수 있습니다)
- 잘못된 Nuke 또는 plugin 버전
- Write 노드에서의 순서로 인한 충돌(Read 노드가 항상 먼저 데이터를 읽어오고 그 다음 Write노드가 렌더링됩니다.)
- precomp output에서의 alpha channel 부재(render order 옵션을 사용해서 여러개의 write노드를 사용할 때)
- Output 대상 폴더가 존재하지 않을 때
- 아직 렌더링이 완료되지 않은 파일을 읽을 때(CG 시퀀스 같은) "zib decompression error"
- Write 노드에서 proxy 옵션의 file path를 지정하지 않고 proxy 렌더링을 시도할 때
- Time 관련 노드들(FrameHold, TimeOffset 등) - 보통은 잘 작동되지만 원하지 않는 프레임을 건너뛴다거나 가끔씩 예기치 않은 에러가 발생합니다 - 이 문제는 Windows Vista 64bit와 Nuke 6.0과의 의존성에 문제가 있었습니다.
