# ts_memo
ts_memo

Typescript 타입 안정성 → 버그 감소

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
