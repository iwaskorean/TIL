# Data Fetching

기본적으로 Next.js는 모든 page들을 pre-render하며 pre-render에는 static generation과 server-side rendering 두 가지 형태가 있다. 

static generation에서는 getStaticProps와 getStaticPaths 함수를 통해, server-side rendering에서는 getServerSideProps 함수를 통해 데이터를 가져올 수 있다.

<br>

### getStaticProps(static generation)

getStaticProps는 빌드 타임에 데이터를 가져오는 함수이며, 아래와 같은 경우에 사용한다.

- 사용자 요청에 앞서 빌드 시 page 렌더링에 필요한 데이터가 있을 때
- Headless CMS로부터 오는 데이터가 있을 때
- 캐시될 수 있는 데이터가 있을 때
- 성능 향상 및 SEO를 위해 pre-rendering 해야할 때

<br>

```javascript
export async function getStaticProps(context) {
  return {
    props: {}, // props 객체가 page 컴포넌트에 props로 전달된다.
  }
}
```

getStaticProps의 `context` 파라미터는 아래와 같은 key를 포함하는 객체이다.

- params : 동적 경로를 사용하는 page의 경로를 포함한다. 예를 들어 page 이름이 `[id].js` 일 경우 params는 `{id : ...}` 이다.
- preview : page가 preview 모드일 경우 `true`, 그렇지 않을 경우 `undefined` 값을 가진다.
- preivewData : preview 모드에 대한 데이터를 가지고 있다.
- locale, locales, defaultLocale 

또한, getStaticProps는 아래와 같은 객체를 반환해야 한다.

- props : page 컴포넌트에 전달될 props 객체이다.

- revalidate : page re-generate가 일어날 수 있는 초 단위의 시간을 가진 객체이다.

  ```javascript
  export async function getStaticProps() {
    const res = await fetch('https://.../posts')
    const posts = await res.json()
  
    return {
      props: {
        posts,
      },
      revalidate: 1,
        // 요청이 들어올때마다 1초에 한 번씩
        // re-generate를 시도한다
    }
  }
  
  ```

- notFound : 404 page로 반환하도록 하는 객체이다. 

  ```javascript
  export async function getStaticProps(context) {
    const res = await fetch(`https://.../data`)
    const data = await res.json()
  
    if (!data) {
      return {
        notFound: true,
      }
    }
  
    return {
      props: { data }, props
    }
  }
  ```

- redirect : 내/외부 리소스로의 redirect을 허용하는 객체이다.

  ```javascript
  export async function getStaticProps(context) {
    const res = await fetch(`https://...`)
    const data = await res.json()
  
    if (!data) {
      return {
        redirect: {
          destination: '/',
          permanent: false,
        },
      }
    }
  
    return {
      props: { data }, props
    }
  }
  ```

<br>

#### Techical details

getStaticProps 함수의 기술적인 세부사항은 아래와 같다.

- 빌드 타임에만 실행된다. 따라서 HTML header나 query 파라미터와 같이 요청시에만 사용할 수 있는 데이터들은 수신하지 않는다.
- getStaticProps는 server-side에서 실행되기 때문에 server-side 로직 및 코드를 직접 작성할 수 있다. 
- HTML과 JSON 둘 다 정적으로 생성된다.
- page에서만 허용된다. page가 렌더링 되기전 React에 필요한 모든 데이터를 가지고 있어야하기 때문이다.
- development(`next dev`)에서는 getStaticProps에 모든 요청에 대해 호출된다.
- preview 모드를 지원한다. 빌드 타임이 아닌 요청시에 렌더링할 수 있다.

<br>

#### Example

getStaticProps를 사용해 CMS에서 블로그 포스트 목록을 가져오는 예제이다.

```javascript
// with ts

import { InferGetStaticPropsType } from 'next'

type Post = {
  author: string
  content: string
}

export const getStaticProps = async () => {
  const res = await fetch('https://.../posts')
  const posts: Post[] = await res.json()

  return {
    props: {
      posts,
    },
  }
}

// Blog 컴포넌트
function Blog({ posts }: InferGetStaticPropsType<typeof getStaticProps>) {

}

export default Blog
```

<br>

###### ...

###### getStaticPaths(static generation)

###### getServerSideProps(server-side rendering)

###### ...

<br>

<br>

------

**Reference**

- [Data Fetching](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation)