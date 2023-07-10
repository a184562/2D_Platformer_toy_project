# 2D 플랫포머 게임 토이 프로젝트

---

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
---
### 플레이어 이동

```csharp
// 사용 코드
// 파일명 : PlayerMove.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMove : MonoBehaviour
{
    public float maxSpeed;
    Rigidbody2D rigid;
    SpriteRenderer spriteRenderer;
    Animator anim;

    void Awake()
    {
        rigid = GetComponent<Rigidbody2D>();
        spriteRenderer = GetComponent<SpriteRenderer>();
        anim = GetComponent<Animator>();
    }

    private void Update()
    {
        // Stop Speed
       if(Input.GetButtonUp("Horizontal"))
        {
            rigid.velocity = new Vector2(rigid.velocity.normalized.x * 0.5f, rigid.velocity.y);
        }

        // Direction Sprite
        if (Input.GetButton("Horizontal"))
        {
            spriteRenderer.flipX = Input.GetAxisRaw("Horizontal") == -1;
        }
       

        if(Mathf.Abs(rigid.velocity.x) < 0.3)
        {
            anim.SetBool("isWalking", false);
        }
        else
        {
            anim.SetBool("isWalking", true);
        }
    }

    void FixedUpdate()
    {
        // Move Speed
        float h = Input.GetAxisRaw("Horizontal");
        rigid.AddForce(Vector2.right * h, ForceMode2D.Impulse);

        // Max Speed
        if(rigid.velocity.x > maxSpeed) // Right Max Speed
        {
            rigid.velocity = new Vector2(maxSpeed, rigid.velocity.y);
        }
        else if(rigid.velocity.x < maxSpeed * (-1)) // Left Max Speed
        {
            rigid.velocity = new Vector2(maxSpeed * (-1), rigid.velocity.y);
        }
    }
}
```

- FixedUpdate : 초당 대략 50회 → 꾹 누르면 빨라짐(상한가 제한 필요)
- Public 변수를 사용하면 Unity Engine Inspector에서 수치를 직접 조정할 수 있어서 편리
- 오르막을 위해서는 마찰력을 뺄 필요가 있음
- 물리 법칙 적용 → Project창 > 우클릭 > Create > 2D > Pyshic Material 2D
    - Friction : 마찰력
    - Bounciness : 탄성

- Player > Rigidbody 2D > Linear Drag : 공기 저항, 이동 시 속도를 느리게 함
    - 공기 저항이 없으면 키를 눌렀다 떼면 멈추지 않음
    - 너무 크면 문제가 되니 1~2 권장
        - 양 옆 이동 뿐 아니라 낙하 속도에도 영향을 주기 때문에

- 단발적인 키 입력은 FixedUpdate 보다 Update가 좋음
    - Update는 1초에 60프레임 정도(컴퓨터에 따라서)
    - FixedUpdate는 1초에 50프레임 정도라서 손해 보는 경우가 생김(키가 씹히는 경우 존재)
- normalized : 벡터 크기를 1로 만든 상태(단위 벡터)
    - 우측 이동 시 해당 값은 1
    - 좌측 이동 시 해당 값은 -1
    - 요약하면 방향을 판단할 때 사용

- 방향에 따라 달라지는 애니메이션 적용하기 위한 작용
    - 오브젝트 > Sprite Renderer > Flip : 스프라이트를 뒤집는 옵션
        - 코드로 값을 주면 됨
    - Animator에 들어가 현재 디폴트 State를 우클릭 > Make Transition
        - Transition : 애니메이션 상태를 옮겨가는 통로
    - Animator에 Parameters
        - 애니메이터 매개변수 : 상태를 바꿀 때 필요한 변수
        - State 사이에 연결 된 Transition 클릭
        - Inspector 창 맨 아래 Conditions에 Patameters에 있는 것을 추가
        - Inspector 창 Has Exit Time : 애니메이션이 끝날 때까지 상태를 유지(잘 안씀)
