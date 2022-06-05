## Nextjs 에서 App컴포넌트의 용도

- [참고](https://nextjs.org/docs/advanced-features/custom-app)

Nextjs는 App컴포넌트를 페이지 초기화를 위해 사용한다.

기본 app을 오버라이드하기 위해서는 ./pages/\_app.tsx 에 아래와 같이 쓰면된다.

```tsx
default export function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

App컴포넌트는 다음과 같이 사용 됨

- 페이지간의 레이아웃 변화를 유지하고 싶을 때
- 페이지 이동할때 상태를 유지하고 싶을 때
- componentDidCatch를 사용해 에러를 핸들링하고 싶을 때
- 페이지에 추가적인 데이터를 주입하고 싶을 때
- global css를 추가하고 싶을 때

## Nextjs에서 Document의 용도

- [참고](https://nextjs.org/docs/advanced-features/custom-document)

Document는 `<html>` 이나 `<body>`태그를 업데이트 할 때 사용되곤 한다. 이 파일은 오직 서버에서 랜더링 되며 Document안에서는 onClick같은 이벤트핸들러는 사용이 불가능하다.
