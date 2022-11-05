# Catch All Url
>[노마드 코더의 'Next.js 시작하기'](https://nomadcoders.co/nextjs-fundamentals)강의를 듣고 내용 정리

### 1. Catch All Url?
- Catch all url은 이름에서 알 수 있듯 url에 많은 정보를 담아도 그 정보들을 모두 잡아낼 수 있는 url이다.
- Dynamic Routes와 비슷하게 파일명에 대괄호와 함께 앞에 '...'을 붙여준다. (spread 문법과 비슷)
    - 예시: [...params].tsx
- 예시: url에 영화 제목을 넣는 경우
    - url에 영화 제목이 담겨 있어서 SEO에 유리하다.
    - 유저가 url 주소를 통해 들어오더라도 영화제목 정보를 url에서 받아올 수 있다.
- router의 query가 배열 형태로 들어오게 된다.
- 예시: '/movies/Terrifier 2/124/125/533'  
    => query = { params: [Terrifier 2, 124, 125, 533] }

### 2. 고려 사항
- Incongnito mode로 접근하는 경우
    - 따로 처리를 해주지 않으면 에러가 발생한다.
    - 에러가 발생하는 이유는 Next.js의 첫 화면은 pre-render되는데 서버에는 router.query.params가 배열로 존재하지 않기 때문이다.
    - catch all url은 결국 CSR환경에서만 동작한다는 것! (소스코드를 보면 CSR로 동작하는 것을 알 수 있음)
- 해결방법 1: OR 연산자 이용
    ```typescript
    const router = useRouter();

    // 다음과 같이 OR 연산자 뒤에 []를 붙여서 hydration되서 CSR이 동작하기 전에 에러가 뜨지 않게 해준다.
    const [title, id] = (router.query.params as string[]) || [];
    ```
- 해결방법 2: getServerSideProps
    - Next.js에서 context를 제공하는데 getServerSideProps함수의 첫번째 매개변수로 들어온다.(context안에 params 존재)
    ```typescript
    export const getServerSideProps:GetServerSideProps = async ({ params }) => {
        const { params: routerParams } = params as { params: string[] };

        return {
            props: {
            routerParams,
            },
        };
    };
    ```
    - 로딩 단계를 화면에 보여주지 않고 SEO를 최적화 할 수 있다.