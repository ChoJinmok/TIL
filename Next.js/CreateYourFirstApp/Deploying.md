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
