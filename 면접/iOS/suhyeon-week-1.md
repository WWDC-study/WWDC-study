## Frame ↔️ Bounds
1. Frame
   - parent view의 좌표계를 기준으로 한 원점, 사이즈를 나타냄
   - origin: parent view의 원점으로부터 떨어진 위치
   - size: 뷰를 감싸는 직사각형의 사이즈 
  
2. Bounds
   - 뷰 자신의 좌표계를 기준으로 한 원점, 사이즈를 나타냄
   - origin: 뷰 자신의 좌표계 원점
   - size: 뷰 자체의 사이즈 

### 특징
1. Frame
    - 뷰를 회전했을 때 사이즈가 변할 수 있다.
2. Bounds
    - origin을 변경하면, subview가 들을 반대방향으로 이동시킨다.
    - 이유: 좌표계의 원점(origin)이 변경되는 것이기 때문이다.
  