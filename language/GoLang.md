##  1. 基础

### 1.1 if

```go
const filename = "abc.text" //const 常量，无需定义类型
//1
content, err := ioutil.ReadFile(filename) // := 初始化声明，无需定义类型 := 是一个声明语句 
if err != nil{ //和Java相比，内有()
    fmt.Print(err)
}else{
    fmt.Printf("%s",content)
}
//2 可以加;后接条件
if content, err := ioutil.ReadFile(filename); err != nil{
    fmt.Print(err)
}else{
    fmt.Printf("%s",content)
}
```

### 1.2 switch

==switch可以不需要表达式，在条件中加入就可以，不需要break（jdk14支持）==

```go
//函数返回值类型在参数后声明
func grade(score int) string {
    level := ""
    switch{
        case score < 0 || score > 100
       		panic(fmt.Sprintf("Wrong Score!"))
        case score < 60:
        	level = "F"t
        case score < 70:
        	level = "D"
        case score < 80
        	level = "C"
        case score < 90
        	level = "B"
        case score <= 100
        	level = "A"
    }
    return level
}
```

### 1.3 for

==for没有括号，只有条件相当于Java中的while，什么没有就是死循环==

```go
//整数转成二进制
func convertToBin(n int) string {
	result := ""
	if n == 0 {
		return "0"
	}
	for ; n > 0; n /= 2{
		lsb := n%2
		result = strconv.Itoa(lsb) + result
	}
	return result
}
```

```go
//读取文件，按行读
func printFile(filename string)  {
	file, err :=os.Open(filename) //OutputStream 返回的是文件 *file
	if err != nil {
		panic(err) //出错
	}
	scanner := bufio.NewScanner(file) //BufferInpuStream 返回的是 *Scanner
	for scanner.Scan() {
		fmt.Print(scanner.Text())
	}
}
```



## 2. 函数

### 2.1 定义

==func sum(numbers ...int)可变参数列表==

```go
//简单计算
func eval(a,b int, op string) (int, error) {
	switch op {
	case "+":
		return a + b, nil //返回多个值
	case "-":
		return a - b, nil
	case "*":
		return a * b, nil
	case "/":
		if b == 0 {
            return 0,fmt.Errorf("0不能做除数")
		}
        q,_ := div(a, b) //只需要第一个参数
		return q, nil
	default:
        return 0,fmt.Errorf("unsupport opration : %s",op)
	}
}

//余数
func div(a, b int) (q, r int){ //多返回值参数类型相同
    return a / b, a % b
}
```

### 2.2 指针

==指针不能运算==

```go
var  a int = 2
var pa *int = &a
*pa = 3
fmt.Print(a)
```

`go语言只有值传递和Java一样`

<img src="C:\Users\Administrator\Desktop\note\GoLang\1.PNG" style="zoom:80%;" />

```go
//交换两个数
func swap(a,b *int) {
	*a,*b =*b,*a
}
func swap(a,b int)(c,d int){
    c,d=b,a
    return c,d
}
```

### 2.3 参数传递

- **普通变量**
  使用普通变量作为函数参数的时候，在传递参数时只是对变量值得拷贝，即将实参的值复制给变参，当函数对变参进行处理时，并不会影响原来实参的值。
- **指针**
  函数的变量不仅可以使用普通变量，还可以使用指针变量，使用指针变量作为函数的参数时，在进行参数传递时将是一个地址看呗，即将实参的内存地址复制给变参，这时对变参的修改也将会影响到实参的值。
- **数组**
  和其他语言不同的是，go语言在将数组名作为函数参数的时候，参数传递即是对数组的复制。在形参中对数组元素的修改都不会影响到数组元素原来的值。
- **slice**, **map**, **chan**
  在使用slice, map, chan 作为函数参数时，进行参数传递将是一个地址拷贝，即将底层数组的内存地址复制给参数slice, map, chan 。这时，对slice, map, chan 元素的操作就是对底层数组元素的操作。
