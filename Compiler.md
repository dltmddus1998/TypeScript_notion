# 12. 타입스크립트 컴파일러

## TSConfig Setup

---

✔︎ `**html`파일에서는 `.ts`를 바로 가져올 수 없으므로, 타입스크립트 컴파일러를 이용하여 .js로 컴파일해주는 과정이 필요하다.**

✔︎ **이를 위해, `tsc --init`을 터미널에 입력하면 `tsconfig.json` 파일이 생성된다.**

**✔︎ 이후, `tsc -w`를 통해 `tsconfig.json`파일이 있는 프로젝트 폴더 안에 있는 모든 .ts 파일들을 .js로 변환해준다.** 

## 프로젝트 구조 정리

---

### tsconfig.json

- outDir - 컴파일된 파일들을 어디에 지정할건지 설정
    - 기본 경로: 현재 폴더
    `"outDir": "./build",`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab0ac63d-e103-4378-8013-2d7cd5c9bab6/Untitled.png)

- rootDir - 루트 디렉토리 설정

<aside>
💡 **`tsconfig.json`에서 `include`, `exclude`를 통해 원하는 파일만 포함하거나 포함하지 않은 채 컴파일할 수 있다.**

</aside>

## Compiler Options

---

- incremental - 수정된 부분만 컴파일
- composite - incremental과 함께 쓰임 → 이전에 빌드된 부분을 기억해서, 다음에 빌드할 때 빠르게 빌드할 수 있도록 도와줌.
- tsBuildInfoFile - incremental이 true면 관련된 정보들을 담을 수 있는 파일을 지정함
- allowJs - 프로젝트 안에서 .js를 같이 쓸건지 설정
- checkJs - .js에서 오류가 있다면 에러가 뜰 수 있도록 설정
- noEmit - 컴파일 에러 체크만 하고, 실제로 .js 코드로 변환하지 않음
- sourceMap - 디버깅시 유용하게 사용

[TSConfig Reference - Docs on every TSConfig option](https://www.typescriptlang.org/tsconfig)

<br />
<br />

## How to Debug?

---

✔︎ **`tsconfig.json`에서 `“sourceMap”: true` 설정**

`.map` → 우리가 작성한 `.ts` 코드 & 컴파일된 `.js` 코드 연결시켜주는 모든 정보가 담긴 파일

**이를 통해 크롬 브라우저에서 디버깅이 가능해진다.**