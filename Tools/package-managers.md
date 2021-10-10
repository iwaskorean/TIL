# Package Managers

패키지 관리자(package manager)란 프로젝트가 제대로 작동하기 위해 필요한 의존성(dependencies,  외부에서 작성된 코드 또는 모듈과의 연결)을 관리할 수 있는 소프트웨어이다.

<br>

### What is a package?

패키지는 전역 레지스트리에서 개발자의 로컬 환경으로 다운로드할 수 있는 재사용가능한 소프트웨어이다. 하나의 패키지가 올바르게 작동하게 위해 다른 패키지를 의존하는 것이 일반적이다.

<br>

## What is package managers?

패키지 관리자는 아래와 같은 부분들을 처리한다.

### Project Code

다양한 의존성들을 관리해야하는 코드들이다. 일반적으로 이 코드는 Git과 같은 버전 관리 시스템에 체크인된다.

### Manifest file

manifest file이란 모든 의존성(관리해야하는 패키지들)을 추적하는 파일이다. 이 파일은 프로젝트에 대한 다른 메타데이터도 포함한다. 자바스크립트에서의 menifest file은 `package.json`이다.

```json
// package.json
{
  "name": "ex",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "dependenciesExample": "^0.21.1"
  }
}

```

### Dependency code

의존성 코드란 의존성을 구성하는 코드이다. 어플리케이션의 수명동안 수정되서는 안되며 필요할때 프로젝트 코드에서 접근할 수 있어한다.

### Lock file

lock file은 패키지 관리자에 의해 자동으로 작성되는 파일이다. 이 파일에는 전체 의존성 트리를 나타내는데 필요한 모든 정보 및 프로젝트의 의존성들에 대한 정보가 포함되어 있다. 

패키지 관리자는 `package.json`에 `^`로 명시되어있는 버전의 패키지들을 최신 버전으로 설치하는데 이로 인해 설치시점에 따라 패키지들의 버전이 달라질 수 있다. 그러나 lock file이 존재하는 경우 lock file 내의 버전 정보를 통해 패키지를 설치하기 때문에 이 문제를 해결할 수 있다.

```json
// yarn.lock

"@babel/code-frame@7.10.4":
  version "7.10.4"
  resolved "https://registry.yarnpkg.com/@babel/code-frame/-/code-frame-7.10.4.tgz#168da1a36e90da68ae8d49c0f1b48c7c6249213a"
  integrity sha512-vG6SvB6oYEhvgisZNFRmRCUkLz11c7rp+tbNTynGqc6mS1d5ATd/sGyV6W0KZZnXRKMTzZDRgQT3Ou9jhpAfUg==
  dependencies:
    "@babel/highlight" "^7.10.4"

"@babel/code-frame@7.12.11":
  version "7.12.11"
  resolved "https://registry.yarnpkg.com/@babel/code-frame/-/code-frame-7.12.11.tgz#f4ad435aa263db935b8f10f2c552d23fb716a63f"
  integrity sha512-Zt1yodBx1UcyiePMSkWnU4hPqhwq7hGi2nFL1LeA3EUl+q2LQx16MISgJ0+z7dnmgvP9QtIleuETGOiOH1RcIw==
  dependencies:
    "@babel/highlight" "^7.10.4"
```

<br>

## NPM vs Yarn

### npm

npm은 node package manage의 약자이며 자바스크립트 런타임 환경인 Node.js의 기본 패키지 관리자이다. 

### yarn

yarn은 yetanother resource negotiator의 약자이며 Facebook에서 개발한 오픈 소스이다. npm의 성능 및 보안 이슈를 해결하기 위한 의도로 개발되었다.

<br>

### Installation Procedure

npm은 별도 설치없이 Node와 함께 자동으로 설치되지만 yarn은 npm이 설치되어있어야만 설치할 수 있다.

```
npm install yarn --global
```

### lock file

yarn은 항상 lock file을 생성하지만 npm은 자동으로 lock file을 생성하지 않는다. npm의 lock file인 `pacakge-lock.json`은 npm으로 `package.json` 파일이나 `node_modules` 트리를 수정할때 생성된다.

### Fetching packages

npm은 `npm install` 커맨드를 통해 npm 레지스트리에서 패키지를 가져오는 반면 yarn은 의존성을 로컬에 저장하고 `yarn add` 커맨드를 통해 디스크에서 가져온다(특정 버전의 의존성이 현재 로컬에 있을때)

<br>

### npm vs yarn commands

| command                | npm                                                          | yarn                                                         |
| :--------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Install dependencies   | npm install                                                  | yarn                                                         |
| Install package        | npm install package_name npm install package_name@version_number | yarn add package_name yarn add package_name@version_number   |
| Uninstall package      | npm uninstall package_name                                   | yarn remove package_name                                     |
| Install dev package    | npm install package_name –save-dev                           | yarn add package_name –dev                                   |
| Update dev package     | npm update package_name npm update package_name@version_number | yarn upgrade package_name yarn upgrade package_name@version_number |
| View package           | npm view package_name                                        | yarn info package_name                                       |
| Global install package | npm install -g package_name                                  | yarn global add package_name                                 |



| npm                 | yarn                 |
| :------------------ | :------------------- |
| npm init            | yarn init            |
| npm run [script]    | yarn run [script]    |
| npm list            | yarn list            |
| npm test            | yarn test            |
| npm link            | yarn link            |
| npm login or logout | yarn login or logout |
| npm publish         | yarn publish         |

<br>

## devDependencies vs dependencies

패키지 관리자를 통해 프로젝트에 dependency를 저장하게 되면 manifest file에 dependencies 또는 devDependencies에 dependency의 이름과 버전이 업데이트된다.

<br>

### devDependencies

다음과 같은 커맨드로 dependency를 devDependencies로 설치할 수 있다.

```
npm install 'package-name' --save-dev 
	or
yarn add --dev 'package-name'
```

devDependencies은 개발중에만 필요한 모듈이며 nodemon, babel, eslint나 chai, mocha와 같은 테스팅 프레임워크들이 해당된다.

<br>

### dependencies

다음과 같은 커맨드로 dependency를 dependencies로 설치할 수 있다.

```
npm install 'package-name' --save 
	or
yarn add 'package-name'
```

devDependencies와 비교해 dependencies는 런타임에도 필요한 모듈이라는 차이가 있다. react, redux, express, axios 등과 같은 패키지들이 dependencies에 해당된다.

<br>

<br>

------

**Reference**

- [An introduction to how JavaScript package managers work](https://www.freecodecamp.org/news/javascript-package-managers-101-9afd926add0a/)
- [Difference between npm and yarn](https://www.geeksforgeeks.org/difference-between-npm-and-yarn/)
- [NPMmmm #1: Dev Dependencies, Dependencies](https://medium.com/@dylanavery720/npmmmm-1-dev-dependencies-dependencies-8931c2583b0c)

 

