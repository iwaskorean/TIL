# Data Fetching

기본적으로 Next.js는 모든 page들을 pre-render하며 pre-render에는 static generation과 server-side rendering 두 가지 형태가 있다. 

static generation에서는 getStaticProps와 getStaticPaths 함수를 통해, server-side rendering에서는 getServerSideProps 함수를 통해 데이터를 가져올 수 있다.

<br>

## getStaticProps(static generation)

getStaticProps는 빌드 타임에 데이터를 가져오는 함수이며, 아래와 같은 경우에 사용한다.

- 사용자 요청에 앞서 빌드 시 page 렌더링에 필요한 데이터가 있을 때
- Headless CMS로부터 오는 데이터가 있을 때
- 캐시될 수 있는 데이터가 있을 때
- 성능 향상 및 SEO를 위해 pre-rendering 해야할 때

<br>

```javascript
export async function getStaticProps(context) {
  return {
    props: {}, // props 객체가 page 컴포넌트에 props로 전달된다.
  }
}
```

getStaticProps의 `context` 파라미터는 다음과 같은 키를 포함하는 객체이다.

- params : 동적 경로를 사용하는 page의 경로를 포함한다. 예를 들어 page 이름이 `[id].js` 일 경우 params는 `{id : ...}` 이다.
- preview : page가 preview 모드일 경우 `true`, 그렇지 않을 경우 `undefined` 값을 가진다.
- preivewData : preview 모드에 대한 데이터를 가지고 있다.
- locale, locales, defaultLocale 

또한, getStaticProps는 `props`와 `revalidate`를 포함한 객체를 반환해야한다.

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

### Techical details

getStaticProps는 다음과 같은 특징을 가진다.

- 빌드 타임에만 실행된다. 따라서 HTML header나 query 파라미터와 같이 요청시에만 사용할 수 있는 데이터들은 수신하지 않는다.
- getStaticProps는 server-side에서 실행되기 때문에 server-side 로직 및 코드를 직접 작성할 수 있다. 
- HTML과 JSON 둘 다 정적으로 생성된다.
- page에서만 허용된다. page가 렌더링 되기전 React에 필요한 모든 데이터를 가지고 있어야하기 때문이다.
- development(`next dev`)에서는 getStaticProps에 모든 요청에 대해 호출된다.
- preview 모드를 지원한다. 빌드 타임이 아닌 요청시에 렌더링할 수 있다.

<br>

### Example

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

## getStaticPaths(static generation)

getStaticPaths는 동적 라우팅을 사용해 페이지를 생성할 때 사용되는 함수이다. 동적 라우팅을 사용하는 페이지에서 async getStaticPaths 함수를 export 하면 Next.js는 지정된 모든 경로를 정적으로 pre-render한다.

```javascript
export async function getStaticPaths() {
  return {
    paths: [
      { params: { ... } } 
    ],
    fallback: true or false
  };
}
```

<br>

### paths

`paths` 키는 pre-render되는 경로를 지정하는 역할을 한다. 

```javascript
return {
  paths: [
    { params: { id: '1' } },
    { params: { id: '2' } }
  ],
  fallback: ...
}
```

`pages/posts/[id].js` 라는 동적 라우팅을 사용하는 page에서 getStaticPaths를 export 하면 이 page는 `paths`의 `params` `id`를 이용해 빌드 타임에  `posts/1`, `posts/2`을 정적으로 생성한다.

`params`는 page 이름에 사용된 파라미터의 이름과 일치해야한다.

- page 이름이 `pages/posts/[postId]/[commentId]` 인 경우 `params`에는 `postId`와 `commentId`가 포함되어야한다.
- `pages/[...slug]`와 같이 catch-all 라우트를 사용하는 경우 `params`에는 `slug` 배열이 포함되어야한다. 만약 `slug` 배열이 `['foo', 'bar']` 인 경우 Next.js는 `/foo/bar` 경로에 페이지를 정적으로 생성한다.

<br>

### fallback

`fallback`키가 `false`일 경우 404 page를 반환한다. pre-render할 경로의 수가 적은 경우나 새로운 page들이 자주 추가되지 않을 때  `fallback`키를 `false`로 설정하는 것이 효율적이다.

`fallback`키가 `true`일 경우 `getStaticPaths`로 부터 반환되는 경로는 빌드 타임에 HTML이 렌더링되며 빌드 타임에 생성되지 않은 경로들에 대해서는 404 page가 아닌 fallback page가 제공된다. 동적 라우팅을 사용해 pre-render할 페이지가 많을 경우 `fallback`키를 `true`로 사용하는 것이 좋다.

<br>

## getServerSideProps(server-side rendering)

getServerSideProps는 각 요청마다 반환된 데이터를 사용해 페이지를 pre-render하는 함수이다.

```javascript
export async function getServerSideProps(context) {
  return {
    props: {}, 
  }
}
```

`context` 파라미터는 다음과 같은 키를 포함하는 객체이다.

- params : 동적 라우팅을 사용하는 page일 경우, params는 라우트 파라미터를 포함하고 있다.
- req : HTTP 요청 객체
- res : HTTP 응답객체
- query : 쿼리 스트링 객체
- preview, previewData, resolveUrl, local, locales,defaultLocale

getServerSideProps는 page 컴포넌트에 전달되는 `props`와 404 page 반환 여부를 결정하는 `notFound`를 반환해야한다.

```javascript
export async function getServerSideProps(context) {
  const res = await fetch(`https://...`)
  const data = await res.json()

  if (!data) {
    return {
      notFound: true,
    }
  }

  return {
    props: {}, 
  }
}
```

<br>

### Techical details

getServerSideProps는 아래와 같은 특징을 가진다.

- 서버측에서만 실행되고 브라우저에서는 실행되지 않는다.
- getServerSideProps는 매 요청마다 실행되며 반환된 props를 컴포넌트에 전달한 후 렌더링 한다.
- page에서만 export 할 수 있다. 

<br>

### Example

```javascript
import { InferGetServerSidePropsType } from 'next'

type Data = { ... }

export const getServerSideProps = async () => {
  const res = await fetch('https://.../data')
  const data: Data = await res.json()

  return {
    props: {
      data,
    },
  }
}

function Page({ data }: InferGetServerSidePropsType<typeof getServerSideProps>) {
  // will resolve posts to type Data
}

export default Page
```

<br>

<br>

------

**Reference**

- [Data Fetching](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation)