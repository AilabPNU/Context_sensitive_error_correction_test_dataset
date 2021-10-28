# 영어 문맥의존 철자오류 교정 테스트 데이터셋

<br>

- 본 결과물은 Google Web 1T( https://catalog.ldc.upenn.edu/LDC2006T13 )의 3-gram 정보를 기반으로 구축된 문맥의존 철자오류 교정 시스템의 성능을 테스트하는 데이터셋입니다. 테스트 데이터셋은 단순철자오류와 문맥의존 철자오류를 섞어 교정 난이도를 맞췄습니다. 
<br><br>


## <b>철자오류</b> 

<br>

- 철자오류는 단순철자오류와 문맥의존 철자오류로 나뉘며, 단순철자오류는 문서 작성에 사용되지 않는 단어를 말하고 문맥의존 철자오류는 단어 하나만 봤을 때는 문제가 없지만 주변 문맥을 봤을 때 의미가 틀린 오류를 말합니다.
<br><br>


## <b>Google Web 1T</b>

<br>

- 약 1조 어절의 웹 문서 말뭉치 기반으로 n차수를 최대 5까지 n-gram 형태로 구축되 데이터셋으로 교열 없이 구축되었기 때문에 사용자들이 키보드 입력 환경에서 주로 범하는 오류를 풍부하게 담고있습니다.
<br><br>


## <b>주변 문맥 기반 오류어 생성</b>

<br>

- 오류 생성 중심 단어의 문맥에서 나타나는 모든 단어를 3-gram으로 검색하고 오류어 생성 기준에 맞는 단어를 선별하고 확률적으로 자주 범해지는 오류어를 최종 오류어로 결정합니다. 오류어의 등장 확률 계산은 주변 문맥과 오류어의 동시 등장확률을 계산하고 사용자들이 키보드 입력과정에서 주로 범하는 오류어를 결정합니다.

<center><img src="https://user-images.githubusercontent.com/93320598/139245522-a0e41bed-9d51-4c59-b90a-4d65f92af295.png" width="700" height="350"/></center>
<br><br>


## <b>오류어 생성 기준</b>

<br>

- 오류 기준은 편집거리, 입력과정에서 입력 오류 유형(키보드 입력 오류 범위, 반복 입력, 누락 등) 등을 고려하였습니다.

<br>


|<h6>오류 유형</h6>|<h6> 예 <br> (원본단어 /<br>오류어)</h6>|설명|
|:---:|:---:|---|
|<h6>입력<br>누락</h6>|major / maor|사용자가 알파벳 입력을 누락한 경우입니다.|
|<h6>오류<br>알파벳<br>추가 <br> 입력</h6>|this / thjis|사용자가 단어에서 입력 할 알파벳 이외의 입력을 한 경우입니다. <br>대부분 오류 알파벳의 등장 좌/우의 알파벳 주위 키보드에서 나타날 확률이 높습니다.|
|<h6>반복<br>입력</h6>|operation / opperation|사용자가 입력을 실수로 연속으로 두번 한 경우입니다.|
|<h6>반복<br>누락 <br> 입력</h6>|succeeded / succeded|사용자가 두번 입력될 알파벳을 누락하고 한번 입력한 경우입니다.|
|<h6>입력<br>오류</h6>|continue / continur|사용자가 알파벳을 잘 못 입력하는 경우입니다. <br>대부분 원본 알파벳의 주변 키보드를 잘 못 입력할 확률이 높습니다.|
|<h6>알파벳<br>입력<br>순서 <br> 오류</h6>|atlanta / altanta|연속으로 입력되는 알파벳의 순서가 사용자의 실수에 의해서 바뀌는 경우입니다.|
|<h6>복합<br>오류</h6>|payed / paid|위의 모든 유형이 복합적으로 나타나는 경우입니다. <br>단어의 알파벳의 수가 적고 복합오류가 한 단어에서 다양할수록 원본단어를 유추하기가 매우 힘듭니다.|

<br><br>

## <b>문서 형식</b>

<br>

- 한 문장에 하나의 오류어가 있다고 가정합니다. 그렇기 때문에 동일 문장에 오류어가 여러개 나타나면 분리하여 처리하고 있습니다.
- 생성 문장 이외의 주변 문장이 추가로 주어집니다. 딥러닝 시스템 같은 경우 넓은 문맥의 참조가 가능하므로 예측 정보를 더 제공합니다.
- 문서에서는 정답 단어, 오류 단어, 주변 문맥을 반복적으로 제공합니다.
- 정답 단어와 오류 단어의 위치는 "<mask>"가 됩니다.
- 오류어 생성 문서는 브라운 말뭉치( https://en.wikipedia.org/wiki/Brown_Corpus )의 일부를 사용하였으며, 참고로 다양한 분야에서 수집된 전문가에 의해서 교열된 균형말뭉치입니다.
- 문서 형식에 불편한 점이 있으면 "it_leejh@pusan.ac.kr"로 문의 주시면 감사하겠습니다.

<br>

```
정답 단어
오류 단어
앞 문장 ... <mask> ... 뒷 문장
정답 단어
오류 단어
앞 문장 ... <mask> ... 뒷 문장
```

<br><br>


## <b>문서 배포 정보</b>

<br>

|배포 파일명|설명|
|:--:|--|
|[50k_errorWord](http://pnuailab.synology.me/sharing/VNUTOGQbT)|5만개의 오류어 생성 테스트 데이터 (단순철자오류 + 문맥의존 철자오류)|
|[100k_errorWord](http://pnuailab.synology.me/sharing/YY2sweTER)|10만개의 오류어 생성 테스트 데이터 (단순철자오류 + 문맥의존 철자오류)|
|[200k_errorWord](http://pnuailab.synology.me/sharing/U80rx9icT)|20만개의 오류어 생성 테스트 데이터 (단순철자오류 + 문맥의존 철자오류)|

<br><br>

# <b>교정 시스템 성능 측정</b>

<br>

- 교정 테스트를 최근 발표된 논문과 현재 서비스되는 교정 시스템을 기반으로 측정하였으며, 테스트 시스템들은 철자오류를 중점으로 만들어진게 아니라 문법이나 다른 오류 유형 오류를 주로 교정하므로 본 성능이 교정의 모든 성능을 대표하지 않습니다.
- 브라운 코퍼스 660문장에서 생성된 오류어를 기반으로 교정 실험을 하였습니다.
- 분석 : grammarly와 MS Word의 경우 상용 시스템이기 때문에 정확도를 최대로 높여서 사용자가 체감하기에 찾은 오류에 대해서 잘 교정이 되도록 유도하였습니다. 논문을 통해서 보고된 Neuspell은 재현율을 높이기 위해서 최대한으로 많은 오류 가능성 후보를 찾았으며, 그렇기 때문에 정확도는 낮은 반면 다른 시스템에 비해서 재현율이 높은 것을 알 수 잇습니다.

<br>

|시스템 명|정확도(precision)|재현율(recall)|
|:--:|--:|--:|
|grammarly <br> ( https://www.grammarly.com/ )|86%|40%|
|MS Word|82%|27%|
|Neuspell<br>( https://github.com/neuspell/neuspell )|50%|61%|

<br><br>

# <b>참고 문헌</b>

<br>

- Google web 1T : https://catalog.ldc.upenn.edu/LDC2006T13
- 브라운 말뭉치 : https://en.wikipedia.org/wiki/Brown_Corpus
- Neuspell : https://arxiv.org/abs/2010.11085