- **函数名字**
  在go语言中，函数也作为一种数据类型，所以函数也可以作为函数的参数来使用

## 3.数组，切片介绍

### 3.1 数组的定义

==[5]int和[]int是不同类型，数组是值传递，作为函数参数，会拷贝数组，不会改变原值==

```go
var arr1 [5]int //数量写在类型前面
arr2 := [3]int{1,2,3}
arr3 := [...]int{1,2,3} //自动推断长度
var grid [4][5]bool
//range关键字可以获得下标和值
//for _,v := range arr3 //下划线可省略值
for i,v := range arr3{
    fmt.Printf("%d %d\n",i,v)
}
```

### 3.2 数组指针

==数组指针没有指向首地址的说法，使用的时候直接用arr[i]==

```go
func updateArr(arr *[5]int,i,v int)  {
    //不需要 *arr[i]
	arr[i] = v
}
//调用
updateArr(&arr1,2,8)
```

### 3.3 切片(Slice)

`切片是数组的view 底层是数组`    ==改变试图就改变切片==

```go
arr := []int {0,1,2,3,4,5,6,7}
//使用
fmt.Println(arr[2:6]) //半开半闭 2，3，4，5 (2 <= i < 6)
fmt.Println(arr[:6])
fmt.Println(arr[6:])
fmt.Println(arr[:])
```

<img src="C:\Users\Administrator\Desktop\note\GoLang\2.PNG" style="zoom:75%;" />

<img src="C:\Users\Administrator\Desktop\note\GoLang\3.PNG" style="zoom:75%;" />

==slice可以向后扩展(不可以超过cap值)，不可以向前扩展==

### 3.4 切片append操作

- **像切片中添加元素如果超越了cap,系统会重新分配更大的底层数组（原数组可能会被gc)**
- **由于值传递关系,必须接受append 的返回值**
- **s =append(s,val)**

```go
func sliceOps(arr []int,v int)  []int{
   return append(arr,v)
}
arr := [...]int {0,1,2,3,4,5,6,7}
// 第一种
fmt.Println(sliceOps(arr[4:],45)) //超过原数组长，创建新的底层数组
//[4 5 6 7 45]
fmt.Println(sliceOps(arr[4:],54)) //超过原数组长，创建新的底层数组
//[4 5 6 7 54]

//第二种
tem := sliceOps(arr[4:],45)
//[4 5 6 7 45]
fmt.Println(tem) //超过原数组长，创建新的底层数组
fmt.Println(sliceOps(tem[:],54)) //超过tem切片底层组长，创建新的底层数组
//[4 5 6 7 45 54]
```

### 3.5 创建切片

==可以使用make(s,len,cap)定义切片==

```go
//切片初始化
var arr []int
for i := 0 ;i < 100; i++  {
    arr = append(arr,i * 2 + 1)
}

//slice大小为3,cap为3
s1 := []int {1,2,3}
fmt.Println(s1)

//slice大小为16,cap为16
s2 := make([]int,16)

//slice大小为10，底层数组cap为32
s3 := make([]int, 10, 32)
```

### 3.6 切片复制，删除元素

`arr[3:]...可变参数`

```go
//将arr切片复制给s
copy(s,arr[:])
//删除下标为2的元素
s1 := append(arr[0:2],arr[3:]...)  //len减一,cap不变

//popping first element
front := arr[0]
arr = arr[1:]
fmt.Println(front)

//popping last element
last := arr[len(arr)-1]
arr = arr[:len(arr)-1]
fmt.Println(last)
```

##  4.map

### 4.1 map创建

`map是无序`

```go
//1. 直接定义赋值初始化
m0 := map[string]string{
    "key": "value",
    "name": "bob",
}

//2. 使用make函数创建
m1 := make(map[string]string)

//变量定义
var m2 map[string]string
fmt.Println(m0,m1,m2)
```

### 4.2 map获取元素

`如果key不存在，返回的value为空`

