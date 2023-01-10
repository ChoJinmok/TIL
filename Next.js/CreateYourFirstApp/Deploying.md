# Deploying Your Next.js App

> Next.js 공식문서 CREATE YOUR FIRST APP 중 [Dynamic Routes](https://nextjs.org/learn/basics/dynamic-routes)내용들을 정리했습니다.

<br />

Next.js 앱을 프로덕션에 배포해보겠다.

Next.js 개발자가 만든 플랫폼인 [Vercel](https://vercel.com/?utm_source=next-site&utm_medium=learnpages&utm_campaign=next-website)에 Next.js를 배포하는 방법을 배울 것이고, 다른 배포 옵션에 대해서도 이야기해보겠다.

<br />

## 1. Push to GitHub

배포하기 전 Next.js 앱을 [GitHub](https://github.com/vercel/next.js)에 푸시해서 배포를 쉽게 할 수 있다.

- 개인 GitHub 계정에서 새 리포지토리를 만든다.
- 리포지토리는 공개 또는 비공개이어도 되며 README 또는 기타 파일 등 초기화할 필요 없다.
- 리포지토리 설정이 어려운 경우 [GitHub 가이드](https://help.github.com/en/github/getting-started-with-github/create-a-repo)를 참조

그런 다음:

- Next.js 앱에 대 git 저장소를 로컬로 초기화한다.
- Next.js 앱을 GitHub 리포지토리로 푸시한다.

GitHub에 푸시하려면 다음 명령을 실행할 수 있다(`<username>`을 GitHub 사용자 이름으로 대체).

```console
git remote add origin https://github.com/<username>/nextjs-blog.git
git push -u origin main
```

<br />

## 2. Deploy to Vercel

Next.js를 프로덕션 환경에 배포하는 가장 쉬운 방법은 Next.js 제작자가 개발한 [Vercel](https://vercel.com/dashboard?utm_source=next-site&utm_medium=learnpages&utm_campaign=next-website) 플랫폼을 사용하는 것이다.

Vercel은 헤드리스 콘텐츠, 커머스 또는 데이터베이스와 통합하도록 구축된 정적 및 하이브리드 애플리케이션을 위한 서버리스 플랫폼이다. Vercel은 성능은 물론 프론트엔드 팀의 즐겁고 쉬운 개발 경험에 미리보기 까지 제공한다. 무료로 사용할 수 있다.

### 2.1. Create a Vercel Account

먼저 https://vercel.com/signup으로 이동하여 Vercel 계정을 만든다. **Continue with GitHub**를 선택하고 가입 절차를 진행한다.

### 2.2. Import your repository

가입했으면 Vercel에서 리포지토리를 가져온다: https://vercel.com/import/git.

- **GitHub용 Vercel을 설치**해야 한다. **모든 리포지토리**에 대한 액세스 권한을 부여할 수 있다.
- Vercel을 설치했으면 리포지토리를 가져온다.

다음 설정에 대해 기본값을 사용할 수 있다. 아무 것도 변경할 필요가 없다. Vercel은 사용자에게 Next.js 앱이 있음을 자동으로 감지하고 최적의 빌드 설정을 한다.

- Project Name
- Root Directory
- Build Command
- Output Directory
- Development Command

배포하면 Next.js 앱 빌드가 시작된다.

> 도움말 사용 가능: 배포에 실패하면 언제든지 [GitHub Discussions](https://github.com/vercel/next.js/discussions)에서 도움을 받을 수 있다. 배포에 대해 자세히 알아보려면 [문서](https://nextjs.org/docs/deployment)를 살펴봐라.

완료되면 **배포 URL**을 받게 된다. URL 중 하나를 클릭하면 Next.js 시작 페이지가 라이브로 표시된다.

<br />

## 3. Next.js and Vercel

[Vercel](https://vercel.com/dashboard?utm_source=next-site&utm_medium=learnpages&utm_campaign=next-website)은 Next.js의 제작자가 만들고 Next.js를 최고 수준으로 지원한다:

- [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) 및 assets(JS, CSS, 이미지, 글꼴 등)을 사용하는 페이지는 [Vercel Edge Network](https://vercel.com/docs/concepts/edge-network/overview)에서 자동으로 제공된다.
- [서버 사이드 렌더링](https://nextjs.org/docs/basic-features/pages#server-side-rendering) 및 [API Routes](https://nextjs.org/docs/api-routes/introduction)를 사용하는 페이지는 자동으로 격리된 [서버리스 함수](https://vercel.com/docs/concepts/functions/serverless-functions?utm_source=next-site&utm_medium=learnpages&utm_campaign=next-website)가 ​된다. 이를 통해 페이지 렌더링 및 API 요청을 무한대로 확장할 수 있다.

Vercel에는 다음과 같은 더 많은 기능이 있다:

- **사용자 지정 도메인**: Vercel에 배포되면 사용자 지정 도메인을 Next.js 앱에 할당할 수 있다. [문서](https://vercel.com/docs/concepts/projects/domains?utm_source=next-site&utm_medium=learnpages&utm_campaign=next-website)를 살펴보기.
- **환경 변수**: Vercel에서 환경 변수를 설정할 수 있다. [문서](https://vercel.com/docs/concepts/deployments/configure-a-build#environment-variables?utm_source=next-site&utm_medium=learnpages&utm_campaign=next-website)를 살펴보기. 그런 다음 Next.js 앱에서 해당 [환경 변수를 사용](https://nextjs.org/docs/basic-features/environment-variables#loading-environment-variables)할 수 있다.
- **Automatic HTTPS**: HTTPS는 기본적으로 활성화되어 있으며(사용자 지정 도메인 포함) 추가 설정이 필요하지 않다. SSL 인증서를 자동 갱신한다.

[Vercel 문서](https://vercel.com/docs?utm_source=next-site&utm_medium=learnpages&utm_campaign=next-website)에서 플랫폼에 대해 자세히 알아볼 수 있다.

### 3.1. Preview Deployment for Every Push

> 아래 단계는 선택 사항이다.

Vercel에 배포한 후 가능하면 다음을 수행:

- 앱에 새 **branch**를 만든다.
- 몇 가지 사항을 변경하고 GitHub에 푸시한다.
- 새 **pull request**을 생성한다([GitHub 도움말 페이지](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)).

pull request 페이지에서 `vercel` 봇의 댓글을 볼 수 있다.

![vercel-bot](./images/vercel-bot.png)

이 댓글 안에 있는 **미리보기** URL에 방문하면 방금 변경한 내용이 표시된다.

pull request이 열려 있으면 Vercel은 푸시할 때마다 해당 branch에 대한 **미리보기 배포**를 자동으로 생성한다. 미리보기 URL은 항상 최신 미리보기 배포를 가리킨다.

이 미리보기 URL을 공동 작업자와 공유하고 즉각적인 피드백을 받을 수 있다.

미리 보기 배포가 양호하면 **`main`에 병합**한다. 이렇게 하면 Vercel이 프로덕션 배포를 자동으로 생성한다.

### 3.2. Develop, Preview, Ship

Vercel에서는 이 workflow를 **DPS**라고 한다: **D**evelop(개발), **P**review(미리 보기) 및 **S**hip(배송)

- **Develop**: Next.js에 코드를 작성하고 Hot Reloading 기능을 활용하기 위해 Next.js 개발 서버를 사용한다.
- **Preview**: GitHub의 branch에 대한 변경 사항을 푸시하고 Vercel은 URL을 통해 접근할 수 있는 미리보기 배포를 만든다. 피드백을 위해 이 미리보기 URL을 다른 사람들과 공유할 수 있다. 코드 검토 외에도 배포 미리 보기를 검토할 수 있다.
- **Ship**: pull request를 `main`으로 병합하여 프로덕션으로 배송한다.

Next.js 앱을 개발할 때 이 워크플로를 활용하는 것이 좋다. 앱을 더 빠르게 반복하는 데 도움이 된다.
