# Styled Components

Styled Components는 리액트에서 CSS를 작성하고 관리하기 위한 라이브러리이다. Styled Components는 자바스크립트 파일에 CSS를 작성하는 "CSS-in-JS" 라이브러리이므로 사용하지 않는 CSS 코드와 스타일은 자동으로 제거되며 BEM 방법론과 같은 CSS 클래스 명명을 관리할 필요가 없다는 장점이 있다. 또한 props를 통해 동적으로 스타일을 적용할 수 있다.

<br>

### Installing styled-component

먼저 프로젝트에 npm이나 yarn 커맨드로 설치한 후 `styled`를 import 해 사용할 수 있다.

```
npm install styled-components
```

```javascript
import styled from 'styled-components'
```

<br>

### Building your first styled component

`styled`는 HTML 요소나 리액트 컴포넌트에 스타일을 적용하기 위한 키워드이다. 스타일 지정 후 HTML 요소 또는 컴포넌트로 리턴된다.

- HTML 요소 스타일링 

  ```javascript
  import styled from 'styled-components'
  
  styled.button`
  `	...
  ;
  
  // styled.element
  ```

- 리액트 컴포넌트 스타일링

  ```javascript
  import styled from "styled-components";
  import Button from "./Button";
  
  styled(Button)`
  	...
  `;
  
  // styled(component)
  ```

#### Example

```javascript
// App.js
import React from 'react';
import styled from 'styled-components';
// Button 컴포넌트는 스타일이 적용된 <a> 태그로 렌더링된다.
const Button = styled.a`
  background-colour: teal;
  color: white;
  padding: 1rem 2rem;
`
const App = () => {
  return (
    <Button>I am a button</Button>
  )
}
export default App;
```

<br>

### Using props to customize Styled Components

props를 활용하면 컴포넌트 스타일을 동적으로 적용할 수 있다.

아래의 코드는 `Button`의 `primary` prop의 boolean 값을 통해 배경색을 적용하는 예제이다.  

```javascript
// App.js
import React from 'react';
import styled from 'styled-components';
import Button from './Button';
const App = () => {
  return (
    <>
      <Button>I am a button</Button>
      <Button primary>I am a primary button</Button>
    </>
  )
}
export default App;
```

```javascript
// Button.js
import styled from 'styled-components';
const Button = styled.a`
  display: inline-block;
  border-radius: 3px;
  padding: 0.5rem 0;
  margin: 0.5rem 1rem;
  width: 11rem;
  border: 2px solid white;
  background: ${props => props.primary ? 'white' : 'palevioletred' }
  color: ${props => props.primary ? 'palevioletred' : 'white' }
`
export default Button;
```

`{css}`를 import해 동적 스타일 적용 코드 부분을 따로 분리할 수도 있다.

```javascript
// Button.js
import styled, { css } from 'styled-components';
const Button = styled.a`
  display: inline-block;
  border-radius: 3px;
  padding: 0.5rem 0;
  margin: 0.5rem 1rem;
  width: 11rem;
  background: transparent;
  color: white;
  border: 2px solid white;

  ${props => props.primary && css`
    background: white;
    color: palevioletred;
  `}
`
export default Button;
```

<br>

### Using media-queries to make your Styled Components responsive 

Styled Components는 CSS와 마찬가지로 media query를 지원하며 이를 사용해  반응형 디자인을 구현할 수 있다.

```javascript
// Button.js
import styled from 'styled-components';
const Button = styled.a`

	...
	
  @media (min-width: 768px) { 
    padding: 1rem 2rem;
    width: 11rem;
  }
  
  @media (min-width: 1024px) { 
    padding: 1.5rem 2.5rem;
    width: 13rem;
  }
`
export default Button;
```

<br>

### Creating global styles

전역적으로 스타일을 적용하려면 컴포넌트 트리의 최상단에 위치한 컴포넌트에서 `createGlobalStyle` 함수로 컴포넌트 스타일을 설정해야한다.

```javascript
// App.js
import React from 'react';
import styled, { createGlobalStyle } from 'styled-components';
import { Container, Nav, Content } from '../components';

const GlobalStyle = createGlobalStyle`
  :root {
	--font-regular: 16px;
  }
  html {
    font-family: Open-Sans, Helvetica, Sans-Serif;
	font-size: var(--font-regular);
  }
  body {
    margin: 0;
    padding: 0;
    background: teal;
  }
`;

const App = () => {
  return (
    <GlobalStyle />
    <Container>
      <Nav />
      <Content />
    </Container>
  )
}
export default App;
```

<br>

## Best Practices

다음은 Styled Components를 실제 프로젝트에서 사용하는 방식에 대한 예제들이다.

### Co-located Styled Components

