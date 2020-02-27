# js-examples

## Array.prototype.reduce()

- A função Array.prototype.reduce() tem 2 parametros, o primeiro é uma função e o outro é o o que voce quer passar como sendo o acumulador inicial (pode ser Number, String, Array, Object, ..., o que vc quiser)

`
[1, 2, 3, 4].reduce(() => {}, acumuladorInicial)
`

- a primeira função tem 3 parametros default, nessa ordem: (acumulador, itemAtual, indexDoItemAtual)

`
[1, 2, 3, 4].reduce((acumulador, itemAtual, indexDoItemAtual) => {}, acumuladorInicial)
`

O reduce vai percorrer todas as posições do array, assim como um map ou um forEach
A diferença esta no fato de ele ter um acumulador, onde vc literalmente acumula os resultados

1 - Exemplo com números

``` 
const sum = [1, 2, 3, 4].reduce((acumulador, itemAtual, indexDoItemAtual) => {
  return acumulador + itemAtual
}, 0)

console.log(sum)  // 10
``` 

- inicio: acumulador === 0. // note que poderia ser qualquer valor
- Após percorrer a 1a posição [0]: acumulador === 0 + 1 === 1
- Após percorrer a 2a posição [1]: acumulador === 1 + 2 === 3
- Após percorrer a 3a posição [2]: acumulador === 3 + 3 === 6
- Após percorrer a 4a posição [3]: acumulador === 6 + 4 === 10 

2 - Exemplo para mudança de estrutura

```
const newStructure = 
   [{ name: "xxx", age: 30 }, { name: "yyy", age: 20 }, { name: "zzz", age: 10 }]
        .reduce((acumulador, objetoAtual) => {
          return {
              ageSum: acumulador.ageSum ? acumulador.ageSum + objetoAtual.age : objetoAtual.age
          }
        }, {})

console.log(newStructure) // { ageSum: 50 + 10 }
```

- Inicio: acumulador === {}
- Após percorrer a 1a posição [0]: acumulador === { ageSum: 30 } // o acumulador não tinha nada
- Após percorrer a 2a posição [1]: acumulador === { ageSum : 30 + 20 } // 30 do acumulador anterior, 20 do objeto atual
- Após percorrer a 3a posição [2]: acumulador === { ageSum: 50 + 10 } // 50 do acumulador, 10 do objeto atual

3 - Exemplo utilizando spread operator

``` 
const newStructure = 
   [{ name: "xxx", age: 30 }, { name: "yyy", age: 20 }, { name: "zzz", age: 10 }]
        .reduce((acumulador, objetoAtual, indexAtual) => {
          return {
              ...acumulador,              
              [`age-${objetoAtual.name}`]: objetoAtual.age
          }
        }, {})

console.log(newStructure) // { age-xxx: 30, age-yyy: 20, age-zzz: 10 }
``` 

- Inicio: acumulador === {}
- Após percorrer a 1a posição [0]: acumulador === { age-xxx: 30 }
- Após percorrer a 2a posição [1]: acumulador === { age-xxx : 30, age-yyy: 20 } // age-xxx: 30 veio do uso do spread no acumulador, visto que inicialmente acumulador === { age-xxx: 30 }
- Após percorrer a 3a posição [2]: acumulador === { age-xxx: 30, age-yyy: 20, age-zzz: 10 }
