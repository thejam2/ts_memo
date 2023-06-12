# ts_memo
ts_memo

Typescript 타입 안정성 → 버그 감소

### 설치
`npm i -D typescript`

package.json 초기화

`npm init -y`

### 기본
`let a :string = "x" //변수명 옆 :타입명`

any: 아무 타입

undefined: 선언X 할당X

null: 선언O 할당X
```
//변수명 옆 ?는 undefined 가능(optional)
const player : { name: string, age?:number} = { name: "nico" }
```

### Alias(별칭) 타입
```
type Player = { name: string, age?:number }
const player : Player = { name: "nico" }
```

일반함수
```
type Player = { name: string, age?:number } 

//함수명(파라미터명:타입명) : 타입
function playerMaker1(name:string) : Player { return { name } } 
```
화살표함수
```
type Player = { name: string, age?:number } 

const playerMaker = (name: string): Player => ({name})
```
### readonly
```
//readonly 있으면 수정 안됨
type Player = { readonly name:string, age?:number }
```

### void
void는 값을 반환하지 않는 함수의 반환 값

### unknown
unknown타입은 모든 값.

any타입과 비슷하지만 any보다 unknown이 더 안전.

이유는 unknown값으로 작업을 수행하는 것은 합법적이지 않기 때문

### never
일부 함수는 값을 반환하지 않음.

이는 함수가 예외를 throw하거나 프로그램 실행을 종료함을 의미.

### ts함수 오버로딩
Function(=Method) Overloading은 직접 작성하기보다 외부 라이브러리에 자주 보이는 형태로, 하나의 함수가 복수의 Call Signature를 가질 때 발생한다.

```
type Config = { path: string, state: number }
type Push = { (config: Config): void, (config: string): void }
const push: Push = (config) => { 
  if (typeof config === "string") console.log(config);
  else console.log(config.path);
 }
```

매개변수의 데이터 타입이 다른 경우 예외 처리
```
type Add = { 
  (a: number, b: number): number, 
  (a: number, b: string): number 
}
const add: Add = (a, b) => {
  if (typeof b === "string") return a;
  return a + b; 
}
```

매개변수의 수가 다른 경우 예외 처리 이와 같은 함수는 거의 없지만 외부 라이브러리에서 활용될 수 있다
```
type Add2 = { 
  (a: number, b: number): number,
  (a: number, b: number, c: number): number 
}
const add2: Add2 = (a, b, c?: number) => { 
  //옵션인 c는 따로 타입 설정
  if (c) return a + b + c;
  return a + b;
}
```

### polymorphism(다형성)
concrete type -> number, boolean, void 등 지금까지 배운 타입

generic type -> 타입의 placeholder
```
type SuperPrint = { 
	<작명>(arr: 작명[]): 작명 	//〈T〉(arr: T[]): T 이런식
}

const superPrint: SuperPrint = (arr) => {
    arr.forEach(i => console.log(i))
}
const superReturn: SuperReturn = (arr) => arr[0]

superPrint([1, 2, false, true])
```

### 추상(abstract) 클래스
추상 클래스는 오직 다른 클래스가 상속받을 수 있는 클래스이다.

하지만 직접 새로운 인스턴스를 만들 수는 없다.

추상 메서드는 추상 클래스를 상속받는 클래스들이 반드시 구현(implement)해야하는 메서드이다.

### Type Aliases과 Interfaces의 차이점
Type Aliases과 인터페이스는 매우 유사하며 많은 경우 자유롭게 선택할 수 있습니다.

인터페이스의 거의 모든 기능은 type에서 사용할 수 있으며, 주요 차이점은 type을 다시 열어 새 속성을 추가할 수 없는 것입니다.

반면 인터페이스는 항상 확장 가능합니다.

결론: 대부분의 경우 개인 취향에 따라 선택 가능 (인터페이스 사용을 조금 더 추천)

### 상속
```
interface a {}
interface b extends a{}
type a = {}
type b = a & {}
```

### tsconfig.json
디렉터리에 tsconfig.json 파일이 있으면 해당 디렉터리가 TypeScript 프로젝트의 루트임을 나타냅니다. 

tsconfig.json 파일은 프로젝트를 컴파일하는 데 필요한 루트 파일과 컴파일러 옵션을 지정합니다.

https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#handbook-content

### Target (기본값: ES3)
최신 브라우저는 모든 ES6 기능을 지원하므로 ES6는 좋은 선택입니다.

코드가 이전 환경에 배포된 경우 더 낮은 target을 설정하거나 최신 환경에서 코드 실행이 보장되는 경우 더 높은 target을 설정하도록 선택할 수 있습니다.

ex) 화살표 함수() => this는 ES5 이하이면 함수 표현식으로 바뀝니다.
