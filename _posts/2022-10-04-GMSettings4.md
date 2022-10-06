---

title: "GameManager Settings(4)"
layout: single
categories:
  - Unity
tags:
  - Unity2DGame
  - '2022'

published: true
---



## 목표

- 게임 시작 / 종료 기능을 구현.
- 현재 점수와 최고 점수 기능을 구현



---

### 게임 시작 / 종료 기능

<br>

1. 게임 종료 기능

   - 플레이어가 파괴되거나 목숨이 0이 되면 최고 점수를 저장하고, 특정 키를 누르면 리셋되게 하는 기능

   ![image-20221004142525334](/assets/images/2022-10-04-GMSettings4/image-20221004142525334.png)

   - 다음 화면과 같이 Canvas 에 Empty를 생성하고, 그 밑에 화면에 표시할 Base를 생성한다. 마지막으로 Base 밑에 Text를 생성한다. 

   - SetActive(false)를 이용해 게임이 종료된 상황에서 이 팝업창을 띄우게 구현하는 코드는 다음과 같다.

     ```csharp
     [SerializeField] GameObject Popup;
     ...
     public void GameOver()
     {
        Popup.SetActive(true); 
        Time.timeScale = 0; // 화면 일시정지
     }
     ```

     - 이 GameOver() 함수를 Meteor 스크립트에서 플레이어와 충돌 했을 때와 life가 0일 때 추가해준다.

       ```c#
       ...
       
       private void OnTriggerEnter2D(Collider2D collision)
       {
           if (collision.CompareTag("Player"))
           {
               collision.gameObject.SetActive(false);
               GameManager.instance.PlayerLife = 0;
               GameManager.instance.ShowHeart();
               GameManager.instance.GameOver(); // 추가된 함수
           } 
           ...
           
           if(GameManager.instance.PlayerLife == 0)
           {
               GameManager.instance.GameOver();
           }
       }
       ```



2. 게임 시작 기능

   - 게임을 다시 시작하려면 게임 점수를 0을 초기화 -> 플레이어를 재활성화 -> 팝업창 종료라는 방법

   - 불러올 Scene을 다시 재로드하는 방법이 있는데, 후자가 간편하므로 이를 이용해보자. 이때 SceneManagement를 임포트 해줘야 한다.

     ```csharp
     using UnityEngine.SceneManagement; // SceneManager를 이용하기 위해 임포트
     ...
     
     public void GameStart()
     {
         Time.timeScale = 1; // 일시정지 해제
         SceneManager.LoadScene("Filed");
         // 필자의 경우 현재 Scene의 이름을 Filed으로 해놈
     }
     ```

     <br>

---

### 점수 기능 구현

<br>

- 최고 점수를 PC의 레지스트리에 저장하고 불러오는 기능을 구현해보자.

- Unity에서는 이러한 데이터 관리를 용이하게 해주는 **PlayerPrefs** 클래스를 제공하는데, 이를 간단하게 알아보자.

  <br>

  ![image-20221004150241121](/assets/images/2022-10-04-GMSettings4/image-20221004150241121.png)

<br>

- 이를 이용해 Player의 Best Score를 지정해 저장하는 코드를 작성하면 다음과 같다.

  ```c#
  public void GameOver()
  {
      if(!PlayerPrefs.HasKey("BestScore")) // 게임을 종료할 때 BestScore가 없으면
      {
          PlayerPrefs.SetInt("BestScore", Score); // 현재 점수를 BestScore를 저장
      }
  
      else
      {
          if(Score > PlayerPrefs.GetInt("BestScore", Score)) // 현재 Score가 BestScore보다 크면
          {
              PlayerPrefs.SetInt("BestScore", Score); // 현재 Score를 BestScore로 저장
          }
      }
      
      TextPopup.text = 
          "Best Score: " + PlayerPrefs.GetInt("BestScore").ToString(); // 최고 점수를 Text로 출력
      ...
  }
  ```

- 그리고 R버튼을 누르면 재시작하는 기능을 구현하기 위해 Update() 함수에서 코드를 추가해주자.

  ```c#
  void Update()
  {
      ...
          
      if (Input.GetKeyDown(KeyCode.R))
      {
          GameStart();
      }
  }
  ```

<br>

- 최종적으로 Inspector 창에 오브젝트들을 추가해주면 된다.

  ![image-20221004151056276](/assets/images/2022-10-04-GMSettings4/image-20221004151056276.png)
