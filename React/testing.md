# Testing

테스트는 어플리케이션이 제대로 동작하는지 검증하기 위한 작업이다. 사용자의 유즈케이스가 테스트로 커버되어있다면 어플리케이션의 일부분이 제대로 동작하지 않을 때 빠르게 피드백을 얻을 수 있다.

프로젝트의 모든 기능을 하나하나 직접 테스트하는 것은 매우 비효율적이다. 그렇기 때문에 테스트 코드를 작성해 테스트 시스템이 자동으로 테스트하는 테스트 자동화가 필요하다.

테스트는 유닛 테스트(unit test)와 통합 테스트(integrated test), E2E 테스트(end to end test)로 나누어진다.

<br>

### Unit Test

유닛 테스트란 아주 조그마한 단위 즉, 완전히 분리된 컴포넌트들을 대상으로하는 테스트를 의미한다. 유닛 테스트의 예시는 다음과 같다.

- 컴포넌트가 정상적으로 렌더링되는가 확인한다.
- 컴포넌트의 특정 함수를 실행하면 state가 의도한 대로 바뀌는지 확인한다.

유닛 테스트를 통해 컴포넌트의 기능이 잘 작동하는지 확인할 수 있지만 기능들이 전체적으로 잘 동작하는지는 확인하기 어렵다는 단점이 있다.

<br>

### Integrated Test

전체적으로 기능들이 정상 작동하는가를 확인하기 위해서는 통합 테스트가 필요하다. 통합 테스트의 예시는 다음과 같다.

- 다수의 컴포넌트들을 렌더링하고 서로 상호작용이 정상적으로 동작하는지 확인한다.
- DOM 이벤트를 발생시켰을 때, 의도한 UI의 변화가 발생하는지 확인한다.

유닛 테스트는 하나의 작은 부분에 초점을 둔다면, 통합 테스트는 여러 요소들을 고려해 작성한다.

<br>

### End to End Test

E2E 테스트는 어플리케이션이 의도한대로 작동하는지 확인하기 위해 처음부터 끝까지의 전체 기능을 테스팅하는 테스트이다. E2E 테스트의 예시는 다음과 같다.

- 전체적인 유저 플로우를 확인한다.
- 테스트 커버리지의 증가를 확인한다.
- 하위 시스템의 문제점을 식별한다.

cypress와 같은 프레임워크를 통해 테스트할 수 있다.

<br>

### Testing Library

테스팅 라이브러리는 테스트를 사용자 관점에서 접근한다. 그렇기 때문에 최종적으로 통합 테스트로 연결된다.

테스트 자동화에 가장 많이 사용되는 라이브러리는 jest와 react-testing-library의 조합이다. jest는 mocha나 jasmine으로 대체할 수 있으며 react-testing-library는 enzyme로 대체할 수 있다.

<br>

## React Testing Library

react-testing-library는 모든 테스트를 DOM 위주로 진행한다. 

###### ...

###### snapshot testing, types of queries, handling event



<br>

------

**Reference**

- [[번역] 초보자를 위한 React 어플리케이션 테스트 심층 가이드](https://blog.rhostem.com/posts/2020-10-14-beginners-guide-to-testing-react-1#overview_of_testing_react_apps)
- [벨로퍼트와 함께하는 리액트 테스팅](https://velog.io/@velopert/series/react-testing)

