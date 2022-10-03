# 고양이 짤방 생성기

https://woohyeonjoe.github.io/cat-jjal-maker/

> 고양이 짤방을 생성하는 리액트 앱입니다

![328234-0-resize](https://user-images.githubusercontent.com/3839771/149098995-0b89419a-58fb-494a-ade3-27aae5342553.gif)


# 만들면서 배우는 리액트 : 기초

### 섹션 0. 인트로

**Live Server** 설치 → 수정 시 웹사이트에 즉시 적용 가능

**Visual Code Settings**열기 : `ctrl + ,` → `format on save` 체크 (코드를 깔끔하게 만들어 준다.)

---

### 섹션 1. 리액트가 왜 좋은가요?

자바스크립트로 빈하트 클릭시 빨간하트로 변경

```html
<script>
      console.log("야옹");

      // 1. 좋아요 버튼 찾기
      const likeButton = document.querySelector(".main-card button");
      // 2. 좋아요 버튼 눌렀을 때 이벤트
      likeButton.addEventListener("click", function () {
        // 3. 하트 색 바꾸기
        likeButton.innerHTML = "❤️";

        // 4. 고양이 사진을 추가할 곳 찾기
        const favorites = document.querySelector(".favorites");
        // 5. 새로운 고양이 사진 만들기
        const newFavoriteImage = document.createElement("img");
        newFavoriteImage.src =
          "https://cataas.com/cat/60b73094e04e18001194a309/says/react";
        // 6. 고양이 사진을 감싸는 li태그 만들기
        const li = document.createElement("li");
        // 7. li태그에 고양이 사진 넣기
        li.appendChild(newFavoriteImage);
        // 8. 방금 만든 요소 넣기
        favorites.appendChild(li);
      });
    </script>
```

요소들에 이벤트를 만들기 위해 해당 클래스 이름을 항상 갖고와야 하는 복잡한 과정.

**리액트 추가하기**

위에서 부터 리액트, 리액트돔, 바벨을 추가하는 코드

```
<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>

<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

**바벨이란?**

브라우저가 이해할 수 있도록 하는 자바스크립트 컴파일러

**JSX를 통한 자바스크립 함수 호출**

1. 중괄호`{}` 안에 사용할 자바스크립트 함수 넣기 `{catItem}`
2. 렌더링 할 위치 지정 함수 생성

```html
const 여기다가그려 = document.querySelector('#app');
```

1. 이후 해당 `render` 함수를 통해 그리기 `ReactDOM.render(catItem, 여기다가그려);`

리액트에서는 이런 const들을 element라고 부른다.

---

### **섹션 2. 리액트 앱 바닥부터 만들기**

**컴포넌트란?**

반복적인 틀로 내용만 바뀌는 공간 → 컨텐츠만 넘기면 일관적인 UI제공

📎[ant.design](http://ant.design) `오픈소스 컴포넌트 디자인`

컴포넌트를 만들땐 항상 대문자로 시작

**기존 element를 컴포넌트로 변경해보기**

컴포넌트 생성

```html
function CatItem(props) {
      return (
        <li>
          <img src={props.img} />
        </li>
      );
    }
```

컴포넌트 사용

```html
const favorites = (
      <ul class="favorites">
        <CatItem img="https://cataas.com//cat/5e9970351b7a400011744233/says/inflearn" />
        <CatItem img="https://cataas.com/cat/595f280b557291a9750ebf65/says/JavaScript" />
      </ul>
    );
```

**화살표 함수**

```html
var a = () => {
  // ...
};
```

매개변수 props를 받을때 처음부터 {}형태로 받으면 사용할때 props.img가 아닌 바로 img로 사용이 가능하다.

```html
const MainCard = ({ img }) => {
      return (
        <div class="main-card">
          <img src={img} alt="고양이" width="400" />
          <button>🤍</button>
        </div>
      );
    };
```

**리액트에서는 class가 아닌 className을 사용한다.**

**리액트 스타일링 하기**

1. className으로 스타일링
2. 인라인 스타일링
3. 최근 트렌드: [emotion.sh](http://emotion.sh), [tailwindcss.com](http://tailwindcss.com) 사용

**리액트 이벤트 처리**

버튼 이벤트

```html
const MainCard = ({ img }) => {
      function handleHeartClick() {
        console.log("하트 눌렀음");
      }
      function handleHeartMouseOver() {
        console.log("하트 스쳐 지나감");
      }
      return (
        <div className="main-card">
          <img src={img} alt="고양이" width="400" />
          <button onClick={handleHeartClick} onMouseOver={handleHeartMouseOver}>🤍</button>
        </div>
      );
    };
```

submit 이벤트

```html
const Form = () => {
      function handleFormSubmit(event) {
        event.preventDefault();
        console.log("폼 전송됨")
      }

      return (
        <form onSubmit={handleFormSubmit}>
          <input type="text" name="name" placeholder="영어 대사를 입력해주세요" />
          <button type="submit">생성</button>
        </form>
      );
    };
```

**카운트 컴포넌트 코드 구조 변경해보기 (상태 끌어올리기)**

```html
const App = () => {
      const [counter, setCounter] = React.useState(1);

      console.log("카운터", counter);

      function handleFormSubmit(event) {
        event.preventDefault();
        console.log("폼 전송됨")
        setCounter(counter + 1);
      }

      return (
        <div>
          <Title> {counter}번째 고양이 가랏대 </Title>
          <Form handleFormSubmit={handleFormSubmit} />
          <MainCard img="https://cataas.com/cat/60b73094e04e18001194a309/says/react" />
          <Favorites />
        </div>
      );
    };
```

App 자체에서 컴포넌트를 구현하고 그 컴포넌트 자체를 넘긴다.

`<Form handleFormSubmit={handleFormSubmit} />`

**리스트**

```html
function Favorites() {
      const CAT1 = "https://cataas.com/cat/60b73094e04e18001194a309/says/react";
      const CAT2 = "https://cataas.com//cat/5e9970351b7a400011744233/says/inflearn";
      const CAT3 = "https://cataas.com/cat/595f280b557291a9750ebf65/says/JavaScript";

      const cats = [CAT1, CAT2];

      return (
        <ul className="favorites">
          {cats.map((cat) => (<CatItem img={cat} key={cat} />))}
        </ul>
      );
    }
```

**로컬스토리지에 데이터 싱크하기**

삽입

`localStorage.setItem("counter", counter);`

사용

```html
const [counter, setCounter] = React.useState(
        Number(localStorage.getItem("counter"))
      );
```

---

### **섹션 3. 지금까지 만든 앱 배포하기**

---

### 섹션 4. 중간 정리

**JSX**

JavaScript + XML

HTML 태그 안에서 중괄호({}) 를 사용해서 JS를 쓸 수 있다.

**Babel**

JSX를 브라우저가 이해 할 수 있도록 해주는 컴파일러

**상태**

const [counter, setCounter] = React.useState(초기값)

funtion 카운터증가() {

setCounter(counter+1);

}

**리스트**

const favorites = [”이미지1”, ”이미지2”, ”이미지3”]

<ul>

{favorites.map(image => <img src={iamge}</image>)}

</ul>

---

### 섹션 5. 리액트 앱에 숨 불어넣기

API 사용 [https://cataas.com/#/](https://cataas.com/#/)

`React.useEffect()`

---

### 섹션 6. 실무 개발환경 만들기

배포 시 유의사항

1. react.develoment → react.production으로 변경
2. 바벨 제거 → create-react-app이 자동으로 처리 해줌

**create-react-app 만들기**

1. 노드 설치 
2. `npx create-react-app cat-jjal-maker-cra` 명령어 입력
3. `cd cat-jjal-maker-cra`
4. `npm start`

**github page로 배포하기**

1. cra폴더로 이동
2. `npm run build` → build 파일이 생성됨
3. npm install gh-pages
4. package.json파일에 

"homepage": "https://woohyeonjoe.github.io/cat-jjal-maker",

"deploy": "gh-pages -d build”   ←  이건 디버그 쪽에

코드 추가

1. 다시 `npm run build`
2. `npm run deploy`

## 연관 리액트 강의

인프런의 [만들면서 배우는 리액트: 기초](https://www.inflearn.com/course/%EB%A7%8C%EB%93%A4%EB%A9%B4%EC%84%9C-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EA%B8%B0%EC%B4%88) 강의에서 만드는 토이 프로젝트입니다.

![KakaoTalk_Photo_2022-01-12-18-15-08](https://user-images.githubusercontent.com/3839771/149098759-6a7b4a16-5c7f-431e-8fb5-cc750fd527a2.jpeg)   
