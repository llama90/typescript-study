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

### Declaring and Invoking Functions
JavaScript에서 함수는 일급 객체(first-class object)로, 다른 객체와 동일하게 사용할 수 있다. 변수에 할당할 수 있고, 다른 함수에 전달할 수 있고, 함수에서 반환할 수 있으며, 객체와 프로토타입에 할당하고, 속성을 쓰고, 해당 속성을 다시 쓸 수 있다. TypeScript는 이러한 일련의 작업을 Type 시스템으로 모델링한다.

TypeScript에서의 함수를 선언하는 방법
```TypeScript
// Named function
function greet(name: string) { 
  return 'hello ' + name
}

// Function expression
let greet2 = function(name: string) { 
  return 'hello ' + name
}

// Arrow function expression
let greet3 = (name: string) => { 
  return 'hello ' + name
}

// Shorthand arrow function expression
let greet4 = (name: string) => 
  'hello ' + name

// Function constructor
let greet5 = new Function('name', 'return "hello " + name')
```
* 모든 구문은 Type 안전성을 기반으로 선언
* 매개변수에 대한 필수 Type 애노테이션과 반환 Type에 대한 선택적 애노테이션에 대해 동일한 규칙을 따름
  * 매개변수(parameter): 함수 선언(function declaration)의 일부로 선언된 함수를 실행하는데 필요한 데이터, 형식 매개변수(*formal paramter*)라고도 함
  * 인수(argument): 호출할때 함수에 전달한 데이터, 실제 매개변수(*actual parameter*)라고도 함

> Function constructor 구문은 지양해야 한다. 매개변수 및 반환 유형이 지정되지 않기 때문에 TypeScript의 Type Safety 사상에 위배된다.

TypeScript에서 함수를 호출하는 방법  
함수를 호출할 때 추가 타입 정보를 제공할 필요가 없다. 일부 인수를 전달하면 TypeScript가 인수별로 함수 매개변수 타입과 매칭되는지 확인한다.

#### Optional and Default Parameters
`?` 애노테이션을 사용해 매개변수를 선택적(Optional)으로 표시할 수 있다. 함수의 매개변수를 선언할 때 필수(required) 매개변수가 먼저 와야 하고, 그 다음에 선택적 매개변수가 와야 한다. 선택적 매개변수에 기본값을 제공할 수 있으며, 기본값을 제공하는 경우 선택적 매개변수 애노테이션인 ? 및 타입을 입력하지 않아도 TypeScript가 기본 값을 바탕으로 매개변수의 타입을 추론한다. 반면에 명시적으로 타입 애노테이션을 추가할 수도 있다.

#### Rest Parameters
함수가 인수 목록을 취하는 경우 목록을 배열로 전달할수 있다. 경우에 따라 고정적인 개수의 인수를 사용하는 고정 인수(fixed-arity) API 대신, 가변적인 개수의 인수를 사용하는 가변 함수(variadic function) API를 사용할 필요가 있다. TypeScript는 가변 함수를 안전하게 입력하기 위해서 나머지 매개변수(`...`)를 사용한다. 함수는 최대 하나의 나머지 매개변수를 가질 수 있으며, 해당 매개변수는 함수의 매개변수 목록에서 마지막 매개변수여야 한다.

#### call, apply, and bind
괄호 `()` 를 사용하여 함수를 호출하는 것 외에도 두 가지 방법이 더 있다.

```typescript
function add(a: number, b: number): number { 
  return a + b
}

add(10, 20) // evaluates to 30
add.apply(null, [10, 20]) // evaluates to 30 
add.call(null, 10, 20) // evaluates to 30 
add.bind(null, 10, 20)() // evaluates to 30
```

