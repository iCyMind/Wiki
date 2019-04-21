# Program Languges

## 类型/变量

- C
  - 强类型语言，必须类型匹配，否则会发生类型转换或者报错。
  - 主要类型 char, short, int, long, long long, float, double, long double 以及各整型的unsigned版本.
  - 各类型的指针:
```c
int value = 10;
int* ptr = &value;
printf("%d", *ptr);
```

  - 数组
```c
int array[10] = {1, 2, [4] = 3};
int multi[10][10];
char str[20];
```

  - 结构体:
```c
struct myType{
  int value;
  myType left;
  myType right;
};
struct myType node;
gets(ndoe.value);
```

  - 枚举类型, C语言中 enum 类型通常都是 int 类型的。
```c
enum color{
  red,
  blue
};
enum color myColor;
```

  - 常量
```c
const int PI = 3.1415;
#define PI 3.1415
```

  - 引进头文件
```c
#include <stdio.h>
```

  - 注释
```c
//comment
/* comments */
```

- Swift
  - 强类型语言。主要类型 Character, Int, UInt, UInt8, UInt16, Float, Double, Bool, String, 以及各类型的 optional 版本, 如 Int?
  - 变量/常量
```swift
var name = "change"
let name = "not_change"
```

  - 指针，通常 Swift 是不鼓励使用指针的，但是如果需要和一些 C 的 API 打交道，可以使用UnsafePointer/UnsafeMutablePointer类型
```swift
//C
void method(const int* num){
  printf("%d", *num);
}
//等价的 swift
func method(num: UnsafePointer<CInt>){
  print(num.memory)
}
```

  - Arrays/Sets/Dictionaries
```swift
var someInts = [Int]()
var threeDoubles = [Double](count: 3, repeatedValue: 0.0)
var shoppingList = ["eggs", "milk"]
//Sets
var letters = Set<Character>()
var music: Set<String> = ["rock", "classical", "hip hop"]
//Dictionaries
var namesOfIntegers = [Int: String]()
namesOfIntegers[16] = "sixteen"
namesOfIntegers = [:]
```

  - 结构体
  - 枚举类型, 和 c 语言的区别是，枚举类型不会隐式地赋值为0，1，2...
```swift
enum CompassPoint{
case North
case South
case East
case West
}
enum CP {
case North, South, East, West
}
```
  - 类
  - 引进头文件
  - 注释

- Bash
  - 弱类型，也只有数字和字符了。 **赋值时不允许带空格**
```bash
domain="wunderEye.com"
echo $domain
port=1984
```

  - 常量
```bash
readonly NAME=simon
```

  - 注释
```bash
#!/bin/bash
echo "this is a shell script"
#comment
```

  - 列表
```bash
list="item1 item2 item3"
#add item to list
list=$list" item4"
```
- ruby
  - 弱类型, 拥有Integer(Fixnum或者Bignum的实例), 浮点数(Float的实例)，字符串，数组， 哈希，范围等类型, 一个变量既可以存各种类型的数据。
  - ruby 的 Bignum 可以包含很大的数字，如 88 ** 88
  - ruby 还有一种特殊的 symbol 类型，如
```ruby
condition = :good
puts "good condition." if condition == :good
puts "bad condition." if condition == :bad
```
  可以将 symbol 理解为一种不存储值的变量名字。某些情况下，symbol 比字符串有效率，因为"good"/"bad"字符串每使用一次，就要创建相应的 String 实例。而 symbol 无论使用多少次，都是指向同一个引用。
  - 常量：大写字母开头的变量。
  - $前缀表示全局变量，@前缀表示实例变量，@@前缀表示类变量
  - 注释
```ruby
# comment
=begin
comments
=end
```

  - Array/Hash
```ruby
array = Array.new
array = Array.new(10)
array = Array.new(10, 0)
array = [1, 2, 3]
hash = Hash.new("default_value")
hash = {"red" => 0xf00, "green" => 0x0f0}
```

  - 结构体
```ruby
Customer = Struct.new(:name, :address)
Customer.new("Dave", "Street")
#或者用stdlib的openstruct
require 'ostruct'
record = OpenStruct.new
record.name = "John Smith"
record.age = 70
```

  - 类
