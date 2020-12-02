# 学习笔记

我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码？

答：应该Wrap这个error，抛给上层。在error发生的地方Wrap，上层可以用WithMessage添加信息，在程序的顶部或者是工作的 goroutine 顶部(请求入口)，
使用 %+v 把堆栈详情记录。避免到处打日志

# 代码

```go
package main

import (
	"database/sql"
	"fmt"
	"github.com/pkg/errors"
)

type User struct {
	ID   string
	Name string
}

func Dao(id int) (User, error) {
	err := sql.ErrNoRows
	return User{}, errors.Wrap(err, fmt.Sprintf("Dao: Can not find User with id: %d", id))
}

func main() {
	user, err := Dao(1)
	if err != nil {
		fmt.Printf("error info %+v\n", err)
		return
	}
	fmt.Println(user)
}

```