* apply(): 함수 내에서 this에 값을 바인딩하고(예제에서는 null에 바인딩), 두 번째 인수를 함수의 매개변수에 퍼트린다(spread). 
* call(): apply와 전반적인 기능은 동일하지만 순서대로 인수를 함수의 매개변수에 적용한다.
* bind(): this 인수와 인수 목록을 함수에 바인딩하는 점에서 유사하나, bind가 함수를 호출하지 않는다. 대신 ()로 호출할 수 있는 새 함수를 반환한다.

#### Typing this
JavaScript에서는 this 매개변수가 클래스에서 메소드로 사용되는 함수뿐만 아니라, 모든 함수에 대해 정의되어 있다. this는 함수를 호출하는 방법에 따라 다른 값을 가질 수 있으며, 깨지기 쉽고 값을 추론하기 어렵게 만든다(여기서 값이 깨지기 쉽다는 것은 this 매개변수에 값을 할당하는 방법과 연관하여 언제 this 값에 할당되었던 값이 새로운 값으로 할당될지 파악하기 어려운 부분을 의미하는것 같다). TypeScript는 함수에서 this를 사용해야 하는 경우, 예상되는 this 타입을 함수의 첫번째 매개변수로 선언하도록 유도한다. 이를 통해 사용자가 함수에 this 매개변수를 의도에 맞도록 사용할 수 있게 한다.

> JavaScript의 this 매커니즘이 코드 작성시 값 할당에 대한 일관성을 해치고 불확실성을 수반하기 떄문에, this 매개변수를 사용해야하는 경우 자신이 의도한 범위를 벗어나는 것을 막기 위해 this 애노테이션을 지원한다고 이해

#### Generator Functions
많은 값을 생성하는 편리한 방법으로, Generator의 소비자가 값이 생성되는 속도를 제어할 수 있도록 한다. 이는 Generator가 소비자가 값을 필요로 할때 만들어주기 때문에 가능하다. `function*` 함수 뒤에 `*` 애노테이션을 이용해 Generator임을 선언하며 호출될때마다 Iterator가 반환된다. 값은 Generator 함수 내에 `yield` 키워드로 할당된 변수 값이 전달된다.

#### Iterators
Generator와 연관이 있으며, Generator는 값 스트림을 생성하는 방법인 반면, Iterator는 이렇게 생성된 값을 사용하는 방법이다. 
* Iterable: 값이 반복자를 반환하는 함수인 `Symbol.iterator`라는 속성을 포함하는 모든 객체
* Iterator: 속성 값이 있는 객체를 반환하고, 완료되는 next라는 메소드를 정의하는 객체

#### Call Signatures
전체 타입의 함수 자체를 표현하는 방법(C++에서 함수에 대한 선언부와 구현부를 나누는 것)을 의미한다. Call Signatures에는 타입 수준(Type Level) 코드만 포함한다. 여기서 타입 수준 코드는 타입과 타입 연산자로만 구성된 코드를 의미한다.

```typescript
type Log = (message: string, userId?: string) => void

let log: Log=(
  message,
  userId = 'Not signed in'
) => {
  let time = new Date().toISOString() 
  console.log(time, message, userId)
}
```
* log를 Call Signature를 이용해 선언
* Call Signature에 이미 매개변수에 대한 타입을 정의했기 때문에 log에 선언하면서 타입 애노테이션을 추가할 필요가 없다. TypeScript가 Log Call Signature를 기준으로 추론한다.

#### Contextual Typing
**Call Signatures** 예제에서 log를 Log 타입으로 선언했기 때문에 함수 매개변수에 타입에 대해 명시적으로 애노테이션을 추가하지 않아도 TypeScript가 스스로 log의 매개변수 타입을 추론하는 것을 의미한다.

#### Overloaded Function Types
Overloaded Function은 함수를 여러 Call Signature로 선언하는 것을 의미한다. 함수 오버로드는 같은 이름을 가진 함수에 대해 매개변수는 달리해서 같은 함수 이름을 갖지만 매개변수에 따라 호출되는 로직을 다르게 하고 싶을때 활용한다. JavaScript는 동적 언어이기 때문에 주어진 함수를 호출하는 여러가지 방법이 존재하고, TypeScript는 static type system을 사용해서 이와 같은 Overloaded Function을 모델링한다. 오버로드된 함수 선언 및 입력 타입에 따라 함수의 출력타입을 다르게해서 표현력이 풍부한 API를 디자인할 수 있다.

