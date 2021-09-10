# Programming TypeScript

## Introduction 
JavaScript에서 Type 체크를 하기 위해서는 명시적으로 `typeof`라는 함수를 사용해야 한다. 만약 명시적으로 타입 체크를 하지 않는 경우 잠재적으로 타입이 맞지 않아서 발생하게 되는 문제를 내재하게 된다. 개발하면서 타입 때문에 문제가 되는 일이 빈번할까 싶지만, 얼마나 JavaScript에서 Type이 명확하지 않아서 문제가 발생하는 경우가 많고 불편하면 TypeScript라는 언어가 나왔는지 싶다. 원래 Java로 개발해서 그런지 JavsScript의 타입에 대한 자유로움 때문에 JavaScript를 공부하기 힘들었다. 는 핑계를 대본다. 어쨌든 타입으로 인한 문제를 내포한 상태에서 런타임시 문제를 표출하는 JavaScript 보다는 TypeScript를 공부해야하는 상황도 오고 해서 다시 공부해보고자 한다.

TypeScript가 JavaScript의 가장 큰 차이점은 `Type safety` 이다. 즉, 타입을 명시하지 않아서 잠재된 문제로 인해 프로그램이 잘못 동작하는 것을 막아준다. 타입을 명시하지 않은 경우 불편함에 대해서는 일단 코드 가독성이 떨어지는것 같고, 이로 인해 코드를 명확하게 이해하지 못하는 경우 잘못된 타입간의 연산 또는 로직으로 인해 문제를 유발할 수 있다고 생각한다. 디버깅도 상대적으로 어려운것 같고, 개인적인 생각으로는 타입에 대한 자유로움으로 인한 장점 보다는 단점이 많은 것 같다.

아무튼 개인 공부에 손을 놓은 부분이 있는데 다시 필요한 것들을 공부해야겠다.


## TypeScript: A 10_000 Foot View
### Create new NPM project

#### Create a new folder
```typescript
mkdir chapter-2
cd chapter-2
```

#### Initialize a new NPM project (follow the prompts)
```typescript
npm init
```

#### Install TSC, TSLint, and type declarations for NodeJS
```typescript
npm install --save-dev typescript tslint @types/node
```

### tsconfig.json
모든 타입스크립트 프로젝트는 루트 디렉토리에 `tsconfig.json` 파일을 포함
* 타입스크립트가 컴파일 해야 하는 파일, 컴파일 해야 하는 디렉토리, 내보내야 하는 자바스크립트 버전을 명시

#### Create a new `tsconfig.json` file
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

### tslint.json
또한 타입스크립트 프로젝트에는 TSLint 설정을 포함하는 `tslint.json` 파일이 존재
* TSLint는 선택사항이지만, 코드 스타일에 대한 논쟁을 피하기 위해 권장

#### Generate `tslint.json` file
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


## All About Types
**Type**은 일련의 값과 그 값을 이용해 할 수 있는 일을 의미한다. 어떤 값의 Type을 알면, 값의 Type을 알게되는 것 뿐만 아니라 해당 Type으로 할 수 있는 연산에 대해서도 정확히 알 수 있다. 

### any
any는 모든 Type을 대체할 수 있다. any는 어떤 값이든 저장할 수 있으며, 프로그래머 또는 TypeScript가 어떤 Type인지 파악할 수 없을때 기본 Type이 된다. 가능한 애매모호함을 피해야하기때문에 사용을 지양해야 한다.
> TypeScript가 암시적 anys에 대해 검사하도록 하려면, tsconfig.json에서 noImplicitAny 플래그를 활성화해야 한다.

### unkown
any Type을 대체할 수 있는 값으로, Type을 미리 알 수 없는 값이 있는 경우에 활용한다. - `구체적으로 언제?`

### boolean
true 또는 false

### number
정수, 부동 소수점, 양수, 음수, 무한대, NaN 등 모든 숫자의 집합

### bigint
반올림 오류 없이 큰 정수로 작업할 수 있는 Type

### string
문자열

### symbol
많이 사용하지는 않는데, 사람들이 잘 알려진 올바른 키를 사용하고 있고 실수로 키를 설정하지 않았는지 확인하려는 경우 개체에 대한 기본 반복자를 설정하는 것을 고려할때 사용 - `어떻게 활용하는지?` 

### Objects
객체의 형태를 지정

### Arrays
연결(concatenation), 푸시(pushing), 검색(searching) 및 슬라이싱(slicing)과 같은 연산을 지원하는 특별한 객체
> TypeScript는 TypeScript는 `T[]` 및 `Array<T>`의 두 가지 배열 구문을 지원한다. 의미와 성능면에서 큰 차이가 없음으로 취향 차이다.

### Tuples
Array의 하위 Type으로, 각 인덱스 값에 알고있는 특정 Type이 있는 고정 길이 배열을 입력하는 방법
> 명시적으로 Type을 지정해야 한다.

### null, undefined, void and never
JavaScript에는 null과 undefined 두 가지 값이 있으며, TypeScript도 이를 지원
* null: 값이 없음
* undefined: 무언가가 아직 정의되지 않았음

추가로 TypeScript는 void 및 never을 지원
* void: 명시적으로 아무 것도 반환하지 않는 함수의 반환 Type
* never: 전혀 반환하지 않는 함수의 Type (예외를 발생시키거나, 영원히 실행되지 않는 함수)

### Enums
열거형 Type


## Functions

## Classes and Interfaces

## Advanced Types

## Handling Errors

## Asynchronous Programming, Concurrency, and Parallelism

## Frontend and Backend Frameworks

## Namespaces.Modules

## Interoperating with JavaScript

## Building and Running TypeScript

## Conclusion