```ruby
class Vehicle
  @@no_of_customers = 0
  def initialize(id, name, addr)
    @cust_id = id
    @cust_name = name
    @cust_addr = addr
  end
end
```

  - 枚举类型, 可以用 module 实现
  ```ruby
  module color
    RED = 1
    BLUE = 2
  end
  ```

  - 引进头文件: require something
## 输入输出

- c
```c
printf("%s", "something");
int age;
scanf("%d", &age);
FILE* fp = fopen("name.txt", "r");
fclose(fp);
```

- swift
```swift
print "something"
// input
let stdin = NSFileHandle.fileHandleWithStandardInput()
let input = NSString(data: stdin.availableData,
              encoding: NSUTF8StringEncoding)
// read from file
do {
  let text = String(contentsOfFile: path, encoding: NSUTF8StringEncoding)
}
catch {/* error handling here */}
// write to file
let some = "This is Text"
do {
  some.writeToFile(path, atomically: false, encoding: NSUTF8StringEncoding)
}
catch { /* error handling here */}
```  

- ruby

  - puts/putc/print/gets/readlines
```ruby
# gets oneline/multiline from stdin 
line = gets
lines = readlines
```

  - file IO
```ruby
# gets file conents with open
File.open("filename") do |file|
    puts file.gets
end
# gets file contents with new
f = File.new("filename", "r")
puts f.gets
f.close
# File.open("filename").each(",")
# File.open("filename").each_byte
# File.open("filename").each_char
# File.open("filename").each_readlines
# read an arbitrary number of bytes from file
File.open("filename") do |f|
    puts f.read(6)
end
```

```ruby
# some convenient methods 
data = File.read("filename")
array_of_file = File.readliens("filename")
```

```ruby
# write to file
f = File.new("log.txt", "a")
f.puts Time.now
f.close
# write and read
f = File.open("text.txt", "r+")
puts f.gets
f.puts "This is a test"
puts f.gets
f.close
# file mode: r/r+/w/w+/a/a+/b
```

  ①For opening and reading files, File.new and File.open appear identical, but they have different uses. File.open can accept a code block, and once the block is finished, the file will be closed automatically. However, File.new only returns a File object referring to the file. To close the file, you have to use its close method. Both the code block and file handle techniques have their uses. ②一般情况下，应该多使用类似File.read/File.readlines的快捷方法，它们更短更易于阅读，而且不需要手动关闭文件句柄。当需要逐行读取文件时再用其他方法。

- bash
  - echo/read  
  
## 运算符/表达式/语句

- c
  - \+ \- \* / % ++ -- += -= *= /= && || ! & |
  - int a, b = 4;

- swift
  - 除了和 C 一样的运算符，还有闭区间运算符 a...b， 半开区间运算符 a..<b
  - var a: Integer, 不需要分号结尾

- ruby
  - 除了和 C 一样的运算符，还有幂乘运算符 ** ， 和 swift 相反，ruby 的闭区间运算符是.. , 如'A'..'Z'包含所有的大写字母，'A'...'Z'则包含 A 到 Y 的所有大写字母。
  - 此外，ruby 也不支持++和--运算符，只能用+=/-=代替
  - age = 24 既没有分号，也不用指定类型
- bash
  - bash里只支持整数的运算，如果需要浮点数运算，需要用到程序 bc。并且做运算时需要美元符和中括号，sum=$[12 + 2]
  - bash 里拼接字符串不能用加号，之间写在一起即可。如
```bash
fname="lu"
name=$fname"zuoqing"  
```
  - bash的语句和 ruby 差不多，没有结束符号，也不用指定类型。但是赋值很特别，=号左边一定不能加空格。

## 循环

- c
```c
for (size_t i = 0; i < count; i++) {
  //code
}
while (/* condition */) {
  //code
}
do {
  //code
} while(/* condition */);
```

- swift
```swift
for var i = 0; i < 10; i++ {
  // code
}
for index in 1...5 {
    print(index)
}
while condition {
 // code
}
repeat {
 // code
} while condition
```
swift里0/1并不代表真假, 而且条件或者迭代并不需要括号围住

- ruby
```ruby
5.times {
  #code
}
1.upto(5) {
    puts "hello"
}
5.downto(1) {
    puts "hello"
}
#iteration of aray
for element in my_array
    puts element
end
# iteration 2
my_array.each { |e| puts e }
#while
x = 1
while x <= 5
    puts x
    x += 1
end
#while 2
x += 1 while x <= 5
#until
x = 1
until x > 5
    puts x
    x += 1
end
#until 2
x += 1 until x > 5
# ruby没有 do...while 循环，但是可以用其他语句代替
loop do
    puts "first run"
    break unless condition
end
```

