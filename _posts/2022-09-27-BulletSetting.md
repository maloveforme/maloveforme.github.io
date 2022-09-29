---

title: "Bullet Settings"
layout: single
categories:
  - Unity
tags:
  - Unity
  - Unity2DGame
  
---

### 공격 설정
- 비행기가 쏠 총알을 구현해보자.

---

### 총알 설정

1. 에셋스토어에서 받아온 총알 이미지를 Resource 파일에 편집해 저장한다.

2. 총알도 운석과 충돌해야 하므로 Collider와 Rigidbody를 추가해준다.  이때, 총알은 +y 방향으로 움직여 중력을 거스르므로 kinematics 설정을 해주고 운석을 통과하기 위해 isTrigger 설정을 해준다. (이 부분은 이미 운석에 트리거 설정이 되어 있어서 선택)

![bullet](/assets/images/2022_bullet.png)

3. 총알의 속도를 지정해주고, 운석과 마찬가지로 시간이 지나면 사라지게 설정해준다.

```C#

void Start()
{
	RbBullet.velocity = new Vector2(0, speed);
}

void Update()
{
	TimeCurrent += Time.deltaTime;
	
	if( TimeCurrent >= TimeLimit )
	{
		Destroy(this.gameObject);
	}
}


```

4. 그리고 운석과 충돌했을 때 운석과 총알 모두 사라지게 설정한다. 이때 이름을 비교하는 것이 아닌 Tag를 비교한다.

```C#

private void OnTriggerEnter2D(Collider2D collision)
{
	if(collision.CompareTag("Enemy"))
	{
		Destroy(collision.gameObject);
		Destroy(this.gameObject);	
	}
}
```

> **collision.CompareTag()**: 충돌한 물체의 태그를 확인

---

### 플레이어와 총알 연동

1. 플레이어가 특정 버튼을 눌렀을 때 총알을 나가게 해야하므로 Player 스크립트를 연다.

2. 총알도 운석처럼 프리팹으로 바꿔준다.

![Bullet_Prefabs](/assets/images/2022_Pfbullet.png)

3. 플레이어가 특정 버튼을 눌렀을 때 총알이 나가야 하므로 플레이어 스크립트에서 GameObject를 생성해 구현해준다.

```C#
[SerializeField] GameObject PfBullet;
...

void Update()
{
	TimeCurrent += Time.deltaTime;
	
	if(Input.GetKeyDown(KeyCode.Z))
	{
		MakeBullet();
	}
}
...

void MakeBullet()
{
        Instantiate
        (PfBullet, this.gameObject.transform.position,
            Quaternion.identity);
}		

```

> **Input.GetKeyDown(KeyCode)**: 특정 키 버튼을 누르는 것을 감지
>
> **Instantiate(Object, Position, Rotation)**: Object를 위치, 회전 여부를 확인해 복제.

---

### 문제점
- 운석 스크립트에서 프리팹을 다루니까, 운석에 총알을 맞추면 오브젝트가 파괴되어버려서 더이상 운석이 생성되지 않는다.
- 이를 해결하기 위해 빈 오브젝트를 만들어야 된다는 걸 알았는데, 다음 시간에 GameManager를 구현해보겠다.

