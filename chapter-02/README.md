# Create new NPM project

### Create a new folder
```typescript
mkdir chapter-2
cd chapter-2
```

### Initialize a new NPM project (follow the prompts)
```typescript
npm init
```

### Install TSC, TSLint, and type declarations for NodeJS
```typescript
npm install --save-dev typescript tslint @types/node
```

## tsconfig.json
모든 타입스크립트 프로젝트는 루트 디렉토리에 `tsconfig.json` 파일을 포함
* 타입스크립트가 컴파일 해야 하는 파일, 컴파일 해야 하는 디렉토리, 내보내야 하는 자바스크립트 버전을 명시

### Create a new `tsconfig.json` file
```typescript
vim tsconfig.json
```

tsconfig.json
```json
{
  "compilerOptions": {
    "lib": ["es2015"], 
    "module": "commonjs", 
    "outDir": "dist", 
    "sourceMap": true, 
    "strict": true, 
    "target": "es2015"
  }, "include": [
    "src"
  ]
}
```
* lib: 타입스크립트 컴파일러(TSC)가 코드를 실행하는데 있어 어떤 라이브러리를 포함하는지 명시
  * 예) ES5의 `Function.prototype.bind`, ES2015의 `Object.assign` 및 DOM의 `document.querySelector` 등
* module: TSC가 컴파일해야 하는 모듈 시스템을 명시
  * 예) `CommonJS`, `SystemJS`, `ES2015` 등
* outDir: TSC가 컴파일 후 생성된 자바스크립트 코드를 생성할 폴더를 명시
* strict: 모든 코드에 대한 타입을 엄격하게 정의되도록 설정
* target: TSC가 코드를 컴파일하는 자바스크립트 버전
  * 예) `ES3`, `ES5`, `ES2015`, `ES2016` 등
* include: TSC가 타입스크립트 파일을 찾기 위해 확인해야 하는 폴더

## tslint.json
또한 타입스크립트 프로젝트에는 TSLint 설정을 포함하는 `tslint.json` 파일이 존재
* TSLint는 선택사항이지만, 코드 스타일에 대한 논쟁을 피하기 위해 권장

### Generate `tslint.json` file
```typescript
./node_modules/.bin/tslint --init
```

아래와 같이 자신의 코딩스타일에 맞게 재정의 가능
* 가능한 모든 규칙을 살펴보려면 [TSLint documentation](https://palantir.github.io/tslint/rules/) 참고
* 사용자 정의 규칙을 추가도 가능

```json
{
  "defaultSeverity": "error", 
  "extends": [
    "tslint:recommended"
  ], 
  "rules": {
    "semicolon": false,
    "trailing-comma": false
  }
}
```