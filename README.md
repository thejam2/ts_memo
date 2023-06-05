# ts_memo
ts_memo

Typescript 타입 안정성 → 버그 감소

### 기본
`let a :string = "x" //변수명 옆 :타입명`

any: 아무 타입

undefined: 선언X 할당X

null: 선언O 할당X

`const player : { name: string, age?:number //변수명 옆 ?는 undefined 가능(optional)} = { name: "nico" }`

### Alias(별칭) 타입
```
type Player = { name: string, age?:number }
const player : Player = { name: "nico" }
```