예를 들어 항공권을 예약하는 함수를 디자인하는 경우 출항일, 입항일 그리고 목적지를 입력받을 수 있을텐데, 왕복인 경우와 편도인 경우를 고려할 수 있다. 이를 위한 함수를 아래와 같이 Overloaded Function으로 디자인할 수 있다.

```typescript
type Reserve = {
  (from: Date, to: Date, destination: string): Reservation 
  (from: Date, destination: string): Reservation
}

let reserve: Reserve = (
  from: Date,
  toOrDestination: Date | string, 
  destination?: string
) => {
  if (toOrDestination instanceof Date && destination !== undefined) {
    // Book a one-way trip
  } else if (typeof toOrDestination === 'string') { 
    // Book a round trip
  } 
}
```

### Polymorphism
Generic Type Paramter에 대한 이야기로, 함수에 대한 구체적인 타입(Concrete Type)은 함수에서 사용할것이라 예상되는 타입을 정확히 알고, 해당 타입이 실제로 전달되는 경우에 유용하다. 하지만 때때로 타입에 따른 함수 동작을 제약하고 싶지 않을 수 있다. 예를 들어 Filter 함수를 개발하는 경우, function signature를 이용해 Filter 함수가 필요한 타입이 필요할때마다 추가하는 형태로 개발할 수도 있을것이다. 하지만 Object 타입의 경우, 객체의 구체적인 형태를 알지 못하기 때문에 에러가 발생 할 수 있다. 이럴때 Generic Type Parameter를 사용해서 TypeScript가 객체의 구체적인 타입을 추론하도록 유도할 수 있다. 
* Generic Type Parameter: 타입 수준 제약 조건을 적용하는데 사용되는 Placeholder, 다형성 타입 매개변수라고도 부른다.

```typescript
type Filter = {
  <T>(array: T[], f: (item: T) => boolean): T[]
}
```
- Generic Type Parameter T를 사용하는 Filter 함수

TypeScript는 배열에 대해 전달할 타입에서 T를 유추한다. 유추를 통해 모든 T에 대해 해당 타입으로 대체한다. T는 context에서 typechecker에 의해 채워지는 placeholder 타입과 같다. Filter의 타입을 매개변수화 하므로 이를 Generic Type Parameter라 한다. `< >`는 Generic Type Parameter를 선언하는 방법이다. 배치 위치는 Generic 범위로, TypeScript는 해당 범위 내에서 Generic Type Parameter의 모든 인스턴스가 결국 동일한 구체적인 타입에 바인딩 되는지 확인한다. 그리고 Filter를 어떤 타입으로 호출했는지에 따라 T에 바인드할 구체적인 유형을 결정한다. 한 쌍의 `< >` 사이에 원하는 만큼 쉼표로 구분된 Generic Type Parameter를 선언할 수 있다.

```typescript
filter([1, 2, 3], _ => _ > 2)
```
TypeScript가 T를 바인딩하는 방법
1. 필터의 타입 signature에서 TypeScript는 배열이 T 유형의 element 배열임을 알 수 있다.
2. TypeScript는 배열 [1, 2, 3]을 전달했으므로 T는 숫자임을 알 수 있다.
3. TypeScript가 T를 만날때마다 number 타입으로 대체한다.
  * 매개변수 `f: (item: T) => boolean`는 `f: (item: number) => boolean`이 된다.
  * 반환타입 `T[]`는 `number[]`가 된다.
4. TypeScript는 모든 타입이 할당 가능성(assignability)을 충족하는지, f로 전달한 함수가 새로 추론된 signature에 할당 가능한지 확인한다.

