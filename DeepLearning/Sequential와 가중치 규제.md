### 모델 정의 : Sequential
1. 학습 데이터 로드
2. 모델 정의 : 입력층, 은닉층, 출력층을 정의하기 위해 keras에 Sequential()을 사용
    - model.add():층을 단계적으로 추가
    - model.add(Embedding(...)
    - **model.add(Dense(출력 뉴런의 수, input_dim = 입력 뉴런의 수, activation = 활성화 함수))**
3. 컴파일(Compile) : 모델을 기계가 이해할 수 있도록 컴파일
    - **model.compile(optimizer, loss, metrics)**
    - optimizer : 훈련 과정을 설정하는 옵티마이저를 설정('adam', 'sgd'와 같은 문자열로 지정)
    - loss : 훈련 과정에서 사용할 손실함수를 설정
    - metrics : 훈련을 모니터링하기 위한 지표 선택
4. 모델 학습(Fit) : 모델을 학습
    - **model.fit(훈련데이터, 레이블데이터, epoch, batch_size)**
    - epoch : 1은 전체 데이터를 한 차례 훑고 지나갔음을 의미함. 정수값 기재 필요. 총 훈련 횟수를 정의
    - batch_size : 기본값은 32. 미니 배치 경사 하강법을 사용하고 싶지 않을 경우에는 batch_size=None을 기재
    - verbose : 학습 중 출력되는 문구를 설정
    - validation_data(X_val,y_val)
5. 모델 검증(Evaluate) : 테스트 데이터를 통해 학습한 모델에 대한 정확도를 평가
6. 모델 예측(Prediction) : 임의의 입력에 대한 모델의 출력값을 확인


* * * 
## 가중치 규제(Regularization)
<= overfitting을 해결할 수 있음

### EarlyStopping
학습을 좀만 시키기 !! 에러가 증가하는 부분에서 멈춰주기

### Weight Decay
- 가중치를 감소시키는 기술
- 손실함수(=objective function)에 weight의 크기를 나타내는 항을 추가하여 weight가 커지는 것을 방지(손실함수를 최소화하는 것이 목적이므로)
    - w가 커지면 overfitting이 될 수 잇으니 w의 크기까지 loss함수에 넣겠다!
- **L1 규제** : 가중치의 절댓값에 비례하는 비용이 추가
    - activity_regularizer=regularizers.l1(0.01)
- **L2 규제** : 가중치의 제곱에 비례하는 비용이 추가 (L1 규제는 일부 가중치 파라미터를 0으로 만듬. L2 규제는 가중치 파라미터를 제한하지만 완전히 0으로 만들지는 않아 L2 규제를 더 많이 사용함)
    - kernel_regularizer=regularizers.l2(0.01)

>가중치의 초깃값을 모두 0으로 설정(균일한 값으로 설정)하면 안되는 이유  
> > 파라미터의 값이 모두 같다면 역전파(Back propagation)를 통해서 갱신하더라도 모두 같은 값으로 변하게 됨
> > 신경망 노드의 파라미터가 모두 동일하다면 여러 개의 노드로 신경망을 구성하는 의미가 사라짐
> > '가중치가 고르게 되어버리는 상황(가중치의 대칭적인 구조를 무너뜨리려면)'을 막으려면 **초깃값을 무작위**로 설정해야 함

### Constraints
- 물리적으로 Weight의 크기를 제한
- kernel_constraint=MaxNorm(2.)) ## add constraints, 2보다 크지않게 제한

### Dropout : 일부에 집착하지 않고 중요한 요소가 무엇인지 터득해 나간다
- 신경망에서 가장 효과적이고 널리 사용하는 규제 기법 중 하나
- 훈련하는 동안 층의 출력 특성을 랜덤하게 끔(즉, 0으로 만듬
- 드롭아웃 비율 : 0이 되는 특성의 비율 (보통 0.2에서 0.5 사이를 사용)
- 바로 이전 층의 출력에 드롭아웃을 적용
    - keras.layers.Dense(16, activation='relu'),
    
      keras.layers.Dropout(0.5)
