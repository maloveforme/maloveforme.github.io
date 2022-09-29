---
title: "Background Settings"
layout: single
categories:
  - Unity
tags:
  - Unity
  - Unity2DGame
---

### 배경 설정
1. 배경으로 쓸 이미지를 Project에서 Hierarchy에 옮겨 생성한다.

![background](/assets/images/2022_Background(1).png)

2. 크기를 화면에 맞게 조절한다.

---

### 바닥 설정
1. 운석이 바닥과 부딪혔을 때 파괴되어야 하므로 바닥을 만든다.
2. Hieararchy에서 2D Object -> Sprites -> Square을 만들고 바닥에 깐다.

![Floor](/assets/images/2022_Floor.png)

---

### 우선순위 설정
- 앞선 방법들을 따라왔다면 내가 생각한 화면이 안 나올 것이다.
- 이를 해결하기 위해 Sprite Renderer의 Sorting Layer을 조정한다.

> **Sprtie Renederer**: Sprite를 렌더링하고 씬에서 시각적으로 표시되는 방식을 제어

- Player와 운석이 제일 먼저 와야 하고, Floor가 배경 뒤에 숨어 있어야 하므로 다음과 같이 설정하였다.

![SortingLayer](/assets/images/2022_Background.png)

- 추가로 바닥은 후에 충돌을 해야 하므로 Rigidbody와 Collider를 추가해주었다. 

![Floor2](/assets/images/2022_Floor1.png)
