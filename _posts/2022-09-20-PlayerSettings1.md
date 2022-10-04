---
title: "Player Settings(1)"
layout: single
categories:
  - Unity
tags:
  - Unity2DGame
  - 2022
---

### 해상도 설정하기
  - 해상도 720 * 1280 으로 설정

  ![Display](/assets/images/2022_Display.png)

---

### 플레이어 생성

  1. Asset store에서 비행기 찾아서 Resource 폴더에 임포트
  2. 비행기 크기를 화면에 맞게 조절
  3. Sprite Editor를 통해 적당한 크기로 자르고, pixer per unit으로 크기 조정

  ![Display(1)](/assets/images/2022_Display(1).png)

  출처: https://assetstore.unity.com/packages/2d/characters/pixel-art-space-ship-part-1-228832

---

### 플레이어 이동
  1. Scripts 폴더에 Player 스크립트 생성
  2. Player는 중력의 영향을 받지 않는 Kinematics 상태로 설정하고
    움직임을 위해 Rigidbody2D, 충돌과 관련된 Collision 컴포넌트 추가

  ![Player_Component](/assets/images/2022_Player_Component.png)

---

### Player의 움직임을 상하좌우로 움직이게 하기 위해 다음과 같은 문법을 차용

  ```C#    
  public Rigidbody2D AirCraft;

  void Update()
  {
      PlayerMovement();
  }

  void PlayerMovement()
  {
      float Speed = 6.5f;
      // 상하좌우 속도를 6.5로 설정
      float VerticalKey = Input.GetAxis("Vertical");
      // 상하의 움직임을 -1부터 1까지 VerticalKey로 저장
      float HorizontalKey = Input.GetAxis("Horizontal");
  // 좌우의 움직임을 -1부터 1까지 HorizontalKey로 저장

      AirCraft.velocity = new Vector2(HorizontalKey, VerticalKey) * Speed;
      // 플레이어의 속도를 상하좌우(wsad 혹은 방향키)로 설정하여
      // 방향키를 누른 거에 따라 속도가 달라지게 구현
  }
  ```

  + 이때 상하좌우에 대한 정보는 유니티의 Edit -> Project Settings -> Input Manager -> Axes 에서
    조정할 수 있다.  그리고 각 component를 클릭하면 호출하는 키를 알 수 있다.

  > Input.GetAxis(string name): -1부터 1까지의 값을 반환. 부드러운 움직임 구현 가능
  > 
  > Input.GetAxisRaw(string name): -1, 0, 1 세 값 중 하나를 반환. 경직적인 움직임 구현