```go
//通过range关键字遍历
for k ,v := range m0{
fmt.Println(k,v)
}
//通过key获取value
name := m0["name"]
fmt.Println(name)

//通过返回第二个参数 ok 判断key是否存在
if name,ok := m0["name"]; ok {
    fmt.Println(name)
} else{
    fmt.Println("key doesn't exist")
}
```

### 4.3  map删除元素

```go
delete(map,key)
```

<img src="C:\Users\Administrator\Desktop\note\GoLang\4.PNG" style="zoom:75%;" />

>字符串中的每一个元素叫做“字符”，在遍历或者单个获取字符串元素时可以获得字符。

Go语言的字符有以下两种：

- 一种是 uint8 类型，或者叫 byte 型，代表了 ASCII 码的一个字符。
- 另一种是 rune 类型，代表一个 UTF-8 字符，当需要处理中文、日文或者其他复合字符时，则需要用到 rune 类型。rune 类型等价于 int32 类型

### 4.4  最大无重复子字符串

```go
//国际化
func findMaxNonRepeatingString(str string) string{
	if len(str) < 2 {
		return str
	}
	m := make(map[rune]int,len(str))
	cur,pre,suf,maxLen := 0,0,0,0
	for i, c := range []rune(str){
		v, ok := m[c]
		if ok {
			v = m[c] + 1
			m[c] = v
			cur = v
		}else {
			m[c] = i
			if 	i - cur > maxLen {
				maxLen = i - cur
				pre = cur
				suf = i
			}
		}
	}
	//切片半开半闭
	return str[pre:suf+1]
}
```

> rune 类型 相当于Java 中 char
>
> utf-8 每个中文3字节 ，英文1字节

```go
//将字节转中文
func  decodeRune(str string)  {
    //字符数
	utf8.RuneCountInString(str)
    //获得所有字节
	bytes := []byte(str)
	for len(bytes) > 0 {
		ch,size := utf8.DecodeRune(bytes)
		bytes = bytes[size:]
		fmt.Printf("%c ", ch)
	}
}
```

## 5.面向对象

==**go仅支持封装，不支持继承和多态**==

### 5.1 struct的创建

```go
var root treeNode
//1.
root = treeNode{value: 3}
//2.
root.left = &treeNode{}
//3.
root.right = &treeNode{5,nil,nil}
//4.
root.right.left = new(treeNode)
//5.
nodes := []treeNode{
    {1,nil,nil},
    {3,nil,nil},
}
```

**使用自定义的工厂函数创建** `返回的是局部变量的地址`

```go
func createNode(val int) *treeNode{
    return &treeNode{value: val}
}
```

> go局部变量是堆上还是栈上？

### 5.2 创建struck的函数

- ==函数有一个接收者，表示是哪个struck的函数==
- ==只有使用指针才可以改变结构体内容==
- ==nil指针也可以调用方法==



```go
//1.拷贝一份给函数
func (node treeNode) print()  {
	fmt.Print(node.value)
}

//2.
func print(node treeNode)  {
	fmt.Print(node.value)
}

//调用
root.print()
print(root)

//设置和修改值 指针告诉编译器传入的是地址
func (node *treeNode)setValue(val int)  {
	node.value = val
}
//1.
root.setValue(1)
//2. 传入root和root的地址都是等价的，编译器自动判断
Rroot := &root
Rroot.setValue(1)
```

### 5.3 函数封装

==**go语言一般通过名字对函数封装**==

- 名字一般使用CamelCase
- 首字母大写：public
- 首字母小写：private

==**包**==

- 每个目录一个包
- main包包含可执行入口
- 为结构体定义的方法必须放在同一个包内
- 可以是不同的文件

### 5.4 如何扩充系统类型或别人的类型

- 定义别名

  ```go
  type Queue []int
  //类型对应 q表示地址，*q表示 []int 型
  func (q *Queue) push(val int) {
  	 *q = append(*q,val)
  }
  
  func (q *Queue) pop() int{
  	x := (*q)[0]
  	*q = (*q)[1:]
  	return x
  }
  ```

