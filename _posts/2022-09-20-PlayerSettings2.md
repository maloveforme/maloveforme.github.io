---

title:  "Player Settings(2)"
layout: single
categories:
  - Unity
tags:
  - '2022'
---

### 플레이어의 위치 제한
- 이전 게시글에서 설정한 후 플레이어를 움직이면, 화면 밖으로도 나가는 문제가 생긴다. 이를 해결하기 위해 크게 2가지 방법이 있다.

---

#### Scene 화면의 좌표값을 확인한 후, 화면을 벗어나면 그 위치를 고정시키는 방법 (스크린 좌표계 사용)
- 해당 화면에서 캐릭터가 벗어나지 않는 좌표가 다음과 같다고 가정했을 때, x의 **좌표가 -2.8 이하면 -2.8로 고정하고
2.8 이상이면 2.8로 고정**하면 된다. y의 좌표도 마찬가지로 작성하면 코드는 다음과 같아진다.

![Screen](/assets/images/Screen.png)

```csharp
  Vector3 PlayerPosition = gameObject.transform.position;

    if (PlayerPosition.x < -2.8f)
        PlayerPosition.x = -2.8f; // x 좌표가 -2.8 이하면 -2.8로 고정
    if (PlayerPosition.x > 2.8f)
        PlayerPosition.x = 2.8f; // x 좌표가 2.8 이상이면 2.8로 고정
    if (PlayerPosition.y < -2.8f)
      PlayerPosition.y = -2.8f; // y 좌표가 -2.8 이하면 -2.8로 고정
    if (PlayerPosition.y > 2.8f)
      PlayerPosition.y = 2.8f; // y 좌표가 2.8 이상이면 2.8로 고정
    
    gameObject.transform.position = PlayerPosition; // 미리 작업한 것을 실제 컴포넌트로 수정
```

> gameObject.transform.position : 해당 객체의 위치 정보를 담고 있음

---

#### 메인 카메라를 이용한 Viewport(뷰포트)를 이용하는 방법 (뷰포트 좌표계 사용)
- 해당 방법을 설명하기 전에 뷰포트의 개념부터 간단하게 알아보자.

> **ViewPort**: 스크린 좌표를 **0에서 1 사이의 비율로 상대적으로 표시**한 좌표계

- 우리가 작업한 게임 화면은 스크린 화면에서 실행된다. 
  이때 좌표는 게임 세상에서 이동하는 World 좌표와 Canvas에서 Width와 Height로 다룰 수 있는 (해상도) 스크린 좌표, 그리고 뷰포트 좌표로 나뉜다.
  앞서 설명한 스크린 좌표계를 사용하면 디스플레이를 확장하거나 축소할 때 원하는 위치에 오브젝트가 존재하지 않게 되는데, 
  이를 뷰포트를 사용하면 해결할 수 있다.

  

![ViewPort](/assets/images/ViewPort.png)

- 앞선 화면과 달리 화면이 0부터 1사이로 정규화되었다. x,y의 좌표가 0 이하면 0으로 고정하고 1 이상이면 1로 고정하면 된다. 이러한 방법으로 코드를 작성하면 다음과 같다.

  ```csharp
  Vector3 PlayerPosition = Camera.main.WorldToViewportPoint(
      gameObject.transform.position); // 뷰포트 사용.
  // 스크린을 결국 카메라를 통해 이루어지므로 Camera.main(메인카메라)을 사용
  if (PlayerPosition.x < 0)
      PlayerPosition.x = 0;
  if (PlayerPosition.x > 1)
      PlayerPosition.x = 1;
  
  gameObject.transform.position = Camera.main.ViewportToWorldPoint(
      PlayerPosition); // 뷰포트 좌표를 게임 스크린 좌표로 변환
  ```

---

> 최종적으로 코드를 함수화하여 정리하면

```csharp
  void ScreenLimit() // 화면을 제한하는 함수
  {
          Vector3 PlayerPosition = Camera.main.WorldToViewportPoint(
              gameObject.transform.position); 
          if (PlayerPosition.x < 0)
              PlayerPosition.x = 0;
          if (PlayerPosition.x > 1)
              PlayerPosition.x = 1;

          gameObject.transform.position = Camera.main.ViewportToWorldPoint(
              PlayerPosition); 
  }
```