- 2D 애니메이션 정리 : Has Exit Time 끄기, 겹구간 닫기, 매개변수 설정
- 보통 애니메이션은 상호 연결하여 왔다갔다 할 수 있도록 연결
- 이후 가동은 코드로 설정

- 작은 팁
    - C sharp에서는 Mathf라는 수학 관련 함수를 제공하는 클래스
    - 자주 사용하는 함수 : Abs(절대값)

---

## 2023년 07월 10일(월)

---

### 시청강의

- https://youtu.be/2SikOdH7xvQ

---

### 플레이어 점프 구현하기

```csharp
// 사용 코드
// 파일명 : PlayerMove.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMove : MonoBehaviour
{
    public float maxSpeed;
    public float jumpPower;
    Rigidbody2D rigid;
    SpriteRenderer spriteRenderer;
    Animator anim;

    void Awake()
    {
        rigid = GetComponent<Rigidbody2D>();
        spriteRenderer = GetComponent<SpriteRenderer>();
        anim = GetComponent<Animator>();
    }

    private void Update()
    {
        // Jump
        if (Input.GetButtonDown("Jump") && !anim.GetBool("isJump"))
        {
            rigid.AddForce(Vector2.up * jumpPower, ForceMode2D.Impulse);
            anim.SetBool("isJump", true);
        }
        
        // Stop Speed
       if(Input.GetButtonUp("Horizontal"))
        {
            rigid.velocity = new Vector2(rigid.velocity.normalized.x * 0.5f, rigid.velocity.y);
        }

        // Direction Sprite
        if (Input.GetButton("Horizontal"))
        {
            spriteRenderer.flipX = Input.GetAxisRaw("Horizontal") == -1;
        }
       

        if(Mathf.Abs(rigid.velocity.x) < 0.3)
        {
            anim.SetBool("isWalking", false);
        }
        else
        {
            anim.SetBool("isWalking", true);
        }
    }

    void FixedUpdate()
    {
        // Move Speed
        float h = Input.GetAxisRaw("Horizontal");
        rigid.AddForce(Vector2.right * h, ForceMode2D.Impulse);

        // Max Speed
        if(rigid.velocity.x > maxSpeed) // Right Max Speed
        {
            rigid.velocity = new Vector2(maxSpeed, rigid.velocity.y);
        }
        else if(rigid.velocity.x < maxSpeed * (-1)) // Left Max Speed
        {
            rigid.velocity = new Vector2(maxSpeed * (-1), rigid.velocity.y);
        }

        // Landing Platform
        if(rigid.velocity.y < 0)
        {
            Debug.DrawRay(rigid.position, Vector3.down, new Color(0, 1, 0));

            RaycastHit2D rayHit = Physics2D.Raycast(rigid.position, Vector3.down, 1, LayerMask.GetMask("Platform"));

            if (rayHit.collider != null)
            {
                if (rayHit.distance < 0.5f)
                {
                    // Debug.Log(rayHit.collider.name); -> Ray 확인용
                    anim.SetBool("isJump", false);
                }
            }
        }
     }
}
```

- 떨어지는 속도가 뛰는 속도에 비해서 느리기 때문에 Rigidbody 2D의 Gravity Scale의 속도를 조절 : 오브젝트에 적용되는 중력 비율(1은 100%) → JumpPower(코드에 public으로 만든 변수 값)와 Gravity Scale로 조절
- 혹은 Edit > Project setting > Physics 2D에서 중력값 설정 가능(기본값 9.81)

- 레이캐스트 : 오브젝트 검색을 위해 Ray를 쏘는 방식
    - Debug.DrawRay() : 에디터 상에서만 Ray를 그려주는 함수
    - RaycastHit : Ray에 닿은 오브젝트, 변수의 콜라이더로 검색 확인 가능
    - LayerMask : 물리 효과를 구분하는 정수값
    - 기존 Collider면 Player를 인식하고 그 다음에 Platform을 인식하므로 약간의 수정 필요
    - distance : Ray에 닿았을 때의 거리 → 플레이어 크기를 1로 두고 거리를 0.5로 두어서 판단
    
- 여기까지만 했을 때 무한 점프 → 조절 필요