- 使用组合

  ```go
  type myNode struct{
      myNode *Node
  }
  ```

## 6. GOPATH环境变量

### 6.1 设置

- 官方推荐：所有的项目和第三方库文件都放在同一个GOPATH下
- 也可以放在不同的GOPATH

## 7. 接口

### 7.1 接口定义

`没有接口名`

```go
//定义接口
type Retriever interface{
	Get(url string) string
}
```

### 7.2 接口实现

```go
type Retriever struct {
	Content string
}
//接口实现：实现接口中的方法就实现了该接口
func (retriever Retriever)Get(url string) string {
	return retriever.Content
}

//真实实现
func (r Retriever)Get(url string) string {
	resp,err := http.Get(url)
	if err != nil{
		panic(err)
	}
	res, err :=httputil.DumpResponse(resp,true)
	resp.Body.Close()
	if err != nil{
		panic(err)
	}
	return string(res)
}
```

### 7.3 接口的调用

```go
//Retriever的方法实现了接口，所以通过Retriever调用
func Download(retriever Retriever) string {
   return retriever.Get()
}
```

### 7.4 接口的值类型

- 可以是值(是拷贝的)

- 也可以是指针

  ==可以通过`对象.(type)得到对象实体类型`或`对象.(类型)`判断是否是该类型的对象==

  ```go
  func inspect(r Retriever) {
  	fmt.Printf("%T %v\n", r, r)
  	switch v := r.(type) {
  	case mock.Retriever:
  		fmt.Println("content：",v.Content)
  	case *real.Retriever:
  		fmt.Println("TimeOut:",v.TimeOut)
  	}
  }
  //调用
  r := mock.Retriever{Content: "this is a fake page"}
  r1 := real.Retriever{TimeOut: time.Minute}
  inspect(r)
  //结果：
  //mock.Retriever {this is a fake page}
  //content： this is a fake page
  inspect(&r1)
  //结果：
  //*real.Retriever &{1m0s}
  //TimeOut: 1m0s
  ```

**查看接口变量**

1. 表示任何类型： interface{}
2. Type Assertion
3. Type Switch

###  7.5 系统常用接口

```go
String() string //相当于Java中tostring
Read(p []byte) (n int, err error) //读文件 可以是文件，网络
Write(p []byte) (n int, err error) //写文件
```

### 7.6 接口组合

```go
type ReadWriter interface {
	Reader
    Writer
}
```

## 8. 函数式编程

### 8.1 函数式编程差异

- 函数是一等公民：参数，变量，返回值都可以是函数
- 高阶函数
- 函数 -> 闭包

> 什么是闭包？

<img src="C:\Users\Administrator\Desktop\note\GoLang\5.PNG" style="zoom:75%;" />

```go
//闭包
func adder() func(int) int {
	sum := 0 //sum在函数体外，自由变量
	return func(v int ) int{ //return的是 函数以及对sum变量的应用
		sum += v
		return sum
	}
}
```

```go
//正统的函数式编程，没有变量只有常量
//别名
type iAdder func(int) (int, iAdder)
func adder2(base int) iAdder  {
	return func(v int) (int, iAdder) { //匿名函数
		return base + v,adder2(base + v) //递归调用
	}
}
//调用
sum := adder2(0)
for i := 0;i < 10;i++ {
    var res int
    res ,sum =sum(i) //sum(i) --> sum(0).(i) 类似
    fmt.Println("sum:",i,res)
}
```

```java
//java8 以后使用Function接口和lambda表达式来创建函数对象
//匿名类或lambda表达式均支持闭包
Function<Integer, Integer> adder(){
    final Holder<Integer> sum = new Holder<>(0);
    return (Integer value) ->{
        sum.value +=value;
        return sum.value;
    };
}
```

### 8.2 斐波那契数列

```go
//闭包
//定义
type intGen func() int
func fib() intGen{
	a, b := 0, 1
	return func() int{
		a, b = b, a+b
		return a
	}
}
//调用
f := fib()
fmt.Println(f())
```

### 8.3  斐波那契数列升级版

