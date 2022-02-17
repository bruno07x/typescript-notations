# [Introduction ao Typescript - Willian Justen](https://www.youtube.com/watch?v=PMhd1ebCGl8&list=PLlAbYrWSYTiPanrzauGa7vMuve7_vnXG_&index=11)

Superset do javascript
  - Nova feature ao javascript
  - Basicamente, tipagem estática!

## Types

### **Boolean (true/false)**
````typescript
let isOpen:boolean
isOpen = true
````

### **String ('John Doe')**
````typescript
let name:string
name = 'Bruno Couto'
````

### **Number (int, float, hex, binary)**
````typescript
let age:number
age = 27
````

### **Array (type[ ])**
````typescript
let items: string[]
items = ['Bruno Couto', 'John Doe']

let values: Array<number>
values = [1, 2, 5]
````

### **Tuple - Array com tamanho fixo**
````typescript
let fixedData: [number, string]
fixedData = [27, 'John Doe']
````

### **Object - Qualquer coisa que não seja um tipo primitivo (Boolean, String, Number)**
````typescript
let cart: object
cart = {
   key: 'Foo',
}
````

### **Enum - Conjunto de chave e valor**
````typescript
enum Colors{
  white = '#FFF',
  black = '#000'
}
````

### **Any - Qualquer coisa, não é legal**
````typescript
let thing: any
thing = 'Coisa'
thing = 21
thing = [23, 'Bruno']
````

### **Void - Usado para typar funções que não tem retorno**
````typescript
function voidFunction():void {
  console.log('Foo')
}
````

### **Null | Undefined - types alias**
````typescript
type Bla = string | undefined
````

### **Never - Nunca vai ter retorno**
````typescript
function error():never{
  throw new Error('Error')
}
````

---

## Types Inference
Já entende o tipo de uma variável se ela tiver um tipo primitivo como valor.
OBS: Não é possível definir tuplas na interface.

`````typescript
 let myName = 'Bruno Couto' //String
 myName = 'John Doe'

// Error de typescript
myName = 23;
`````

---

## Union Types
Eventualmente haverá necessidade de definir um tipo como sendo de um tipo primitivo ou outro. Então usamos ' | ' para dizer que é um ou outro.

````typescript
function logInfo(data: string | number){
  console.log(`Este dado é: ${data}`)
}
````

---

## Type Alias
Apelido para uma determinada interface. Evita repetição de cód. pode ser usado com tipos primitivos e objetos.

`````typescript
type Uid = number | string | undefined
`````

---

## Type Alias com Intersection
União de dois type alias.

`````typescript
type CharInfo = {
  nickName: string,
  level: number
}

type AccountInfo = {
  id?: number,
  name: string,
  email: string
}
// Intersection
type PlayerInfo = AccountInfo & CharInfo

const player:PlayerInfo = {
  name: 'Bruno ',
  id: 27,
  email: 'brunnohcouto@gmail.com',
  nickname: 'bruno.couto',
  level: 50
}
`````

---

## Interfaces
Conjuntos de dados que descreve a estrutura de um objeto (se difere de um type alias, pois só pode ser usado em objetos)

````typescript
interface IGame{
  title: string
  platform: string[]
  getSimilars?: (title: string) => void
}

interface IDLC extends IGame{
  desc: string
}

const pokemonLegend: IGame = {
  title: 'Pokémon Legend',
  platform: ['Nintendo Switch'],
  getSimilars: (title:string) => {
    console.log(`Games similars to ${title}...`)
  }
}

const pokemonAurora: IDLC = {
  title: 'Pokémon Legend',
  platform: ['Nintendo Switch'],
  desc: 'DLC nova do jogo.'
}
````

---

## Generics
Possibilita certa flexibilidade na hora de typar. Defini um tipo de genérico que após ser atribuido não poderá mudar.

````typescript
// Convenção de letras
// S => State
// T => Type
// K => Key
// V => Value
// E => Element
// useState<S extends number | string>
function useState<S = string>() {
  let state: S

  const getState = () => state
  const setState = (newState: S) => state = newState

  return { getState , setState}
}
// Após definir o valor de generic a função só aceitará aquele tipo.
const newState = useState<number>()
newState.setState(456)
console.log(newState.getState()) // Output 456
````

---

## Type Utilities
Utilitários que são baseados em generics types para executar determinadas ações.

````typescript
interface ITodo {
  title: string
  desc?: string
  completed: boolean
}

// Readonly -> Previne que os valores após criados não sejam alterados.
// EX: task.title = 'Outra tarefa' //Retornará erro.
const task: Readonly<ITodo> = {
  title: 'Lavar louça',
  completed: false
}

// Partial -> Deixa todas as propriedades de uma interface opcionais.
const updateTask = (todo: ITodo, fieldsToUpdate: Partial<ITodo>) => {...todo, ...fieldsToUpdate}

const task2: ITodo = updateTask(task, {completed: true})

// Pick -> Pega determinadas propriedades de uma interface
type TTodoPreviewPick = Pick<ITodo, 'title' | 'desc'>

// Omit -> Pega todas propriedades de uma interface exceto a propriedade informada
type TTodoPreviewOmit = Omit<ITodo, 'desc'>
````

---

## Classes

````typescript
class UserAccount {
  name: string
  age: number

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  logDetails():void {
    console.log(`The player ${this.name} is ${this.age} years old.`)
  }
}

class CharAccount extends UserAccount {
  nickname: string
  level: number

  constructor(name: string, age: number, nickname: string, level: number ) {
    super(name, age)

    this.nickname = nickname
    this.level = level
  }
}
````