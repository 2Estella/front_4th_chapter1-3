## 과제 체크포인트

### 기본과제

- [x] shallowEquals 구현 완료
- [x] deepEquals 구현 완료
- [x] memo 구현 완료
- [x] deepMemo 구현 완료
- [x] useRef 구현 완료
- [x] useMemo 구현 완료
- [x] useDeepMemo 구현 완료
- [x] useCallback 구현 완료

### 심화 과제

- [x] 기본과제에서 작성한 hook을 이용하여 렌더링 최적화를 진행하였다.
- [x] Context 코드를 개선하여 렌더링을 최소화하였다.

## 과제 셀프회고

리액트의 훅들을 구현하며 이 훅들이 어떤 방식으로 동작 하는지, 또 리액트에서 메모이제이션을 이용하여 어떻게 성능 최적화를 진행 하는지에 대해 깊게 파고 들을 수 있어서 좋았습니다.
회사에서 Vue를 사용하여 리액트에 대해서 잘 알지 못 했는데 이번 과제를 진행하며 정말 많이 배웠습니다!
(다들 1~2차 과제에 비하면 쉽다고 하셨지만 저는 이번 과제가 제일 어려웠어요 🥲🥲🥲)

기본과제는 리액트의 훅에 대해 공부하는 데에 시간을 많이 할애 했다면 심화과제는 혼돈의 카오스,, 그 자체였습니다. 무엇보다 컴포넌트 간 의존성을 줄이고, 컨텍스트를 관심사별로 분리하면서 해당 Provider를 알맞게 사용하여 최적화하는 부분에서 많이 고민했던 것 같습니다. 그래도 덕분에 `useCallback`, `useMemo`, `memo` 등 최적화 훅에 대하여 개념을 잡고, 어떤 방식으로 최적화를 진행하는지 배우게 되어 좋았습니다!

### 기술적 성장

- 리액트의 성능 최적화를 위한 `useMemo`, `useCallback`, `memo` 등의 훅과 메모이제이션 기법을 심도 있게 배웠습니다.
    - `useCallback` 을 활용하여 함수의 불필요한 생성을 방지하거나 `memo` 를 이용하여 props가 업데이트 될 때만 렌더링을 하도록 한다는 것을 알게 되었습니다.
- 테스트를 진행하며 최적화를 검증하는 테스트 코드에 대해 학습해 볼 수 있었고, 어떤 방식으로 최적화를 검증해야하는 지 알게 되었습니다.
- ThemeProvider, NotificationProvider, UserProvider등 각 provider의 중첩 구조가 렌더링 최적화에 미치는 영향을 고민 해보고, Context의 세분화를 진행했습니다.
- 컴포넌트 간 의존성을 최소화 하기위해 고민해보고, 최적화를 위해 렌더링 시 영향을 주지 않도록 방향을 잡아보았습니다.

### 코드 품질

- 👍😎 :  `useMemo` 를 직접 구현해보며 어떤 원리인지 파악할 수 있어서 좋았습니다!
    
    ```tsx
    export function useMemo<T>(
      factory: () => T,
      _deps: DependencyList,
      _equals = shallowEquals,
    ): T {
      // 초기값을 null로 설정하여 의도적으로 "값이 없음"을 명시
      const ref = useRef<{
        value: T | null;
        deps: DependencyList;
      }>({
        value: null,
        deps: [],
      });
    
      // 의존성 배열이 변경된 경우 또는 초기화가 필요한 경우에만 factory 실행
      if (ref.current.value === null || !_equals(ref.current.deps, _deps)) {
        ref.current.value = factory();
        ref.current.deps = _deps;
      }
    
      if (ref.current.value === null) {
        throw new Error("useMemo: 값이 초기화되지 않았습니다.");
      }
    
      return ref.current.value;
    }
    ```
    

- 🤔🤔 : context와 provider를 한 파일에 함께 작성했는데 기회가 된다면 세분화하고, 반복되는 부분은 훅으로 만들어봐야겠습니다.

### 학습 효과 분석

- **가장 큰 배움이 있었던 부분**: Context API 성능 최적화 및 렌더링 최소화 기법에 대해 깊이 있는 학습을 할 수 있었습니다. 특히, `useCallback`과 `useMemo`를 활용한 함수 재생성 방지와 상태 관리 최적화 방법을 알게되어 좋았습니다.
- **추가 학습이 필요한 영역**: 이번에 성능 최적화에 대해 학습 하면서 새롭게 알게 된 React의 Suspense 및 Concurrent Mode를 활용한 비동기 렌더링 최적화에 대한 추가 학습을 해보고 싶습니다.


### 과제 피드백

- 과제를 통해 리액트의 메모이제이션과 렌더링 최적화에 대해 학습하고, 고민해볼 수 있어서 좋았습니다.


## 리뷰 받고 싶은 내용

- 처음으로 Context API를 사용해봤는데, 올바르게 구조화가 된 건지 궁금합니다. context 내부에서 쓰인 `useCallback` 과 `useMemo` 가 적절하게 쓰인 건지도 알고 싶습니다.
- 또한, 사용하다 보니 아래와 같이 Provider로 인하여 App.tsx의 구조가 다소 복잡해 보입니다. 실무나 더 큰 프로젝트에서는 더 복잡한 구조를 가질텐데 이런 경우 어떻게 사용하는 지도 알고 싶습니다.(그 시점이 상태관리 라이브러리로 넘어가는 단계가 되는 것 일까요?)
    
   ```tsx
    	<ThemeProvider>
          <NotificationProvider>
            <UserProvider>
              <Header />
    
              <BaseLayout>
                <div className="w-full md:w-1/2 md:pr-4">
                  <ItemList items={items} onAddItemsClick={addItems} />
                </div>
                <div className="w-full md:w-1/2 md:pl-4">
                  <ComplexForm />
                </div>
                <NotificationSystem />
              </BaseLayout>
            </UserProvider>
          </NotificationProvider>
        </ThemeProvider>
    ```