```go
//为函数实现接口
//p是缓冲区
func (g intGen) Read(p []byte) (n int, err error) {
	//读取下一个数
	next := g()
	//读到10000停止
	if next > 10000{
		return 0,io.EOF
	}
	//将数字和换行符转成字符串
	s := fmt.Sprintf("%d\n",next)
	//使用代理将s写入p中
	return strings.NewReader(s).Read(p)
}
//打印输出字节流
func printFileContent(reader io.Reader)  {
	//通过缓冲io读取reader，得到scanner
	scanner := bufio.NewScanner(reader)
	//判断scanner是否打印输出完
	for scanner.Scan() {
		//scanner沾边为字符串
		fmt.Println(scanner.Text())
	}
}
//调用
f := fib()
printFileContent(f)
```

## 9. 资源管理

### 9.1 defer进行资源管理

- `在return之前执行`
- `执行顺序栈，先定义后执行`
- `参数在defer时计算`

```go
//将前二十，写入文件中
func writerFile(filename string)  {
	file, err := os.Create(filename)
	if err != nil {
		panic(err)
	}
	defer file.Close()
	writer := bufio.NewWriter(file)
	//写完之后，执行
	defer writer.Flush()
	f := fib()
	for i := 0; i < 20; i++ {
		fmt.Fprint(writer,f())
	}
}
```

### 9.2 什么时候使用defer

- **Open/Close**
- **Lock/Unlock**
- **PrintHeader/PrintFooter**

## 10. 出错管理

- **panic**
  - 停止当前函数执行
  - 一直向上返回，执行每一层的defer
  - 如果没有遇见recover，程序退出

- **recover**
  - 仅在defer调用中调用
  - 获取panic的值
  - 如果无法处理，可重新panic

## 11. 测试

### 11.1 传统测试 VS 表格驱动测试

|             传统测试             |       表格驱动测试       |
| :------------------------------: | :----------------------: |
|    测试数据和测试逻辑混在一起    | 分离的测试数据和测试逻辑 |
|          出错信息不明确          |      明确的出错信息      |
| 与i但与i个数据出错，测试全部结束 |       可以部分失败       |

<img src="C:\Users\Administrator\Desktop\note\GoLang\6.PNG" style="zoom:75%;" />

### 11.2  测试实例

**编写测试要点**

- `包名要为xxx_test`
- `测试函数名为Testxxx(t  *testing.xxx)`
- `测试数据表格定义为： tests := []struct{参数列表}{{测试数据1}，{测试数据2}}`

```go
package main

import (
	"math"
	"testing"
)

func TestTriangle(t *testing.T)  {
	tests := []struct{a, b, c int}{
		{3,4,5},
		{5,12,13},
		{8,15,17},
	}
	for _,test := range tests {
		if actual := calcTriangle(test.a,test.b);actual != test.c {
		 t.Errorf("calTriangle(%d,%d),get:%d;expected :%d",
			test.a,test.b,actual,test.c)
		}
	}
}
func calcTriangle(a, b int) int {
	c := int(math.Sqrt(float64(a * a + b * b)))
	return c
}
```

## 12. goroutine

### 12.1 协程 Coroutine

- 轻量级"线程"
- 非抢占式多任务处理，由协程主动控制权
- 编译器/解释器/虚拟机层面多任务
- 多个协程可能在一个或多个线程上运行

```go
//非抢占式演示
func main() {
	var a [10]int
	for i := 0; i < 10; i++ {
		go func(i int) {
			for  {
				a[i]++
				//交出控制权，io操作也可以。否则程序死循环
				runtime.Gosched()
				//fmt.Printf("hello %d\n",i)
			}
		}(i)
	}
	time.Sleep(time.Second)
	fmt.Println(a)
}

```

<img src="C:\Users\Administrator\Desktop\note\GoLang\7.PNG" style="zoom:75%;" />

<img src="C:\Users\Administrator\Desktop\note\GoLang\8.PNG" style="zoom:75%;" />

