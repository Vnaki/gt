#### Usage

```go 
package main

import (
	"fmt"
	"gt"
)

type Model struct {
	Id        int32  `db:"id,omitempty" gen:"pk,ai"`
	CreatedAt string `db:"created_at"`
}

type ThreeStudentModel struct {
	Model
	Name  string `db:"name" gen:"notnull"`
    Content   string `db:"content" gen:"type:text"`
	Score int    `db:"score" gen:"length:1,decimal:1,default:1,notnull,unsigned"`
}

type TwoStudent struct {
	Model
	Name  string `db:"name" gen:"notnull"`
    Content string `db:"content" gen:"type:text"`
	Score int    `db:"score" gen:"length:1,decimal:1,default:1,notnull,unsigned"`
}

func main() {
	b := gt.New()
	b.SetSchema("stu")

	sql, err := b.Model(ThreeStudentModel{})
	fmt.Println(sql, err)

	sql, err = b.Model(TwoStudent{}, "twostu")
	fmt.Println(sql, err)

	b = gt.New()
	b.SetMode(gt.MYSQL)

	sql, err = b.Model(TwoStudent{})
	fmt.Println(sql, err)
}
```

result output

```sql 
-- sqlite
CREATE TABLE 'stu'.'three_student'(
'id' int PRIMARY KEY AUTO_INCREMENT,
'created_at' varchar,
'name' varchar NOT NULL,
'content' text,
'score' bigint UNSIGNED NOT NULL DEFAULT 1
);

CREATE TABLE 'stu'.'twostu'(
'id' int PRIMARY KEY AUTO_INCREMENT,
'created_at' varchar,
'name' varchar NOT NULL,
'content' varchar,
'score' bigint UNSIGNED NOT NULL DEFAULT 1
);

-- mysql
CREATE TABLE `three_student`(
`id` int PRIMARY KEY AUTO_INCREMENT,
`created_at` varchar,
`name` varchar NOT NULL,
`content` text,
`score` bigint UNSIGNED NOT NULL DEFAULT 1
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8mb4;

CREATE TABLE `twostu`(
`id` int PRIMARY KEY AUTO_INCREMENT,
`created_at` varchar,
`name` varchar NOT NULL,
`content` varchar,
`score` bigint UNSIGNED NOT NULL DEFAULT 1
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8mb4;
```
#### Mode 

- MYSQL
- SQLITE

#### Tag `db`

Corresponding data table column name, `Id` to `id`, `Content` to `content` 

```go 
type People struct {
    Id        int32  `db:"id,omitempty" gen:"pk,ai"`
    Content   string `db:"content" gen:"type:text"`
}


```

#### Tag `gen`

| 属性 | 默认值 | 说明 |
| --- | --- | --- |
| type | | 原生sql数据类型:char,text,mediumint,timestamp,datetime 等 |
| length | | 数据长度 |
| decimal | 2 | 浮点类型精度 |
| default | | 默认值 |
| pk | | 主键 |
| ai | | 自增 |
| comment | | 注释 |
| unsigned | | 无符号 |
| notnull | | not null |

#### Integer Data Type

| 数据库数据类型 | 范围 | 无符号范围 | 数据类型 |
| --- | --- | --- | --- |
| TINYINT | -128〜127 | 0 〜255 | int8/uint8 |
| SMALLINT | -32768〜32767 | 0〜65535 | int16/uint16|
| INT (INTEGER) | -2147483648〜2147483647 | 0〜4294967295 | int32/uint32|
| BIGINT | -9223372036854775808〜9223372036854775807 | 0〜18446744073709551615 | int64 int / uint64 uint|

#### String Data Type

``` 
string -> varchar
```

```
// int int8 int16 int32 int64 byte rune
// uint uint8 uint16 uint32 uint64 byte rune
// float32 float64
// char varchar text
// datetime timestamp

// TINYINT	-128〜127	0 〜255        int8
// SMALLINT	-32768〜32767	0〜65535   int16
// MEDIUMINT	-8388608〜8388607	0〜16777215
// INT (INTEGER)	-2147483648〜2147483647	0〜4294967295   int32
// BIGINT	-9223372036854775808〜9223372036854775807	0〜18446744073709551615 int64 int
//

```
