---

title:  "Meteor Settings(1)"
layout: single
categories:
  - Unity
tags:
  - Unity2DGame
  - '2022'
---
### Asset Store 이용
- Asset store에서 Import한 Asset은 Windows -> Package Manager -> My Asset에서 확인할 수 있다.

![HowtoUseAsset](/assets/images/2022_Asset.png)

---

### 운석 생성하기

- Asset store 에셋 (출처: [2D Space kit](https://assetstore.unity.com/packages/2d/environments/2d-space-kit-27662))에서 받아온 운석을 추가한다.
- 충돌을 위한 Rigidbody(Kinematics), Box Collider 2D(Istrigger) Component를 추가한다

![Meteor](/assets/images/2022_meteor.png)

---

### 운석 기능 구현하기

![MeteorSetting](/assets/images/2022_meteorsetting.png)
1. 일정한 속도로 하늘(+y방향)에서 바닥(-y방향)으로 떨어져야 한다.
2. 화면에서 랜덤한 개수가 랜덤한 위치에서 생성되어야 한다.
3. 플레이어와 부딪히면 운석과 함께 플레이어가 사라진다.
4. 바닥에 닿으면 목숨이 줄어든다.

- 운석의 기능은 다음과 같다. 오늘은 일정한 속도로 떨어지게 하는 기능(**1**)과 플레이어와 부딪히면 운석과 함께 플레이어가 사라지는(**3**)기능을 구현해보자.

---

### 운석 하강 구현

- 운석이 +y - > -y 방향으로 떨어지고, 속도가 한 번만 주어지면 계속 떨어지기 떄문에 다음과 같이 구현한다.

```csharp
public Rigidbody2D RbMeteor; // Rigidbody 2D 선언, Inspector에서 다루기 위해 public으로 선언
float speed = 6.5f; // 속도

void Start() // 한 번만 실행하면 되므로 Start()에서 구현
{
    Movement(); 
}

void Movement() // 운석의 움직임
{
    RbMeteor.velocity = new Vector2(0, -speed); // -6.5f의 속도로 떨어짐
}
```

### 파괴(충돌) 구현

- 운석과 플레이어가 서로 충돌하면 서로 파괴되어야 한다. 파괴를 구현하기 전에 충돌에 대해 알아보자.
> Collision: 물체가 서로 충돌했을 때 감지하는 것으로, 충돌했을 때 서로 밀어내는 OnCollision 충돌과 충돌했을 때 통과하는 OnTrigger 충돌이 있다.

![Collision](/assets/images/2022_collision.png)

- 운석이 물체랑 부딪혔을 때 플레이어는 파괴되고, 운석은 폭발하는 애니메이션(추후에 애니메이션으로 처리)과 함께 사라지게 구현해야하므로 Trigger충돌을 구현해보자. 파괴를 구현할 때는 직접 파괴하는 함수 Destroy()와 오브젝트를 비활성화하는 방법이 있는데, Destroy()는 오류가 많으므로 후자를 구현해보겠다. 물체와 부딪히는 순간을 비교하기 위해 OnTriggerEnter2D()함수를 사용하여,

```csharp
 private void OnTriggerEnter2D(Collider2D collision)
{
    if (collision.CompareTag("Player")) // Meteor와 충돌한 대상의 Tag가 Player면
    {
      collision.gameObject.SetActive(false); // 부딪힌 오브젝트를 비활성화
    }
}
```

- 물체가 부딪히면 함수에서 알 수 있듯이 충돌 대상에 대한 정보를 받아올 수 있다. 
- 여기서 이름을 비교하는 것이 아닌 Tag를 비교한다. 이는 추후에 운석이 여러 개 만들어졌을 때 하나하나 이름을 비교하는 것보다, 이러한 특성을 갖고 있는 물체들을 하나로 그룹화하여 좀 더 파악하기 쉽게 하기 위해 Tag를 사용한다.

> OnTriggerEnter2D(Collider2D collsion): 물체와 부딪힌 순간, 충돌한 대상의 정보도 가져온다.
> 
> Tag: 이름 그룹

- Tag를 활성화하는 방법은 Hireachy에서 오브젝트를 선택하고 Inspector 창에서 Tag를 고르면 된다. 이때 운석은 Enemy로 미리 설정해놓자.

![Tag](/assets/images/2022_Tag.png)
