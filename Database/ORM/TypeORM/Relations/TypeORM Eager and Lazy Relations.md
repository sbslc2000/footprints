# TypeORM Eager and Lazy Relations

## Eager Relations
```ts
import {
Entity,
PrimaryGeneratedColumn,
Column,
ManyToMany,
JoinTable,
} from "typeorm"
import { Category } from "./Category"

@Entity()
export class Question {
@PrimaryGeneratedColumn()
id: number

@Column()
title: string

@Column()
text: string

@ManyToMany((type) => Category, (category) => category.questions, {
eager: true,
})
@JoinTable()
categories: Category[]
}

```
위 처럼 작성하는 경우 Question을 가져올 때 Category가 자동으로 로드되므로 별도의 join을 명시해주지 않아도 된다.

* Eager relations는 오직 find* 메서드에서만 동작한다.
* 만약 QueryBuilder를 사용할 경우, Eager relations는 비활성화되며, 대신 `leftJoinAndSelect`를 사용해주어야 한다.
* Eager relations는 오직 한 방향의 엔티티에서만 사용되어야 한다.

## Lazy Relations
```ts
import {
Entity,
PrimaryGeneratedColumn,
Column,
ManyToMany,
JoinTable,
} from "typeorm"
import { Category } from "./Category"

@Entity()
export class Question {
@PrimaryGeneratedColumn()
id: number

@Column()
title: string

@Column()
text: string

@ManyToMany((type) => Category, (category) => category.questions)
@JoinTable()
categories: Promise<Category[]>
}
```
Lazy Relations를 사용하고 싶다면 타입 정보에 Promise를 사용해야 한다. Lazy Relations로 엮여있는 데이터를 삽입하거나 가져오려면 다음과 같이 작성해주어야 한다.

```ts
//삽입
question.categories = Promise.resolve([category1, category2])

//조회
const [question] = await dataSource.getRepository(Question).find()  
const categories = await question.categories
```