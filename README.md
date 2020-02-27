MongoDB 
  -> Schema free: sua estrutura pode ser alterada em tempo de vida da aplicação
  -> Na mesma tabela, um registro pode ter user e outro não
  -> Não é feito para ter relacionamentos

SQL
  -> Migrations: "Controle de versão" do seu banco
  -> Banco relacional
  -> Banco recomendado: PostgresSQL

- Não faz sentido usar banco noSQL na maioria das aplicações

- Há a possibilidade de extrair certos campos para bancos noSQL - geralmente mais performático se utilizado corretamente

Níveis de abstração: forma de se comunicar com o banco
  -> 1°: Driver nativo - escrever as queries (node-postgres.com)
  -> 2º: Query builder (knexjs.org)
    Ex.: knex('books').insert({ title: "Slaughterhouse"})
  -> 3°: ORM (sequelize.org)
    - Definimos um model que define como vai ser a comunicação a nossa aplicação com o banco de dados

Etapas no terminal
  > yarn add express pg pg-hstore sequelize

  > yarn add sequelize-cli -D

Criamos o banco de dados definido em src/config/database.js
  > yarn sequelize db:create

Criamos a primeira Migration (que é uma tabela)
  > yarn sequelize migration:create --name=create-users

  > yarn sequelize db:migrate

Desfazer tudo que foi executado na ultima migration (apenas no seu próprio ambiente de desenvolvimento)
  > yarn sequelize db:migrate:undo

Nota: Para arrumar um erro em produção, deve ser feita uma migration nova

---- MODEL

-> model: é uma representação de como a nossa aplicação vai se comunicar com a base de dados

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
