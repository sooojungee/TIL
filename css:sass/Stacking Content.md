# [CSS] Stacking Content

### Stacking Content (쌓임 맥락)

쌓임 순서 (Stacking Order) 란?
z-index가 높으면 위로 오고, z-index가 낮으면 아래로 가는 것이다.

쌓임 맥락 (Stacking Content) 란?
같은 부모 밑에 쌓임 순서에 따라 앞뒤로 한꺼번에 움직일 수 있는 요소를 말한다.

쌓임 맥락이 만들어지는 조건
- html 요소 그 자체
- 요소가 position: static이 아니고, z-index: auto가 아닐때
- 요소의 opacity 값이 1보다 작을 때
- position: fixed는 무조건 쌓임 맥락을 만든다

쌓임 순서
1. 쌓임 맥락의 뿌리 요소
2. position 값이 있고 z-index가 음수인 요소
3. position 값이 없는 요소
4. position 값이 있고, z-index가 auto인 요소
5. position 값이 있고, z-index가 양수인 요소

### position: fixed에서 z-index

position: fixed에서 z-index를 사용하면 제대로 작동하지 않을 때가 많다.

< 주의 할 점 >
fixed의 자식 또한 fixed 속성을 사용가능하다.
parent와 child 간에서는 child의 z-index가 우선이다.
parentA의 child는 z-index를 아무리 높여봤자 parentB에게 영향을 미칠 수 없다
parentA와 parentB가 존재할때 z-index가 지정되지 않거나 같을 경우 나중에 나온것이 더 위에 표현된다.
parentA와 parentB가 존재할 때 z-index가 더 높은 것이 위에 표시된다.
fixed 속성의 child의 z-index가 다른 parent에 속한 child보다 낮더라도 parent 수준에서 더 높은 것으로 결정되므로 parentA의 child는 z-index를 높여도 parentB에게 영향을 미칠 수 없다.