Generic은 구체적인 타입이 허용하는 것보다 더 일반적인 방식으로 함수가 수행하는 작업을 처리한다. Generic에 대해서 생각하는 방식은 제약조건(constraints)로, 함수 매개변수에 `n: number`를 애노테이션으로 달아, 매개변수 n의 값이 `number` 타입으로 제한되어 Generic T를 사용하면 T에 바인딩되는 타입의 타입 T가 표시되는 모든 줄에서 동일한 타입으로 제한된다.

> Generic을 사용하면 코드가 일반적이고, 재사용 가능하며, 간결하게 유지된다.

### Type-Driven Development
먼저 Type Signature를 고려하고 Value를 나중에 채워나가는 프로그래밍 Style
> Sketch out type signature first, and fill in vlaues later

TypeScript 프로그램을 작성할 때, 먼저 함수 Type Signature를 정의한다. 먼저 타입 수준에서 프로그램을 스케치하고, 구현에 이르기 전에 모든 것을 높은 수준에서 프로그램을 스케치하고, 구현에 이르기 전에 모든 것이 높은 수준에서 이해되는지 확인한다.

```typescript
function map<T, U>(array: T[], f: (item: T) => U): U[] { 
  // ...
}
```

map을 본적이 없더라도, signature 만으로 map이 처리하는 일에 대한 직관이 있어야 한다. 
> T의 배열과 U로 매핑하는 함수를사용하고, U의 배열을 반환한다.


## Classes and Interfaces
TypeScript는 객체 지향 언어에서 지원하는 다양한 언어적 특성을 제공한다. 클래스, 인터페이스, 추상화, 생성자, 상속, 구현등 객체지향 언어가 가지고 있는 특성은 모두 포함하고 있다(문법에 익숙해지려면 일단 개발을 많이 해봐야할것 같다).

> `keyword`: class, access modifier(private, protected, public), abstract class, constructor, extends, super(), interface, implements

그 중에 신기한건 `Type` 키워드가 `Interface` 키워드랑 호환되고, 같은 이름을 갖는 타입을 여러개로 나눠서 선언하여 하나로 사용 가능한 부분이 흥미롭다. 

## Advanced Types

### Relationship Between Types
`Subtype`(하위 타입): A와 B 타입이 있을 때, B가 A의 Subtype인 경우, A가 필요한 모든 곳에서 B를 안전하게 사용할 수 있다. `vs.` `Supertype`(상위 타입): 반대로 B가 A의 Supertype인 경우, B가 필요한 모든 곳에서 A를 안전하게 사용할 수 있다.
* A <: B A는 B의 Subtype 또는 B와 동일한 타입
* B <: A B는 A의 Subtype 또는 A와 동일한 타입

TypeScript는 타입이 예상 타입의 하위 타입인 경우는 허용하지만, 예상 타입의 상위 타입인 경우는 허용하지 않는다. 타입에 대해 이야기할때 TypeScript의 모양(Object 또는 Class)는 속성 타입에 `covariant` 하다고 한다. 즉, Object A를 Object B에 할당할 수 있으려면, 각 속성(property)이 <: B를 충족하는 속성이어야 한다. 

let 또는 var를 이용해 최초 선언 이후 변경할 수 있는 형태로 변수를 할당하면, 해당 타입은 리터럴 값에서 리터럴이 속한 기본 타입으로 확장된다. `vs.` 반면, const를 이용한 불변(immutable) 선언은 그렇지 않다. let 또는 var를 사용하여 확장되지 않은 타입을 재할당하면 TypeScript가 확장한다.

TypeScript에 범위를 좁히도록 지시하려면 원래 선언에 명시적 타입 주석을 추가한다.
```TypeScript
const a = 'x' // 'x'
let b = a // string

const c: 'x' = 'x' // 'x'
let d = c  // 'x'
```