스타일 컴포넌트를 실제 컴포넌트 파일 내부에 작성하는 방식이다. 각 스타일 컴포넌트에 대한 스타일 정의를 쉽게 찾을 수 있고 수정이 용이하다. 그러나 컴포넌트의 사이즈가 커질 경우 다른 파일로 분리하는 것이 낫다.

```javascript
const Content = ({ title, children }) => {
  return (
    <section>
      <Headline>{title}</Headline>
 
      <span>{children}</span>
    </section>
  );
};
 
const Headline = styled.h1`
  color: red;
`;
```



### Import Styled Components as Object

스타일 컴포넌트를 다른 파일에 작성한후 import해 가져오는 방식이다.  import 구문을 통해 짧고 간결하게 코드를 작성할 수 있으며  `*` 키워드를 통해 통일된 방식으로 명명이 가능하다.

```javascript
import { Headline } from './styles';
 
const Content = ({ title, children }) => {
  return (
    <section>
      <Headline>{title}</Headline>
 
      <span>{children}</span>
    </section>
  );
};
```

```javascript
import * as S from './styles';
 
const Content = ({ title, children }) => {
  return (
    <section>
      <S.Headline>{title}</S.Headline>
 
      <span>{children}</span>
    </section>
  );
};
```



### Single/Mutiple Styled Components

스타일 지정이 필요한 모든 요소들을 스타일 컴포넌트로 구성하는 방식(`#1`)과 단일 스타일 컴포넌트에서 HTML 요소들에 대한 스타일 지정을 하는 방식(`#2`)에 대한 예제이다. `#2`의 경우 요소와 스타일 매칭에 대해 명시적이지 않기 때문에 오류가 발생하기 쉽다는 단점이 있다.

```javascript
// #1
const Section = styled.section`
  border-bottom: 1px solid grey;
  padding: 20px;
`;
 
const Headline = styled.h1`
  color: red;
`;
 
const Text = styled.span`
  padding: 10px;
`;
 
const Content = ({ title, children }) => {
  return (
    <Section>
      <Headline>{title}</Headline>
 
      <Text>{children}</Text>
    </Section>
  );
};
```

```JAVASCRIPT
// #2
const Container = styled.section`
  border-bottom: 1px solid grey;
  padding: 20px;
 
  h1 {
    color: red;
  }
 
  .text {
    padding: 10px;
  }
`;
 
const Content = ({ title, children }) => {
  return (
    <Container>
      <h1>{title}</h1>
 
      <span className="text">{children}</span>
    </Container>
  );
};
```

<br>

## Examples

다음은 Styled Component의 `createGlobalStyle`과 `ThemeProvider`, props를 사용해 테마 모드를 변경하는 예제이다.



- **theme.ts**

  ```javascript
  // theme.ts
  
  const fonts = {
  	...
  };
  
  const defaultTheme = {
    fonts,
      ...
  };
  
  export const darkTheme = {
    ...defaultTheme,
    color: 'white',
    backgroundColor: 'black',
  };
  
  export const lightTheme = {
    ...defaultTheme,
    color: 'black',
    backgroundColor: 'white',
  };
  
  ```

- **GlobalStyle.ts**

  ```javascript
  import { createGlobalStyle } from 'styled-components';
  import reset from 'styled-reset';
  import { themeType } from './styled';
  
  export const GlobalStyle = createGlobalStyle`
    ${reset}
    * {
      margin: 0;
      padding:   0;
      box-sizing:border-box;
    }
    html {
      font-size: 16px;
      font-family: 'Helvetica',sans-serif;
      color: ${({ theme }: { theme: themeType }) => theme.color}; 
    }
    body {
      width: 100vw;
      height: 100vh;
      background: ${({ theme }: { theme: themeType }) => theme.backgroundColor};
    }
  `;
  ```

- **App.tsx**

  ```javascript
  import { useState } from 'react';
  import { GlobalStyle } from './GlobalStyle';
  import { ThemeProvider } from 'styled-components';
  import { darkTheme, lightTheme } from './theme';
  
  function App() {
    const [theme, setTheme] = useState(darkTheme);
  
    const switchTheme = () => {
      const updatedTheme = theme === darkTheme ? lightTheme : darkTheme;
      setTheme(updatedTheme);
    };
  
    return (
      <>
        <ThemeProvider theme={theme}>
          <GlobalStyle />
          	<h2>Example</h2>
              <button onClick={switchTheme}>Click</button>
        </ThemeProvider>
      </>
    );
  }
  
  export default App;
  ```

<br>

<br>

------

**Reference**

- [How To Use Styled-Components In React: A Quick Start Guide](https://scalablecss.com/styled-components-quickstart-guide/)
- [Styled Components Best Practices](https://www.robinwieruch.de/styled-components)