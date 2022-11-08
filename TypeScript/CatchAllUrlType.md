# Next.js의 Catch All url에서 type

> Next.js의 Catch All url에서 TypeScript를 사용하면서 만난 문제를 해결하면서 학습했습니다.  
> Next.js 항목에 넣을까 했지만 TypeSciprt와 관련된 내용이 많아 TypeScript 항목에 넣었습니다.

### 1. 문제 발단

```typescript
import { useRouter } from "next/router";

export default function Detail() {
  // Type 'string | string[] | undefined' is not an array type.ts(2461) 에러 발생
  const {
    query: {
      params: [title, id],
    },
  } = useRouter();

  // ...
}
```

- Catch All url을 사용하여 url에 담긴 정보들을 params에 배열로 담어서 전달하고 있는 상황이다.
- 에러를 해석했을 땐 뭔가 이상하다고 생각했다. Next.js가 제공해준 type 정의에서는 아마도 'string', 'string[]', 'undefined'세가지가 있는 것 같은데 배열을 넣어주니까 에러가 발생한 것이다.
- 검색으로는 해결 못했지만 [Next.js의 GitHub](https://github.com/vercel/next.js/blob/canary/examples/catch-all-routes/pages/post/%5B...slug%5D.tsx)을 보고 답을 찾을 수 있었다.

### 2. 에러 추측 및 해결방안

- 아마도 타입이 불확실 하다보니 생기는 문제 같았다. 그래서 공식문서에서도 as로 타입은 더 구체적으로 해준 것 같다.  
  => **더 공부하고 알게된 것: 결국엔 세가지 타입이 가능한데 내가 배열로 단정하고 코드를 작성해서 타입스크립트에서 다른 타입이 올 경우 에러가 날 수 있다고 말해주는 것이었다!!**
- TypeScript의 [타입 단언](https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html#%ED%83%80%EC%9E%85-%EB%8B%A8%EC%96%B8)을 보면 타입을 좀 더 구체적으로 명시할 때 as를 사용해서 타입을 단언해준다고 한다.
- as를 사용하지 않고 구체적으로 타입을 지정해주려고 하면 Next.js에서 제공해주는 NextRouter라는 타입과 충돌이 일어나는 것 같았다.

### 3. 공식문서와 같이 as로 해결

```typescript
const router = useRouter();

const [title, id] = (router.query.params as string[]) || [];
```

- 결국엔 useRouter가 반환하는 객체의 타입은 최대한 그대로 두고 router.query.params의 타입만 as로 구체화 해줘서 에러를 해결하는 것 같다.

### 4. 추가 예시

```typescript
export const getServerSideProps: GetServerSideProps = async ({ params }) => {
  const { params: routerParams } = params as { params: string[] };

  return {
    props: {
      routerParams,
    },
  };
};
```

- getServerSideProps함수에서 params를 받아야하는 경우에도 똑같은 문제가 발생한다.
- 여기서도 Next.js에서 제공해주는 context의 타입은 그대로 두되 context.params객체 안의 타입만 as로 구체화 해줘서 에러를 해결할 수 있다.

### 5. 주의할 점

- 타입이 있는 언어는 사용해보지 않아서 잘 모르는 영역이지만 as를 사용하면 다운캐스팅이 발생할 수 있다고 한다.
- 결국 여러 타입이 가능한 union 타입을 구체적으로 함으로써 다른 타입이 되는 것을 아예 막아버리기 때문에 꼭 안전한 방법이라고는 할 수 없다고 한다.
- 위의 예시들 처럼 꼭 사용해야하는 경우가 아니면 지양하도록 해야겠다.