- bash
```bash
#read list
for item in aa bb cc dd ee
do
    echo $item
done
#c style for loop
for (( a = 1, b = 10; a <= 10; a++, b-- ))
do
    echo $a
    echo $b
done
# while loop
var1=10
while [[ $var1 -gt 0 ]]; do
    echo $var1
    var1=$[$var1 - 1]
done
# until loop
var2=10
until [[ $var2 -lt 0 ]]; do
    echo $var2
    var2=$[$var2 - 1]
done
```
bash 也没有 c 中的 do...while 循环。
# 分支

- c
```c
//if else
if (legs == 4)
    printf("It might be a horse.\n", );
else if (legs > 4)
    printf("It is not a horse.\n", );
else {
    legs++;
    printf("Now it has one more leg.\n", );
}
//switch
switch (ch) {
    case 'a':
    case 'A': a_ct++;
              break;
    case 'e':
    case 'E': e_ct++;
              break;
    case 'i':
    case 'I': i_ct++;
              break;
    default: break;
}
```
C语言里 switch 中，case 必须是常量值，不能是表达式。因此如果需要匹配1-5之间的值，你需要一个个写 case。 C语言还保留了 goto 语句，但是尽量不用。

- swift
```swift
// if else
if True {
    print("True")
} else if False {
    print("False")
} else {
    print("never happen")
}
// guard
// guard永远带有一个 else 语句，使用 guard 比 if 更能提高我们代码的可靠性，他可以使代码连贯执行而不需要将它包在 else 语句中。
func greet(person: [String: String]) {
    guard let name = person["name"] else {
      return
    }
    print("Hello \(name)")
}
// switch
switch ch {
    case 'a', 'A':
      a_ct++
    case 'e', 'E':
      e_ct++
    case 'i', 'I':
      i_ct++
    default:
      print("others")
}
// switch range case
switch age {
    case 1...6:
      print("child")
    case 7...17:
      print("young")
    default:
      print("adult")
}
// switch: value bindings
switch age {
    case 1:
        print("you are 1 year old.")
    case let x:
        print("you are \(x) years old.")
}
// label
gameLoop: while square != finalSquare {
    switch result {
    case final_result:
        break gameLoop
    case // other code
    }
}
```
和 c 不同，swift 的 switch 语句不存在隐式的贯穿(即不用写 break 语句)，执行完一个分支 swift 就会 执行switch结构的下一条语句. 如果需要匹配多种条件，写在同一个 case 里即可，也可以手动 Fallthrough. swift 的 switch 的第二个不同是，case 还可以是区间值(1..5), 也可以是元组。第三个不同，switch 还可以进行值绑定。第四个不同，swift 要求 switch 的 case 必须要覆盖所有的情况, 而且每个 case 都需要至少一条可执行的语句(如果你想略过某种 case，用 break 作为该 case 的唯一语句即可)。

- ruby
```ruby
#if...elsif...else
if fruit == "orange"
    color = "orange"
elsif fruit == "apple"
    color = "green"
else
    color = "unknown"
end
# unless
unless age >= 18
    puts "you're too young to use this system."
    exit
end
# case
# 因为 ruby 的每条语句都有返回值，所以可以像以下这么用
color = case fruit
when "orange"
  "orange"
when "apple"
  "green"
else
  "unknown"
end
```
和 swift 类似，ruby 的 case 语句不会贯穿到接下来的分支。case 也支持范围匹配(when 1...6), 和 swift 不同的地方在于，ruby 不要求when 从句覆盖所有的取值可能。ruby 没有 goto 语句

