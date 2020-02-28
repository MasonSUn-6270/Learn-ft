1. #### cmd

   1. `go env`
   2. `go version`
   3. `go run xx.go`

2. :athletic_shoe:基础

   1. #### 三大件

      1. package main
      2. import （）
      3. func main() {           }

   2. #### 自定义函数

      1. func 
      2. add ( x:**int** , y **int**  )
      3. ~~return~~  int     **{   return ....    }**     :arrow_right:(string, string) **{  return y, x}**  :arrow_right:func xx(i int)**( x int ) { x=i return     ~~xxxxxx~~    } **

   3. #### 申明变量

      1. *var* x,y,z **int**
      2. *var* i , j **int** =1 , 2
      3. *var* c **~~bool~~**  =  false
      4. *var* c, python, java **:=** true, false, "no!"

   4. #### 申明常量

      1. ```go
         const Pi float64= 3.14 // 静止使用 :=
         ```

3. ### 流程控制

   1. ##### for

      ```go
      package main
      
      import "fmt"
      
      func main() {
      	sum := 0 
      	for i := 0;// 初始化语句 
          i < 10; //条件表达式
          i++ {
      		sum = i
      	}
      	fmt.Println(sum)
      }
      
      ```

      1. 初始化语句和后置语句是可选的
         1. func main() {
            	sum := 1
            	for  ~~; sum < 1000;~~ *条件可省去表示无限循环*      **{
            		sum += sum
            	}**
            	fmt.Println(sum)
            }

   2. #### if

      1. **if x<0 { }**
      2.  if **x:=a+b+c ;** x<0  {  } **else {  }**

   3. **switch** x:= a+b ; **x{**

       **case ... : **

      **.... **

      **case ... :**

      **....**

      **default:**

      **....... }**

      1. 简写

         ```go
         func main() {
         	t := time.Now()
         	switch {
         	case t.Hour() < 12:
         		fmt.Println("Good morning!")
         	case t.Hour() < 17:
         		fmt.Println("Good afternoon.")
         	default:
         		fmt.Println("Good evening.")
         	}
         ```

   4. #### defer

      1. 推迟调用
      2. defer 栈
      3. 

      

      

      