---
layout:     post
title:      SHUTTER ANGLES & CREATIVE CONTROL
date:       2017-07-02 18:58:42
summary:    셔터앵글에 대하여...
category: translate
tags: [nuke,compositing]
---

흔히(저를 포함해서) Shutter Speed로 많이 알고 방식이 영상을 촬영하는 카메라와 마야, 후디니, 누크 등 여러 DCC(Digital Content Creation) 툴에서는  Shutter Angle이라는 방식으로 사용되고 있습니다. 최근 Shutter Angle에 관하여 다룰 기회가 있어 조사하던 중 개념적인 설명으로 좋은 아티클이 있어서 번역후에 공유합니다.

아래는 카메라 제조사인 RED의 공식사이트에 있는 Shutter Angle에 관한 글을 번역한 내용입니다.

[원문: SHUTTER ANGLES & CREATIVE CONTROL](http://www.red.com/learn/red-101/shutter-angle-tutorial)

...


Digital Cinematography(디지털 영화 촬영 기술)의 출현은 움직임, 즉 모션을 캡쳐하는 방법에 대한 창조적 가능성을 새롭게 열었습니다. 이 튜토리얼은 Shutter angle(개각도)의 영향과, 이를 예술적 지향점에 도달하기 위해 어떻게 창의적인 툴로써 사용하는 가에 대해 설명합니다.

### CONCEPT
Shutter angle은 Frame-rate에 상대적인 셔터 스피드를 설명하기에 유용한 방법입니다. 이는 회전식 셔터로써, 개구부 각도를 가지고 있는 디스크가 회전하면서 각 프레임을 한번씩 빛에 노출시킵니다. 개각도가 클수록 -최대 360도까지 개방하면 Frame-rate만큼- 셔터 스피드가 느려집니다. 뒤집어서 얘기하면, 개각도를 줄임으로써 셔터 스피드를 빠르게 할 수 있습니다.

![Sutter Angle](https://user-images.githubusercontent.com/25483610/27769016-ca3464b2-5f5b-11e7-9920-209b57af9b4b.png)

[3가지 Shutter Angle에 따른 노출시간]

최신 카메라가 이러한 방식으로 셔터 스피드를 조절할 필요는 없지만, 아직까지도 Shutter angle 이라는 용어 자체는 영상의 모션 블러에 대한 간단하고 보편적인 설명 방법으로 사용되고 있습니다. 프레임 안에서의 특정 피사체에 이전/다음 프레임 간 더 큰 모션 블러를 주기 위해서는 큰 개각도를 선택해야 하며, 그 반대의 경우도 마찬가지입니다:

<table width="90%" style='border:0;text-align:center;' align="center">
<tr>
<td>
<img src="https://user-images.githubusercontent.com/25483610/27769013-c2a5478e-5f5b-11e7-9c92-d9be3bea59a3.png"><br />45°
</td>
<td>
180° ![180°](https://user-images.githubusercontent.com/25483610/27769006-c25e2c8c-5f5b-11e7-8d07-eecc42592d09.png)
</td>
</tr>
</table>
270° ![270°](https://user-images.githubusercontent.com/25483610/27769005-c25e28f4-5f5b-11e7-85bd-f63874724598.png)

360° ![360°](https://user-images.githubusercontent.com/25483610/27769007-c26126c6-5f5b-11e7-883c-cf925a9d5934.png)

[연속하는 3프레임에 대한 노출 타임라인]

### APPEARANCE
보통 영화에서는 일반적으로 180°에 가까운 셔터 앵글을 사용하고, 이는 24fps 기준 1/48의 셔터 스피드에 가까운 수치입니다. 수치를 더 크게 주면 한 프레임에서의 모션블러 끝이 다음 프레임 모션블러의 시작점과 가까워지면서 움직임이 더 크고 심하게 보입니다. 반대로 작게 주면 모션블러 사이의 갭이 커지면서 프레임이 간 이미지가 불규칙하고 끊겨보이게 됩니다.

![Frozen Motion for Three Successive Frames](https://user-images.githubusercontent.com/25483610/27769008-c2648dca-5f5b-11e7-86ce-25ac31ef9076.png)

[연속하는 3프레임에 대한 고정모션]

45° ![45°](https://user-images.githubusercontent.com/25483610/27769009-c274726c-5f5b-11e7-8d67-71d5dc96fb5f.png)

180° ![180°](https://user-images.githubusercontent.com/25483610/27769011-c28a45e2-5f5b-11e7-9f7c-ba7076ad0bd7.png)

270° ![270°](https://user-images.githubusercontent.com/25483610/27769012-c28bd380-5f5b-11e7-86d1-7cc679b96d0e.png)

360° ![360°](https://user-images.githubusercontent.com/25483610/27769010-c28a5ab4-5f5b-11e7-982a-0164d486e20c.png)

[Overlaid Motion Blur vs. Shutter Angle]


위 예제 이미지는 기본적인 작동을 이해하는데 도움이 되지만, 스틸 이미지처럼 프레임 간 모션블러는 확인하기 어렵습니다.
실제로, 셔터 앵글은 -정확한 수치를 인식하지 못하더라도- 영상의 전반적인 느낌에 주관적인 영향을 미칩니다. 아래 예제를 클릭하여 차이점을 확인해보세요:

[레이스 영상]

셔터 앵글의 차이가 달리기가 시작하는 시점(움직임이 가장 먼 경우)에서는 거의 눈에 띄지 않지만, 어린이가 카메라에 가까워질수록 더 명확해지는 것을 확인해보세요. 아래의 이미지는 모션블러의 차이가 가장 두드러질 때를 극적으로 보여줍니다.

[스틸이미지]

### DISCUSSION

많은 필름 카메라가 특정 범위의 셔터 앵글만 사용 가능했지만, 디지털은 수많은 새로운 가능성(exciting new possibilities)을 제공합니다. 초점거리(Focal-Length)와 조리개(Aperture)가 스케일감과 피사계 심도(Depth of field)를 조절하는데 창의적인 도구로 사용 된 것처럼 셔터앵글은 모션블러에 대해 동일한 역할을 수행합니다.

최적의 설정은 피사체 움직임 속도 또는 촬영 감독의 의도 등 여러 요소들에 따라 결정됩니다. 예를 들어, 노출 시간을 늘리고 낮은 조명에서 이미지 노이즈를 줄이거나 더 부드럽고 유동적인 움직임을 표현하고자 할 때 더 큰 각도의 셔터앵글을 사용할 수 있습니다. 또는 작은 각도를 사용하여 각 프레임마다 빠른 동작을 선명하게 보여주는 것에 중점을 둘 수도 있습니다.

또 하나의 고려할 만한 것은 표현하고자 하는 영상의 시대(Look) 일 것입니다. 예컨대, 180°보다 작은 각도의 셔터앵글은 1950 년대의 뉴스 화면 같은 룩에 가깝게 표현될 것이고, 180°는 일반적인 영화같은 룩을 보여줍니다.
