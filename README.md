# Postgres parametrized query builder

## Usage:

#### Basic statement:
```typescript
import { sql } from "sqvaer";

const number = 99
const statement = sql`SELECT 1 + ${number} AS sum`

const [rows] = await query(statement)
console.log(rows) // returns [ { sum: 100 } ]

```

#### Append statement:
```typescript
import { sql, query } from "sqvaer";
const params = { age: 18 }

const statement = sql`SELECT name, age FROM users`

if (params.age) {
    statement.append(sql`WHERE age > ${params.age}`)
}

const [rows] = await query(statement)
console.log(rows) // returns [ { name: 'Mary', age: 39 }, { name: 'Ingvar', age: 27 } ]
```

#### Join statement:
```typescript
import { sql, transaction } from "sqvaer";

const params = { age: 18, name: 'Lo' }

const statement = sql`SELECT name, age FROM users`
const conditions = []

if (params.age) {
    conditions.push(sql`age > ${params.age}`)
}

if (params.name) {
    conditions.push(sql`name iLike ${params.name + '%'}`)
}

if (conditions.length) {
    statement.append(sql`WHERE`)
    statement.join(conditions, 'AND')
}

const [rows] = await query(statement)
console.log(rows) // returns [ { name: 'Lotar', age: 19 } ]
```