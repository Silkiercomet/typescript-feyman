# typescript

typescript tiene una relacion inusual con javascript, ya que ofrece todos las caracteristicas de javascript pero con seguridad de datos, javascript te permite asignar valores primitivos como un string o un numero, pero estos valores pueden ser mutados en otros de distinta naturaleza de manera involuntaria, aqui es donde entra typescript en juego, se asegura de que todos los valores usados sean iguales en naturaleza a los declarados durante la inicializacion de la variable, es como un profesor que esta constantemente siguiendo tu codigo y te avisa cuando cometiste un error

## explicit declaration

durante la declaracion de una variable javascript analiza el contenido de esta para determinar la naturaleza de la variable, por eso, javascript es capaz de seguir cuando es imposible usar un metodo apra arreglos en un numeroo parsear un objeto a integer, pero esto es dependiente del valor de la variable de tal forma que si llega a mutar su tipo tambien lo hara, typescriot cambia esto definiendo la naturaleza de la variable es base al nombre de la variable y no a su contenido

```ts

// durante la inicializacion se puede declarar la naturaleza de la variable

const unaCadena : string = "hola mundo"

// se pueden usar interfaces para declarar la estructura y al naturaleza de las entradas de un objeto

interface User {
  name: string;
  id: number;
}

const user : User = {
  name: "Hayes",
  id: 0,
};

//-----------alternativa-----------//
// si el objeto declarado no coincide con la estructura de la interface ys te avisara
const user: User = {
  username: "Hayes",
/*Type '{ username: string; id: number; }' is not assignable to type 'User'.
  Object literal may only specify known properties, and 'username' does not exist in type 'User'.*/
  id: 0,
};
```

### explicit declaration in OOP

ya que javascript soporta OOP tambien typescript con la siguiente sintaxis donde las variable son declaradas dentro de la clase constructora y en la variable en la que se inicializa con el uso de una interface

```ts
interface User {
  name: string;
  id: number;
}
 
class UserAccount {
  name: string;
  id: number;
 
  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}
 
const user: User = new UserAccount("Murphy", 1);
```

### explicit declaration in FP

en la programacion funcional podemos definir el typo de un parametro, la estructura o natraleza del resultado que devolvera una funcion 

```ts
// devolvera un objeto con la estructura User
function getAdminUser(): User {
  //...
}

// esta funcion no devuelve nada
function getAdminUser(): void {
  //...
}

// tomara un parametro de tipo string
function deleteUser(user: User) {
  // ...
}
```

### Unions & generics

#### uniones

las uniones se sulen utilizar para declarar variables de naturaleza compleja, o muy especifica, supongamos una variable que solo puede aceptar solo cadenas especificas y no strings en general

```ts
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
```

o una que solo pueda aceptar valores numericos especificos:
```ts
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

cualquier valor que no coincida con los declarados en dicha union no sera aceptado y typescript te avisara, tambien sirve para señalar diferentes naturalezas de ciertos parametros del mismo tipo supongamos una funcion cuya respuesta depende de si recibe un string o una cadena de strings 
```ts
function wrapInArray(obj: string | string[]) {
  if (typeof obj === "string") {
    return [obj];
            
(parameter) obj: string
  }
  return obj;
}
```

tambien podemos usarlos para formatear una respuesta que debe cumplir con cierta estructura de datos
```ts
type Number = `${string}:${number}`

let number:Number = `hola:${5}`;
```

#### genericos

estos nos permiten declarar la naturaleza del contenido de una estructura de datos, por ejemplo una variable que señalamos como un arreglo puede ser un arreglo de cualquier cosa, con un generico podemos señalar la naturaleza especifica de dichoa arrreglo 
```ts
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

tambien podemos usarlos para definir incluso en otro momento durante la ejecucion la naturaleza del cierto objeto
```ts
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}
 
// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;
 
// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();
 
// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23) // error number is not asignable to type string;
```

## Structural Type System
```ts
```

### tipos en javacript con typeof

string	typeof s === "string"
number	typeof n === "number"
boolean	typeof b === "boolean"
undefined	typeof undefined === "undefined"
function	typeof f === "function"
array	Array.isArray(a)
