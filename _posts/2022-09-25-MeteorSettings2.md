---

title:  "Meteor Settings(2)"
layout: single
categories:
  - Unity
tags:
  - Unity2DGame
  - 2022
---

### 운석 프리팹(Prefabs) 생성

**1. 맵의 랜덤한 위치에서 생성된 운석을 만들기 위해 프리팹을 활용한다.**

> Prefab: GameObject, Component, Property를  Asset의 형태로 저장하는 것

- 여러 운석들을 프리팹으로 다루지 않으면, 운석이 생성될 때마다 생성된 수만큼 오브젝트를 생성하고 컴포넌트를 부착해야하는 비효율적인 상황이 발생한다. 이를 해결하기 위한 것이 바로 프리팹이다.  이를 생성하는 법은 다음과 같다.

- Hierarchy 창에서 Object를 Asset창으로 보내면 된다.

![Prefabs](/assets/images/2022_prefab.png)

---

**2. 이제 프리팹이된 운석을 스크립트 창에서 다뤄보자.**
```C#
[SerializeField] GameObject PfMeteor;
// 프리팹화된 메테오 선언
...

void MakeMeteor() // 운석 생성
{
        Vector2 MeteorPos = new Vector2(Random.Range(0f, 1f), 1.2f);
        // 메테오의 위치를 x좌표는 0부터 1까지 랜덤한 위치에서
        // y좌표는 고정된 위치에서 생성되게 설정
        
        Vector3 Meteor_World_Pos = 
            Camera.main.ViewportToWorldPoint(MeteorPos);
        // ViewPort 사용

        Vector3 Meteor_World_Pos_Edit =
            new Vector3(Meteor_World_Pos.x, Meteor_World_Pos.y, 0);
		// Meteor의 월드 좌표를 뷰포트의 x, y 좌표로 사용
		
        Instantiate(PfMeteor, Meteor_World_Pos_Edit, Quaternion.identity);
        // 인스턴트화
}

```

> Random.Range(a,b): a와 b 사이의 랜덤한 값 생성 
> 
> Instantiate(오브젝트, 좌표, 회전성): 프리팹을 실시간으로 생성하는 함수. 

- 게임을 할 때 운석이 정해진 위치에서 나오는 것이 아닌 무작위 위치에서 생성되는 것이므로 Random.Range()를 이용해 x좌표를 설정해주었다.
- 그리고 이 위치는 화면마다 다르면 안되므로 ViewPort 좌표를 설정해주었다.
- 그리고 프리팹의 인스턴트화를 위해 Instantiate() 함수를 사용하였다.

---

**3. 운석 생성/파괴 시기 설정**
- 2번까지 마치고 게임을 실행하면 무수히 많은 운석 클론이 생성되어 메모리를 많이 잡아먹을 것이다.
- 이를 해결하기 위해 일정 시간이 지났을 때 운석 클론이 삭제되는 코드를 구현해보자.

```C#

float TimeCurrent = 0, TimeLimit = 2f;
// 현재 시간을 표현하는 TimeCurrent, 삭제할 시간을 정하는 TimeLimit 선언
...

void Update()
{
	TimeCurrent += Time.deltaTime;
	// 한 프레임마다 TimeCurrent를 Time.deltaTime만큼 더한다.

	if(TimeCurrent > TimeLimit) // Limit를 넘어섰을 때
	{
	    MakeMeteor();
	    TimeCurrent = 0; // 시간 초기화
	    Destroy(gameObject);
	    // 오브젝트 파괴
	}
}

```

> Time.deltaTime: 1/초당프레임 -> 컴퓨터 사양에 구애받지 않고 시간을 다루기 위해 사용

![deltatime](/assets/images/2022_time_deltatime.png)

