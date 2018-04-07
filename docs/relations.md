# 关系

* [什么是关系](#what-are-relations)
* [关系选项](#relation-options)
* [级联](#cascades)
* [`@JoinColumn` 选项](#joincolumn-options)
* [`@JoinTable` 选项](#jointable-options)

## 什么是关系

关系可以让你方便的处理实体之间的联系。

有以下几种关系:

* 一对一 使用 `@OneToOne`
* 多对一 使用 `@ManyToOne`
* 一对多 使用 `@OneToMany`
* 多对多 使用 `@ManyToMany`
          
## 关系选项

你可以为关系指定以下选项

* `eager: boolean` - 如果设置为真，当使用`find*` 方法时关系将和主实体一同被载入。如果使用`QueryBuilder`，渴求式关系将被禁用与此同时你必须使用`leftJoinAndSelect` 来载入它。
* `cascadeInsert: boolean` - 如果设置为真, 关联对象将被自动插入数据库如果它还不存在于数据库的话。
* `cascadeUpdate: boolean` - 如果设置为真, 关联对象将在实体保存时被更新。
* `cascadeRemove: boolean` - 如果设置为真，关联对象将在实体保存且没有与之关联对象时从数据库中移除。
* `cascadeAll: boolean` - 一次性设置`cascadeInsert`, `cascadeUpdate`, `cascadeRemove`。 
* `onDelete: "RESTRICT"|"CASCADE"|"SET NULL"` - 指定引用对象被删除时外键的行为。
* `primary: boolean` - 表示关系中该列是否为主键列。
* `nullable: boolean` - 表示关系中该列是否允许为空。 默认允许为空。如果你在关系中使用了级联那么不推荐将该选项设置为假。

## 级联

级联的例子:

```typescript
import {Entity, PrimaryGeneratedColumn, Column, ManyToMany} from "typeorm";
import {Question} from "./Question";

@Entity()
export class Category {
    
    @PrimaryGeneratedColumn()
    id: number;
    
    @Column()
    name: string;
    
    @ManyToMany(type => Question, question => question.categories)
    questions: Question[];
    
}
```

```typescript
import {Entity, PrimaryGeneratedColumn, Column, ManyToMany, JoinTable} from "typeorm";
import {Category} from "./Category";

@Entity()
export class Question {
    
    @PrimaryGeneratedColumn()
    id: number;
    
    @Column()
    title: string;
    
    @Column()
    text: string;
    
    @ManyToMany(type => Category, category => category.questions, {
        cascadeInsert: true
    })
    @JoinTable()
    categories: Category[];
    
}
```

```typescript
const category1 = new Category();
category1.name = "animals";

const category2 = new Category();
category2.name = "zoo";

const question = new Question();
question.categories = [category1, category2];
await connection.manager.save(question);
```

正如这个例子所展示的我们没有调用 `category1` 和 `category2`的`save`方法。它们将自动被插入因为我们将`cascadeInsert`设置为真了。

使用 `cascadeUpdate`时，实体被保存的时候关系中的每一个对象都被会调用`save`方法。这意味着关系中的每个实体都会自动的改变如果它们存在于数据库之中。

当使用 `cascadeRemove`时，关系中每一个消失的对象都会被调用`remove`函数。这个方法的一个好例子是`Question`和`Answer`实体间的关系。当你删除一个拥有`answers: Answer[]` 的`Question`关系时你需要同时删除数据库中的所有回答。

记住 - 能力越大责任越大。级联看起来是一个又好又简单的方式来与关系一起作用，但是在一些意料之外的对象被存储到数据库时它也会带来一些bug和其他安全问题。并且，它提供了一个不明确的将新对象存储进数据库的方式。

## `@JoinColumn` 选项

`@JoinColumn` 不仅定义了关系的哪一边包含了带有外键的连接列，也允许你自定义连接列和引用列的名称。当我们设置了 `@JoinColumn`的时候，它将会自动在数据库里创建一个名为 `propertyName + referencedColumnName`的列。

示例:

```typescript
@ManyToOne(type => Category)
@JoinColumn() // this decorator is optional for @ManyToOne, but required for @OneToOne
category: Category;
```

这段代码将在数据库中创建一个 `categoryId` 列。
如果你想改变这个名称你可以指定一个自定义的连接列名：

```typescript
@ManyToOne(type => Category)
@JoinColumn({ name: "cat_id" })
category: Category;
```

连接列常常是一个其他列的引用（通过使用外键）。默认情况下你的关系通常引用了关联实体的主键列，如果您想要创建与关联实体的其他列之间的关系 - 你也可以在`@JoinColumn`中指定它们:

```typescript
@ManyToOne(type => Category)
@JoinColumn({ referencedColumnName: "name" })
category: Category;
```

关系现在指的是 `Category` 实体中的`name`而不是`id`。该关系的列名将改为`categoryName`。

## `@JoinTable` 选项

`@JoinTable`被用于多对多关系并且说明了“junction（连接）”表的列一个连接表是由TypeORM自动创建的包含指向关联实体列的特殊单表。你可以使用 `@JoinColumn`来更改连接表中的列名和他们引用的列：你也可以改变生成的“junction（连接）”表的名称。

```typescript
@ManyToMany(type => Category)
@JoinTable({
    name: "question_categories" // table name for the junction table of this relation
    joinColumn: {
        name: "question",
        referencedColumnName: "id"
    },
    inverseJoinColumn: {
        name: "category",
        referencedColumnName: "id"
    }
})
categories: Category[];
```

如果目标表有组合主键，那么你需要将一个属性数组传递给 `@JoinTable`。