- bash
```bash
# if-then-else
if cmd1
then
    cmd_set_1
elif cmd2
then
    cmd_set_2
else
    cmd_set_3
fi
# case
case $USER in
  rich | barbara)
    echo "Welcome. $USER"
    echo "Please enjoy your visit";;
  testing)
    echo "Special testing account";;
  *)
    echo "Sorry. you are not allowed here.";;
esac
```
bash没有 goto 语句，但是它的 break 和 continue 语句可以后接一个整数参数，以指定跳出/继续某层循环。
## 函数
- C
```c
int func(void) {
    printf("%s\n", "Hello world.");
    return 0;
}
```
- Swift
```swift
func sayHello(to person: String, and anotherPerson: String) -> String {
    return "Hello \(person) and \(anotherPerson)!"
}
print(sayHello(to: "Bill", and: "Ted"))
// Functions with Multiple Return Values
func minMax(array: [Int]) -> (min: Int, max: Int) {
    //code here
}
// Variadic Parameters
func arithmeticMean(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
      total += number
    }
    return total / Double(numbers.count)
}
// In-Out Parameters
func swapTwoInts(inout a: Int, inout _ b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
①swift和 c 相比，多了外部参数名/局部参数名的概念。外部参数名用于在函数调用时传递给函数的参数，局部参数名在函数的实现内部使用。一般情况下，第一个参数名省略其外部参数名，第二个及之后的参数使用其局部参数名作为外部参数名。如果不想为第二个及以后的参数设置外部参数名，用下划线代替一个明确的参数名。②swift 可以通过元组返回多个值。③swift 可以有可变参数，通过在变量类型后面假如...的方式来定义可变参数。④swift 的参数默认是常量参数，试图在函数体中更改参数值将会导致编译错误。通过在参数名前加关键字 var 来定义变量参数。⑤如果想要一个函数可以修改参数的值，并且想要在这些修改在函数调用结束后仍然存在，那么就应该把这个参数定义为输入输出参数。
- Ruby
```ruby
def say_hello(name)
    "Hello, " + name
end
```
ruby 里方法的最后一条语句的值会被自动当做返回值
- Bash
```bash
function addem {
    if [ $# -eq 0 ] || [ $# -gt 2 ]
    then
      echo -1
    elif [ $# -eq 1 ]
    then
      echo $[ $1 + $1 ]
    elif [ $# -eq 2 ]
    then
      echo $[ $1 + $2 ]
    fi
}
```
## 类
- Swift
```swift
class VideoMode {
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
```
- Ruby
```ruby
class Square < ParentClass
    def initialize(side_length)
        @side_length = side_length
        if defined?(@@number_of_squares)
            @@number_of_squares += 1
        else
            @@number_of_squares = 1
        end
    end

    def area
        @side_length * @side_length
    end

    def self.test_method
        puts "Hello from the Square class"
    end
    def test_method
        puts "Hello from an instance of class Square!"
    end

    private
    def this_is_private
        # code here
    end

    public
    def another_public_method
        # code here
    end
end
```
①ruby里在变量名前加上$/@/@@来分别表示全局变量/实例变量/类变量。②ruby 里用module解决命名冲突。③ruby 不能多重继承, 但是可以通过 include 模块来使用多个模块的方法。④如果需要使用 Enumerable 的方法，需要类实现 each 方法；如果需要使用Comparable 的方法，需要实现<=>方法.
## 错误处理/debug
- ruby
```ruby
# exception handle
begin
    puts "code here"
rescue ZeroDivisionError
    puts "code to rescue exception here"
rescue YourOwnException
    puts "code to rescue yourOwn exception"
rescue  => e
    puts e.class
end
# catch/throw
catch(:finish) do
    1000.times do
        x = rand(1000)
        throw(:finish) if x == 123
    end
    puts "Generated 1000 random numbers without generating 123!"
end
# unit test
require "minitest/autorun"
class String
    def titleize
        # self.gsub(/(\A|\s)\w/) { |letter| letter.upcase }
        self.gsub(/\b\w/) { |letter| letter.upcase }
    end
end
class TestTitleize < Minitest::Test
    def test_1
        assert_equal "This Is A Test", "this is a test".titleize
    end

    def test_2
        assert_equal  "Another Test 1234", "another test 1234".titleize
    end

    def test_3
        assert_equal  "We're Testing Titleize", "We're testing titleize".titleize
    end
end
# benchmark
require "benchmark"
iterations = 100000
b = Benchmark.measure do
    for i in 1..iterations do
      x = i
    end
end
c = Benchmark.measure do
    iterations.times do |i|
      x = i
    end
end
puts b
puts c
```
①catch/throw work with symbols rather than exceptions. They're designed to be used in situations where no error has occurred, but being able to escape quickly frome a nested loop, method call, or similar, is necessary. ②catch/throw don't have to be directly in the same scope. throw works from methods called from within a catch block. ③通过`ruby -r debug mycode.rb`进入 debug 模式
