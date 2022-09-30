---

title: "GameManager Settings(3)"
layout: single
categories:
  - Unity
tags:
  - Unity
  - Unity2DGame
published: true
---

# 목표

- 운석을 파괴했을 때 점수를 얻는 것을 구현해보자
- 점수를 저장하는 출력하는 기능을 구현해보자.

---

<br>

### 생명 구현

- 지난 포스트 때 바닥에 한 번 떨어지면 생명이 더이상 감소하지 않는 문제가 있었다. 이는 운석이 바닥에 닿았을 때 setActive(false)로 인해 바닥이 사라져버려 그런 것이었다.
- 따라서 운석(Meteor) 스크립트의 다음 코드를 변경해주었다.

```C#
    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.CompareTag("Player"))
        {
            collision.gameObject.SetActive(false);
            GameManager.instance.PlayerLife = 0;
            GameManager.instance.ShowHeart();
        } 

        if (collision.CompareTag("Floor"))
        {
            //collision.gameObject.SetActive(false);
            GameManager.instance.PlayerLife--;
            GameManager.instance.ShowHeart();
        }
    }
```

---

<br>

### 스코어 구현하기

1. Life를 구현했던 Canvas에서 Text를 추가해 ScoreBoard로 만들자.

   - 이때 아직 Mesh Pro는 다룰 단계가 아니기 때문에 Legacy에 있는 Text를 사용했다.



2. Text를 Score: 로 표시하고 위치를 화면의 우측 상단에 위치시키기 위해 Rect Transform에서 alt + shift 를 누르고 위치를 지정한다.

![image-20220930141312769](/assets/images/2022-09-30-GMSettings3/image-20220930141312769.png)



3. 이후 텍스트와 폰트 사이즈를 조절해준다.

![image-20220930140845418](/assets/images/2022-09-30-GMSettings3/image-20220930140845418.png)



> **Horizontal Overflow**: 글자가 만약 상자의 수평(좌우)의 크기를 넘었을 때 어떻게 처리할 것인지 처리 
>
> **Vertical OverFlow**: 글자가 만약 상자의 수직(상하)의 크기를 넘었을 때 어떻게 처리할 것인지 처리



4. 이제 스크립트를 작성해보자.

   - GameManager Script

     ```C#
     ...
     public Text Scoreboard;
     public int Score = 0;
     
     ...
     public void ShowScore()
     {
         Scoreboard.text = "Score: " + Score.ToString();
     }
     ```

   - Bullet Script

     ```C#
     ...
     
         private void OnTriggerEnter2D(Collider2D collision)
     {
         if (collision.CompareTag("Enemy"))
         {
             Destroy(collision.gameObject);
             Destroy(this.gameObject);
             GameManager.instance.Score++;
             GameManager.instance.ShowScore();
         }
     }
     ```



---



