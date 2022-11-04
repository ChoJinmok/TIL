# Move to detail page in Next.js
>[노마드 코더의 'Next.js 시작하기'](https://nomadcoders.co/nextjs-fundamentals)강의를 듣고 내용 정리

### 1. useRouter의 push 사용하기
- useRouter가 반환하는 객체에 push메서드가 있는데 push에 url을 인자로 넘겨주면 인자로 넣어준 주소로 이동한다.
- push의 매개변수 url은 string으로 전달해도되고 객체로 전달할 수 있다.
    - 객체로 넘겨줄 경우 pathname, query등을 넘겨줄 수 있다.
        ```typescript
            // 문자열로 전달
            router.push(`/movies/${id}`);

            // 객체로 전달
            router.push({
                pathname: `/movies/${id}`,
                query: {
                    title,
                    posterPath
                }
            })
            // movies/124142?title=potatos 주소로 이동
        ```
    - 객체로 넘겨준 정보는 이동한 페이지에서 useRouter로 받아올 수 있다.
    - 하지만 유저가 url을 직접 입력해서 접근하면 url 객체의 정보를 받을 수 없다.
- push의 두번째 매개변수 'as'를 이용하면 url을 마스킹해줄 수 있다.
    - 'as'를 사용해서 브라우저의 주소는 'as'로 바뀌지만 url의 정보는 넘겨줄 수 있다.
    - url이 지저분해지거나 유저에게 보여주고싶지 않은 url정보를 숨길 수 있다.
- 이렇게 정보를 담아서 보내주면 원래있는 데이터들을 활용할 수 있어서 서버와 불필요한 통신을 줄여줄 수 있다.

### 2. Link 컴포넌트
- Next.js에서 제공해주는 Link 컴포넌트로 위의 기능을 똑같이 구현할 수 있다.
    ```jsx
        <>
            <Link href={`/movies/${id}`}>{title}</Link>
            <Link
                href={{
                pathname: `/movies/${id}`,
                query: {
                    title,
                    posterPath,
                },
                }}
                as={`/movies/${id}`}
            >
                {title}
            </Link>
        </>
    ```

### 3. rewrites
- 상세 페이지에서 데이터를 받아오는 요청 주소가 드러나길 원하지 않는다면 작성해줄 수 있다.
- react-router-dom과 같이 변수는 ':'을 앞에 붙여주면 된다.
    ```typescript
        {
            source: '/api/movies/:id',
            destination: `https://api.themoviedb.org/3/movie/:id?api_key=${API_KEY}`,
        }
    ```