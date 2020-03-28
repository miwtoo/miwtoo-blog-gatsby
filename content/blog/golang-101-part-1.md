---
title: "Golang 101 Part 1"
date: 2020-03-27T10:54:22+07:00
draft: false
---

# Hello World 🌐

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, ชาวโลก")
}
```
- `Hello, ชาวโลก`

---

# Package
หากระบุเป็น main จะหมายถึงว่าเป็นไฟล์สำหรับรัน แต่หากระบุเป็นชื่ออื่นจะหมายถึงให้ไฟล์อื่นนำไปใช้

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```
- `My favorite number is 1`


---

# import 💼
ทำได้ 2 แบบ
```go
import (
	"fmt"
	"math"
)
```
หรือ
```go
import "fmt"
import "math"
```
---

# Exported names 🌏

ชื่อที่ส่งออกให้ภายนอกใช้ต้องขึ้นต้นด้วยพิมใหญ่ เช่น

math.Pi ✅

math.pi 🚫 


```go
func main() {
	fmt.Println(math.Pi)
}
```