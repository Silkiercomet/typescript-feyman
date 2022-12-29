# typescript

typescript tiene una relacion inusual con javascript, ya que ofrece todos las caracteristicas de javascript pero con seguridad de datos, javascript te permite asignar valores primitivos como un string o un numero, pero estos valores pueden ser mutados en otros de distinta naturaleza de manera involuntaria, aqui es donde entra typescript en juego, se asegura de que todos los valores usados sean iguales en naturaleza a los declarados durante la inicializacion de la variable, es como un profesor que esta constantemente siguiendo tu codigo y te avisa cuando cometiste un error

## explicit declaration

durante la declaracion de una variable javascript analiza el contenido de esta para determinar la naturaleza de la variable, por eso, javascript es capaz de seguir cuando es imposible usar un metodo para arreglos en un numero o parsear un objeto a integer, pero esto es dependiente del valor de la variable de tal forma que si llega a mutar, su tipo tambien lo hara, typescript cambia esto definiendo la naturaleza de la variable es base al nombre de la variable y no a su contenido

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

en la programacion funcional podemos definir el typo de un parametro, la estructura o naturaleza del resultado que devolvera una funcion 

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

Cuando se trata de argumentos de una funcion typescript evalua la estructura del parametro siendo pasada a esta coincide con la declarada en la inicializacion de dicha funcion, de esta manera el argumento en cuestion sera comaparado y de coincider con los datos especificados se ejecutara incluso si el tipo es distinto

```ts
interface Point {
  x: number;
  y: number;
}
 
function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
 
// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);

// como veras el typo Point no es asignado a la constante point, sin embargo si es asignado al parametro por default de la funcion, lo que hara que esta busque los valores pertenecientes a la interfas y sera los que tomara durante la ejecucion

const point3 = { x: 12, y: 26, z: 89 };
logPoint(point3); // logs "12, 26"
 
const rect = { x: 33, y: 3, width: 30, height: 80 };
logPoint(rect); // logs "33, 3"
 
const color = { hex: "#187ABF" };
logPoint(color);
/*Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.
  Type '{ hex: string; }' is missing the following properties from type 'Point': x, y*/

class VirtualPoint {
  x: number;
  y: number;
 
  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}
 
const newVPoint = new VirtualPoint(13, 56);
logPoint(newVPoint); // logs "13, 56"
```

## Everyday types with typescript

### tipos primitivos
Typescript permite la declaracion explicita del tipo de una variable primitiva, aunque tambien typescript hace un buen trabajo asumiendo el typo de una variable basandose en el valor o la estructura de la variable una vez inicializada.

```ts
let myName: string = "Alice";
```
```ts
let myID: number = 5;
```
```ts
let married: boolean = false;
```

### type any
este es el valor por defecto que asigna TS cuando tu no defines ningun tipo y TS no puede averiguar la naturaleza de esta, este tipo le permitira acceder a cualquier metodo o llamado de cualquier tipo
```ts
let obj: any = { x: 0 };
// None of the following lines of code will throw compiler errors.
// Using `any` disables all further type checking, and it is assumed 
// you know the environment better than TypeScript.
obj.foo();
obj();
obj.bar = 100;
obj = "hello";
const n: number = obj;
```

### Functions
durante la definicion de la funcion podemos definir los tipos de los parametros que seran recibidos y del resultado que sera devuelto por la funcion, en caso de que no se devuelva nada se definira como "void" 
```ts
// Parameter type annotation
function greet(name: string): void {
  console.log("Hello, " + name.toUpperCase() + "!!");
}
```
cuando definimos funciones anonimas o callbacks typescript intuira que devovlera tal operacion basandose en la anturaleza de los parametros recibidos
```ts
// No type annotations here, but TypeScript can spot the bug
const names = ["Alice", "Bob", "Eve"];
 
// Contextual typing for function
names.forEach(function (s) {
  console.log(s.toUppercase());
// error: Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});

// Contextual typing also applies to arrow functions
names.forEach((s) => {
  console.log(s.toUppercase());
// error: Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});
```

### Object types

podemos declarar la estructura que tendra un parametro o una variable indicando las keys y su respectivo typo de la siguiente manera

```ts
// The parameter's type annotation is an object type
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });
```

#### optional types

en ocasiones tendremos valores que seran opcionales y podrian no ser pasados durante la llamada de la funcion TS cuando esto ocurre JS lo toma como un valor undefined, si se interactua con dicho parametro se recibira un error TS nos permite declarar parametros que pueden ser omitidos durante la invocacion de la funcion 

```ts
function printName(obj: { first: string; last?: string }) {
  // Error - might crash if 'obj.last' wasn't provided!
  console.log(obj.last.toUpperCase());
Object is possibly 'undefined'.
  if (obj.last !== undefined) {
    // OK
    console.log(obj.last.toUpperCase());
  }
 
  // A safe alternative using modern JavaScript syntax:
  console.log(obj.last?.toUpperCase());
}
```

### tipos en javacript con typeof

string	typeof s === "string"
number	typeof n === "number"
boolean	typeof b === "boolean"
undefined	typeof undefined === "undefined"
function	typeof f === "function"
array	Array.isArray(a)