null 또는 undefined로 선언된 변수는 any로 확장되며, 해당 타입으로 선언된 변수들은 선언된 범위를 벗어나면 TypeScript에 의해 해당 변수에 할당된 값으로 타입이 할당된다. 

TypeScript가 타입을 좁게 추론하도록 하려면 const로 할당한다.

TypeScript는 Object에 선언된 속성 이름값에 대한 체크도 수행한다.
```TypeScript
type Configuration = {
  accessKey: string
  accessSecretKey: string
  env?: 'prod' | 'dev'
}

class Setting({
  constructor(private configuration: Configuration) {}
})

new Setting({
  accessKey: "accessKey",
  accessSscretKey: "accessSecretKey" // accessSscretKey에 대해서 Configuration내에 없는 속성 값이라는 에러 메시지가 발생
})
```

TypeScript의 Typechecker는 프로그래머가 코드를 읽는 것처럼 if, ?, || 및 switch와 같은 제어 흐름문(control flow statements)과 typeof, instanceof 및 in과 같은 타입 쿼리(type queries)를 사용하여 타입을 구체화하는 일종의 기호 실행(symbolic execution)인 흐름 기반 타입 추론(flow based type inference)를 실행한다.
* 기호 실행: 기호 평가기(symbolic evaluator)라는 특수 프로그램을 사용하여 런타임과 동일한 방식으로 프로그램을 실행하지만, 변수에 명확한 값을 할당하지 않는 프로그램 분석 형태다. 대신 각 변수는 프로그램이 실행될 때 값이 제한된 기호로 모델링된다. 기호 실행을 사용하면 다음과 같은 분석이 가능하다.
  * 이 변수는 사용되지 않습니다.
  * 이 함수는 절대 반환되지 않습니다.
  * 12번째 라인에 있는 if문의 분기에서 변수 x는 null이 아님이 보장됩니다.
* 흐름 기반 타입 추론: TypeScript, Flow, Kotlin 및 Ceylon 등과 같은 소수 언어에서 지원되는 특징으로, 코드 블록 내에서 타입을 구체화하는 방법으로 프로그램을 통해 타입 검사기와 추론에 피드백을 제공하기 위해 기호 실행 엔진(symbolic execution engine)을 가져와 타입검사기에 삽입한다.

### Advanced Object Types
Object는 JavaScript의 핵심이며, TypeScript는 객체를 안전하게 표현하고 조작할 수 있는 다양한 방법을 제공한다.

#### Type Operators for Object Types
* **The keying-in operator** JavaScript 객체에서 필드를 조회하는 방법과 유사하다. 대괄호 표기법을 사용
* **The keyof operator** keyof를 사용해서 모든 객체의 모든 키를 문자열 리터럴 타입으로 반환

keying-in 및 keyof 연산자를 결합하여 Object의 지정된 키에서 값을 조회하는 typesafe getter 함수를 구현할 수 있다.
```TypeScript
function get<
  O extends object, 
  K extends keyof O
>(
  o: O,
  k:K 
): O[K] {
  return o[k] 
}
```

### Advanced Function Types
* **Improving Type Inference for Tuples** 코드에서 많은 튜플 타입을 사용하는 경우, type assersions을 피하기 위해 활용
* **User-Defined Type Guards** Type Guard는 TypeScript 내장 기능으로 typeof 및 instanceof로 타입을 구체화
  * is 연산자를 사용해 매개변수의 타입을 구체화할 수 있으며, 이때 is 연산자는 단일 매개변수로 제한된다.

### Conditional Types
조건부 타입은 `U 및 V 타입에 의존하는 유형 T를 선언하고, U <: V이면 T를 A에 할당하고, 그렇지 않으면 T를 B에 할당한다`.

## Handling Errors

## Asynchronous Programming, Concurrency, and Parallelism

## Frontend and Backend Frameworks

## Namespaces.Modules

## Interoperating with JavaScript

## Building and Running TypeScript

## Conclusion