### 12.2 goroutine的定义

- **任何函数只需要加上go救恩送给调度器运行**
- **不需要在定义时区分是否为异步函数**
- **调度器在合适的点进行切换**
- **使用 -race 来检测数据访问冲突**

### 12.3  goroutine可能切换的点

- **I/O,select**
- **channel**
- **等待锁**
- **函数调用（有时）**
- **runtime.Gosched()**

### 12.4 channel

==**1. goroutine之间的通信**==

<img src="C:\Users\Administrator\Desktop\note\GoLang\9.PNG" style="zoom:75%;" />

==**2. chennel定义及调用**==

`channel只能在goroutine之间使用`

```go
func chanDemo() {
	//var c chan int
	c := make(chan int)
	go func() {
		for true {
            //取出
			n := <-c
			fmt.Println(n)
		}
	}()
    //放入
	c <- 1
	c <- 2
	time.Sleep(time.Microsecond)
}
```

`发数据必须要有接受，阻塞发送，不接受会dead lock`

```go
func worker(c chan int,id int){
    //接受全部信息，否则报错
	for true {
		fmt.Printf("worker:%d,msg:%d \n",id,<-c)
	}
}
func chanDemo() {
	//var c chan int
	c := make(chan int)
	go worker(c,12)
	c <- 1
	c <- 2
	time.Sleep(time.Microsecond)
}
```

```go
//创建chan int 并立刻返回； work部分是 go func()执行
//func createWorker(id int) chan<- int{ 只能接受数据，只读
func createWorker(id int) chan int{
	c := make(chan int)
	go func() {
		fmt.Printf("worker: %d msg:%d \n",id,<-c)
	}()
	return c
}
func demo2()  {
	var channels [10]chan intnel
    //创建十个int 型 channel
	for i := 0; i < 10; i++ {
		channels[i] = createWorker(i)
	}
    //分别向十个channel发送数据
	for i := 0; i < 10; i++ {
		channels[i] <- 'a' + i
	}
	time.Sleep(time.Second)
}
```

**==3. bufferdchannel==**

```go
//定义
c := make(chan int,bufferdsize) //bufferdsize大小的缓冲区
```

**==4. close==**

```go
//使用
c := make(chan int,bufferdsize) 
//发送完，关闭，接收方任然会接受到类型的零值
close(c)

//使用range来收
if n ：= range c
//使用ok接受发送完毕信息
n, ok := <- c
```

### 12.5 等待任务做完

- **设置一个 chan bool 标志**
- **sync.waitGroup**

**select**

```go
c1,c2 := generator(),generator()
for true {
    select {
    case n := <-c1:
    fmt.Println("from c1:",n)
    case n := <-c2:
    fmt.Println("from c2:",n)
    //default:
    //	fmt.Printf("no data from ")
    }
}
//创建数据
func generator() chan  int {
	out := make(chan int)
	go func() {
		i := 1
		for true {
			time.Sleep(
				time.Duration(
					rand.Intn(1000)) * time.Millisecond)
			out <- i
			i++
		}
	}()
	return out
}
```

## 13 package

- 同一个目录下的统同级文件归属于一个包
- 同一个目录下的包名，建议使用该目录名
- main包为应用程序注入口，其他包不能使用
- 包可以嵌套

> 同一个包下的文件属于同一个工程文件，不用import包，直接使用

```yaml
-project
	-src
	-pkg
	-bin
	-vender
GOPATH = /project/
```

## 14 Spider

### 14.1 基本思路

- **清晰需要类容**
- **分析网页**
- **获取网页信息**
- **解析网页信息**

## 15. time包

> 时间的格式化，只能是6,1,2,3,4,5,6 

```go
Create(name string)(file *File,err error) //创建名为filename的文件，如果文件存在，会截断他，任何人都可以读写，返回的对象可用于io，对应的文件描述符具有o—rdwr模式
Open(name string) //打开文件，只能用于读取
OpenFile(naem string,flag int,perm FileMode) (file *File,err error)
```



