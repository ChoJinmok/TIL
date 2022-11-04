# Dynamic Routes in Next.js
>[노마드 코더의 'Next.js 시작하기'](https://nomadcoders.co/nextjs-fundamentals)강의를 듣고 내용 정리

### 1. Next.js에서 url주소는 파일 구조로 결정된다.
- http://localhost:3000/movies
    - pages 디렉토리(폴더)안에 movies.tsx 파일을 생성 후 컴포넌트를 export default해주면 위의 주소로 이동시 export default한 컴포넌트가 브라우저에 렌더링 된다.
- http://localhost:3000/movies/all 처럼 더 깊이 들어가는 경우는?
    - pages > movies > all.tsx 와 같은 구조로 파일을 생성 후에 앞서 말한 방법처럼 컴포넌트를 작성해주면된다.
- 그렇다면 폴더 구조가 깊이 들어갔을 경우 http://localhost:3000/movies는?
    - pages > movies > index.tsx 와 같은 구조로 작성해주면된다. (페이지가 하나밖에 없는 경우 파일만 만들어줘도 된다.)

### 2. Dynamic Routes
- 동적 라우팅은 http://localhost:3000/movies/12와 같이 어떤 movie의 id가 와도 똑같은 페이지를 보여줘야할 경우이다.(react-router-dom의 경우 http://localhost:3000/movies/:id)
- 동적 라우팅의 경우도 디렉토리 구조로 작성해주면 되는데 파일명이 중요하다.
    - pages > movies > [id].tsx와 같은 구조로 작성해주면된다.
    - 여기서 id는 마음대로 작성해줘도 되지만 후에 useRouter가 리턴한 객체의 qeury 프로퍼티 객체 안의 key값으로 정해진다.
        ```JavaScript
        const router = useRouter();

        // router.query.(위에서 파일명의 대괄호 안에 작성한 명칭)으로 접근 가능
        // id로 작성한 경우 => router.query.id 로 접근
        ```
        