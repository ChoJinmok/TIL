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
