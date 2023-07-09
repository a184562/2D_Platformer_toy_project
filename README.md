# 2D 플랫포머 게임 토이 프로젝트


## 2023년 07월 06일(목)

---

### 시청 강의

- https://youtu.be/v_Y5FH_tCpc

### 사용 에셋

- https://assetstore.unity.com/packages/2d/characters/simple-2d-platformer-assets-pack-188518

---

### Sprite

1. Sprite : 2D 오브젝트
2. Sprite Renderer : 스프라이트를 보여주는 컴포넌트 → 색상 변경 가능
3. Camera > Size : 줌인, 줌아웃 가능

### 렌더 순서

1. Camera > Orthographic : 원근법이 없는 정사영 투시(기본 및 2D)
2. Camera > Perspertive : 원근법이 있는 투시
3. 렌더 순서 바꾸기
    1. Z축 옮기기 : 카메라 위치에 따라 다르지만 디폴트 값 기준 Z가 작으면 앞
        1. 3D로 확인하며 위치 조정 가능
    2. Sprite Renderer > Order in Layer로 조정 : 클수록 앞
        1. 수치로 판단 가능
        2. 복잡해지면 Z축을 권장

### 에셋 다운시

- 2D 프로젝트는 자동으로 스프라이트 적용

### 픽셀아트 조정

1. 픽셀아트(도트) : splite의 에셋 파일의 오른쪽 Filter Mode > Point[원본], Bilinear/Trilinear[압축] → 기본적으로 도트 그래픽이면 거의 무조건 Point
2. 도트 크기 조정 : PixelPer Unit으로 조정(제공 에셋의 경우 16으로 하면 1칸에 맞춰 조정됨)
3. 위치 조정 tip : Inspector > Transform > Position에 입력으로 조정하면 코딩하기 쉬워짐
4. 정리 : Sprite의 Pixel Per Unit, Filter Mode, Compression 옵션 설정

### 물리 적용

1. Component에 Box Collider 2D 적용(2D 따로 존재)
2. Box Collider 2D > Offset으로 콜라이더 위치 조정
3. Box Collider 2D > Size로 콜라이더 위치 조정
4. Component에 Rigidbody 2D 적용(2D 따로 존재)
5. 살짝 콜라이더 사이에 틈이 생김 > Project Settings > Physics 2D > Default Contact Offset(충돌여백)으로 조정(최솟값 : 0.0001)

---

## 2023년 07월 09일(일)

---

### 시청 강의

- https://youtu.be/IkvYstCzcoc

---

### 2D 아틀라스

- Sprite Mode
    - 사용 방식 : 여러개 있으면 Multiple로 바꾸면 여러 이미지가 합쳐진 이미지를 여러개 사용 가능
    - Sprite Editor를 통해 이미지 분할하여 사용 가능
        - Automatic : 이미지를 분석해서 자동으로 잡아주는 모드
        - Grid By Cell Size : 입력한 사이즈대로 균일하게 자르는 모드(권장)
        - Grid By Cell Count : 입력한 개수만큼 균일하게 자르는 모드
    - Game > Stats > Batches : 그래픽을 그리기 위해 메모리와 CPU를 사용한 횟수
        - 같은 아틀라스의 스프라이트끼리는 Batrch가 늘어나지 않음

### 애니메이션

- 아틀라스 안에 여러가지 이미지를 통해 애니메이션을 적용시킬 때 기준
- 원하는 모션을 다중으로 선택 후 Sprite에 적용 → 애니메이션 자동 생성
- 컨트롤러 파일과 애니메이션 파일이 생성
- Animator : 애니메이션을 관리하는 컴포넌트
    - 해당 컴포넌트 안에 Avatar는 3D할 때 사용
- Window > Animation > Animation을 이용해 속도 조절
    1. Animation에서 key frame 조정
    2. Animator State에서 Speed를 조정
- Key frame : 애니메이션 값을 가진 프레임

- Idle : 보통 기본(서있는) 상태를 의미

- Window > Animation > Animator : 하나의 오브젝트에 여러 애니메이션을 넣을 때(캐릭터가 서있을 때와 걸을 때의 애니메이션을 나눌 때)
    - State : 애니메이션 상태를 관리하는 애니메이터 단위
        - State를 우클릭한 후 “Set as Layer Default State”를 클릭하면 Entry 이후 기본상태로 설정 가능