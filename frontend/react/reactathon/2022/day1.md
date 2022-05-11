https://www.youtube.com/watch?v=V5hPAl1q7vo&t=3619s

# State Of React

## Keynote Part 1: State of React

1. 즉각적으로 가장 관련있는 콘텐츠를 보여주기
2. 유저인풋에 바로 반응하기
3. 유저인풋에 블로킹하지 않고 이동하기

concurrent features가 위의 옵션을 해결

react 18에따라 새로운 next new routing 시스템이 생김

1. Nested routes / layouts
2. Client and Server Routing
3. React 18 features - startTransition, Suspense
4. Designed for Server Components
5. 100% incrementally adoptable

## Keynote Part 2: Shipping to the Edge

remix가 가지고 있는 장점중 하나인 edge를 코딩으로 나타낼수 있다는 것을 소개

## Keynote Part 3: Advanced Rendering Patterns

1. Static Rendering

- HTML이 build time에 생성된다.
- 쉽게 CDN으로 캐싱 가능하다.

- Plain Static Rendering
  ex) dynamic data를 포함하고 있지 않은 경우

- Static with Client-Side fetch
  ex) 매 페이지가 리프레시 되야만 하는 데이터를 포함하는 경우
  ex) stable placeholder components

- Incremental Static Regeneration
  ex) 특정한 간격이나 on-demand시에 다시 만들어져야 하는 경우

- On-demand Incremental Static Regeneration
  ex) 특정한 이벤트에 따라 다시 만들어져야 하는 경우

2. Server-Side Rendering

- 매우 사적인 콘텐츠를 포함한 경우
- 데이터 기반 요청을 사용하는 경우(ex. 쿠키)
- 렌더 블로킹이 되어야 하는 경우
