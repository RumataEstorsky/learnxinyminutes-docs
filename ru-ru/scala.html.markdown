---
language: Scala
filename: learnscala.scala
contributors:
    - ["George Petrov", "http://github.com/petrovg"]
    - ["Dominic Bou-Samra", "http://dbousamra.github.com"]
    - ["Geoff Liu", "http://geoffliu.me"]
    - ["Ha-Duong Nguyen", "http://reference-error.org"]
    - ["Rumata Estorsky", "https://github.com/RumataEstorsky"]
filename: learn.scala
---

Scala - the SCAlable LAnguage (масштабируемый язык)

```scala

/*
  Разворачивание на своей системе:

  1) Скачайте компилятор Scala - http://www.scala-lang.org/downloads
  2) Распакуйте в предпочитаемое место на диске и поместите подкаталог "bin" в переменную окружения ОС `PATH`
  3) Запустите Scala REPL с помощью комманды `scala`. Вы должны увидеть приглашение:

  scala>

  Эта оболочка называется REPL (Read-Eval-Print Loop - простая интерактивная среда программирования).
  Вы можете исполнять здесь любые выражения на языке Scala, результат будет сразу выводиться на экран.
  Чуть позже мы расскажем о Scala файлах, а сейчас давайте начнём с простых основ. 
*/


/////////////////////////////////////////////////
// 1. Основы
/////////////////////////////////////////////////

// Строковый комментарий начинается с двух слеш-символов 

/*
  Блоковые (многострочные) комментарии, как вы видели выше, можете увидеть прямо здесь.
*/

// Вывод данных с обязательным переходом на новую строку
println("Hello world!")
println(10)

// Вывод без перехода на новую строку
print("Hello world")

// Обьявление значений производится с помощью "var" или "val".
// Переменные "val" становятся иммутабельными (неизвеняемыми), тогда как "var" становятся мутабельными (изменяемыми).
// В большинстве случаев иммутабельные переменные лучше.
val x = 10 // x теперь хранит 10
x = 20     // error: reassignment to val (ошибка: присвоение иммутабельной переменной)
var y = 10
y = 20     // y теперь 20

/*
  Scala - статически-типизированный язык, и еще одно замечание по поводу объявлений,
  вы можете не обьявлять тип. Это возможно благодаря специальной возможности языка,
  называемой выводом типов (type inference). В большинстве случаев компилятор Scala
  может сам определить требуемый тип данных. И так, вы можете опускать тип данных.
  Но всегда остаётся возможность указать тип явно:
*/
val z: Int = 10
val a: Double = 1.0

// Обратите внимание, здесь просиходит автоматическое приведение из Int к Double, в результате будет 10.0, а не 10
val b: Double = 10

// Булевы значения
true
false

// Булевы операторы
!true         // false
!false        // true
true == false // false
10 > 5        // true

// Ну, по обыкновению, арифметика
1 + 1   // 2
2 - 1   // 1
5 * 3   // 15
6 / 2   // 3
6 / 4   // 1
6.0 / 4 // 1.5


// После вычислений REPL вернёт вам тип и значение результата

1 + 7

/* Строка выше вернёт следующий результат:

  scala> 1 + 7
  res29: Int = 8

  Это означает, что результатом вычислений 1 + 7 будет объект типа Int
  со значением 8

  Заметьте, что имя переменной "res29" последовательно генерируется для сохранения результатов выражения,
  которые вы ввели, в вашем случае оно будет отличаться.
*/

"Строки в Scala объявляются в двойных кавычках"
'a' // Символ в языке Scala 
// 'Одиночные кавычки недопустимы в строках Scala' <= Это приведёт к ошибке

// Строки имеют стандартные методы как в строки Java
"hello world".length
"hello world".substring(2, 6)
"hello world".replace("C", "3")

// Но также некоторые дополнительные, специфичные для Scala методы. См. также: scala.collection.immutable.StringOps
"hello world".take(5)
"hello world".drop(5)

// Строковая интерполяция (string interpolation): notice the prefix "s"
val n = 45
s"У Валеры есть $n яблок" // => "У Валеры есть 45 яблок"

// Внутри интерполированных строк также можно  размещать выражения
val a = Array(11, 9, 6)
s"Моей средней дочери ${a(0) - a(2)} лет."               // => "Моей средней дочери 5 лет."
s"А у Лизы всего ${n / 3.0} яблок."                      // => "А у Лизы всего 15 яблок."
s"Квадрат двойки: ${math.pow(2, 2)}"                     // => "Квадрат двойки: 4"

// Можно форматировать интерполируемые строки, указывая префикс "f"
f"Квадрат 5: ${math.pow(5, 2)}%1.0f"                     // "Квадрат 5: 25"
f"Квадратный корень из 122: ${math.sqrt(122)}%1.4f"      // "Квадратный корень из 122: 11.0454"

// Необработанные строки, специальные символы игнорируются.
raw"New line feed: \n. Carriage return: \r." // => "New line feed: \n. Carriage return: \r."

// Некоторые символы нужно "экранировать", например двойные кавычки внутри строки:
"Они стояли около \"Розы и Короны\"" // => "Они стояли около "Розы и Короны""

// Три символа двойных  кавычек позволяет определять многострочные строковые константы,
// которые могут содержать символы двойных кавычек (которые не нуждаются в экранировании)
val html = """<form id="daform">
                <p>Press belo', Joe</p>
                <input type="submit">
              </form>"""


/////////////////////////////////////////////////
// 2. Функции
/////////////////////////////////////////////////

// Функции определяются так:
//
//   def functionName(args...): ReturnType = { body... }
//
// Если вы пришли из традиционных языков, вероятно вам бросилось в глаза, что ключевое слово
// return можно опустить. В Scala, последнее выражение в теле функции и есть возвращаемое значение. 
def sumOfSquares(x: Int, y: Int): Int = {
  val x2 = x * x
  val y2 = y * y
  x2 + y2
}

// Даже фигурные скобки { } можно опускать, если тело функции это одно выражение:
def sumOfSquaresShort(x: Int, y: Int): Int = x * x + y * y

// Синтаксис вызова функции тривиален:
sumOfSquares(3, 4)  // => 25

// В большинстве случаев (рекурсивные функции - наиболее заметное исключение), возвращаемый функцией
// тип данных можно не указывать, как мы это уже видели с переменными:
def sq(x: Int) = x * x  // Компилятор сам выдедет, что возвращаемый тип - Int

// Функции также могут иметь значения по-умолчанию:
def addWithDefault(x: Int, y: Int = 5) = x + y
addWithDefault(1, 2) // => 3
addWithDefault(1)    // => 6


// Анонимные функции (anonymous functions) выглядят так:
(x: Int) => x * x

// В отличие от стандартных функций, даже входные параметры анонимных функций могут быть опущены,
// если это понятно из контекста. Запись типа "Int => Int" означает что это анонимная функция получает
// на вход аргумент типа Int и возвращает Int.
val sq: Int => Int = x => x * x

// Анонимные функции можно вызвать как и обычные:
sq(10)   // => 100

// Если каждый аргумент в вашей анонимной функции используется тлишь однажды,
// Scala предоставляет вам более краткую запись. These
// anonymous functions turn out to be extremely common, as will be obvious in
// the data structure section.
val addOne: Int => Int = _ + 1
val weirdSum: (Int, Int) => Int = (_ * 2 + _ * 3)

addOne(5)      // => 6
weirdSum(2, 4) // => 16


// The return keyword exists in Scala, but it only returns from the inner-most
// def that surrounds it.
// WARNING: Using return in Scala is error-prone and should be avoided.
// It has no effect on anonymous functions. For example:
def foo(x: Int): Int = {
  val anonFunc: Int => Int = { z =>
    if (z > 5)
      return z // This line makes z the return value of foo!
    else
      z + 2    // This line is the return value of anonFunc
  }
  anonFunc(x)  // This line is the return value of foo
}


/////////////////////////////////////////////////
// 3. Управление выполнением
/////////////////////////////////////////////////

1 to 5
val r = 1 to 5
r.foreach(println)

r foreach println
// NB: Scala is quite lenient when it comes to dots and brackets - study the
// rules separately. This helps write DSLs and APIs that read like English

(5 to 1 by -1) foreach (println)

// A while loops
var i = 0
while (i < 10) { println("i " + i); i += 1 }

while (i < 10) { println("i " + i); i += 1 }   // Yes, again. What happened? Why?

i    // Show the value of i. Note that while is a loop in the classical sense -
     // it executes sequentially while changing the loop variable. while is very
     // fast, faster that Java loops, but using the combinators and
     // comprehensions above is easier to understand and parallelize

// A do while loop
do {
  println("x is still less than 10")
  x += 1
} while (x < 10)

// Tail recursion is an idiomatic way of doing recurring things in Scala.
// Recursive functions need an explicit return type, the compiler can't infer it.
// Here it's Unit.
def showNumbersInRange(a: Int, b: Int): Unit = {
  print(a)
  if (a < b)
    showNumbersInRange(a + 1, b)
}
showNumbersInRange(1, 14)


// Conditionals

val x = 10

if (x == 1) println("yeah")
if (x == 10) println("yeah")
if (x == 11) println("yeah")
if (x == 11) println ("yeah") else println("nay")

println(if (x == 10) "yeah" else "nope")
val text = if (x == 10) "yeah" else "nope"


/////////////////////////////////////////////////
// 4. Структуры данных
/////////////////////////////////////////////////

val a = Array(1, 2, 3, 5, 8, 13)
a(0)
a(3)
a(21)    // Throws an exception

val m = Map("fork" -> "tenedor", "spoon" -> "cuchara", "knife" -> "cuchillo")
m("fork")
m("spoon")
m("bottle")       // Throws an exception

val safeM = m.withDefaultValue("no lo se")
safeM("bottle")

val s = Set(1, 3, 7)
s(0)
s(1)

/* Look up the documentation of map here -
 * http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.Map
 * and make sure you can read it
 */


// Tuples

(1, 2)

(4, 3, 2)

(1, 2, "three")

(a, 2, "three")

// Why have this?
val divideInts = (x: Int, y: Int) => (x / y, x % y)

divideInts(10, 3) // The function divideInts gives you the result and the remainder

// To access the elements of a tuple, use _._n where n is the 1-based index of
// the element
val d = divideInts(10, 3)

d._1

d._2


/////////////////////////////////////////////////
// 5. Объектно-ориентированное программирование
/////////////////////////////////////////////////

/*
  Aside: Everything we've done so far in this tutorial has been simple
  expressions (values, functions, etc). These expressions are fine to type into
  the command-line interpreter for quick tests, but they cannot exist by
  themselves in a Scala file. For example, you cannot have just "val x = 5" in
  a Scala file. Instead, the only top-level constructs allowed in Scala are:

  - objects
  - classes
  - case classes
  - traits

  And now we will explain what these are.
*/

// classes are similar to classes in other languages. Constructor arguments are
// declared after the class name, and initialization is done in the class body.
class Dog(br: String) {
  // Constructor code here
  var breed: String = br

  // Define a method called bark, returning a String
  def bark = "Woof, woof!"

  // Values and methods are assumed public. "protected" and "private" keywords
  // are also available.
  private def sleep(hours: Int) =
    println(s"I'm sleeping for $hours hours")

  // Abstract methods are simply methods with no body. If we uncomment the next
  // line, class Dog would need to be declared abstract
  //   abstract class Dog(...) { ... }
  // def chaseAfter(what: String): String
}

val mydog = new Dog("greyhound")
println(mydog.breed) // => "greyhound"
println(mydog.bark)  // => "Woof, woof!"


// The "object" keyword creates a type AND a singleton instance of it. It is
// common for Scala classes to have a "companion object", where the per-instance
// behavior is captured in the classes themselves, but behavior related to all
// instance of that class go in objects. The difference is similar to class
// methods vs static methods in other languages. Note that objects and classes
// can have the same name.
object Dog {
  def allKnownBreeds = List("pitbull", "shepherd", "retriever")
  def createDog(breed: String) = new Dog(breed)
}


// Case classes are classes that have extra functionality built in. A common
// question for Scala beginners is when to use classes and when to use case
// classes. The line is quite fuzzy, but in general, classes tend to focus on
// encapsulation, polymorphism, and behavior. The values in these classes tend
// to be private, and only methods are exposed. The primary purpose of case
// classes is to hold immutable data. They often have few methods, and the
// methods rarely have side-effects.
case class Person(name: String, phoneNumber: String)

// Create a new instance. Note cases classes don't need "new"
val george = Person("George", "1234")
val kate = Person("Kate", "4567")

// With case classes, you get a few perks for free, like getters:
george.phoneNumber  // => "1234"

// Per field equality (no need to override .equals)
Person("George", "1234") == Person("Kate", "1236")  // => false

// Easy way to copy
// otherGeorge == Person("george", "9876")
val otherGeorge = george.copy(phoneNumber = "9876")

// And many others. Case classes also get pattern matching for free, see below.


// Traits coming soon!


/////////////////////////////////////////////////
// 6. Сопоставление с образцом (Pattern Matching)
/////////////////////////////////////////////////

// Pattern matching is a powerful and commonly used feature in Scala. Here's how
// you pattern match a case class. NB: Unlike other languages, Scala cases do
// not need breaks, fall-through does not happen.

def matchPerson(person: Person): String = person match {
  // Then you specify the patterns:
  case Person("George", number) => "We found George! His number is " + number
  case Person("Kate", number)   => "We found Kate! Her number is " + number
  case Person(name, number)     => "We matched someone : " + name + ", phone : " + number
}

val email = "(.*)@(.*)".r  // Define a regex for the next example.

// Pattern matching might look familiar to the switch statements in the C family
// of languages, but this is much more powerful. In Scala, you can match much
// more:
def matchEverything(obj: Any): String = obj match {
  // You can match values:
  case "Hello world" => "Got the string Hello world"

  // You can match by type:
  case x: Double => "Got a Double: " + x

  // You can specify conditions:
  case x: Int if x > 10000 => "Got a pretty big number!"

  // You can match case classes as before:
  case Person(name, number) => s"Got contact info for $name!"

  // You can match regular expressions:
  case email(name, domain) => s"Got email address $name@$domain"

  // You can match tuples:
  case (a: Int, b: Double, c: String) => s"Got a tuple: $a, $b, $c"

  // You can match data structures:
  case List(1, b, c) => s"Got a list with three elements and starts with 1: 1, $b, $c"

  // You can nest patterns:
  case List(List((1, 2, "YAY"))) => "Got a list of list of tuple"
}

// In fact, you can pattern match any object with an "unapply" method. This
// feature is so powerful that Scala lets you define whole functions as
// patterns:
val patternFunc: Person => String = {
  case Person("George", number) => s"George's number: $number"
  case Person(name, number) => s"Random person's number: $number"
}


/////////////////////////////////////////////////
// 7. Функциональное программирование 
/////////////////////////////////////////////////

// Scala allows methods and functions to return, or take as parameters, other
// functions or methods.

val add10: Int => Int = _ + 10 // A function taking an Int and returning an Int
List(1, 2, 3) map add10 // List(11, 12, 13) - add10 is applied to each element

// Anonymous functions can be used instead of named functions:
List(1, 2, 3) map (x => x + 10)

// And the underscore symbol, can be used if there is just one argument to the
// anonymous function. It gets bound as the variable
List(1, 2, 3) map (_ + 10)

// If the anonymous block AND the function you are applying both take one
// argument, you can even omit the underscore
List("Dom", "Bob", "Natalia") foreach println


// Combinators

s.map(sq)

val sSquared = s. map(sq)

sSquared.filter(_ < 10)

sSquared.reduce (_+_)

// The filter function takes a predicate (a function from A -> Boolean) and
// selects all elements which satisfy the predicate
List(1, 2, 3) filter (_ > 2) // List(3)
case class Person(name: String, age: Int)
List(
  Person(name = "Dom", age = 23),
  Person(name = "Bob", age = 30)
).filter(_.age > 25) // List(Person("Bob", 30))


// Scala a foreach method defined on certain collections that takes a type
// returning Unit (a void method)
val aListOfNumbers = List(1, 2, 3, 4, 10, 20, 100)
aListOfNumbers foreach (x => println(x))
aListOfNumbers foreach println

// For comprehensions

for { n <- s } yield sq(n)

val nSquared2 = for { n <- s } yield sq(n)

for { n <- nSquared2 if n < 10 } yield n

for { n <- s; nSquared = n * n if nSquared < 10} yield nSquared

/* NB Those were not for loops. The semantics of a for loop is 'repeat', whereas
   a for-comprehension defines a relationship between two sets of data. */


/////////////////////////////////////////////////
// 8. Неявные параметры и преобразования (Implicits)
/////////////////////////////////////////////////

/* WARNING WARNING: Implicits are a set of powerful features of Scala, and
 * therefore it is easy to abuse them. Beginners to Scala should resist the
 * temptation to use them until they understand not only how they work, but also
 * best practices around them. We only include this section in the tutorial
 * because they are so commonplace in Scala libraries that it is impossible to
 * do anything meaningful without using a library that has implicits. This is
 * meant for you to understand and work with implicts, not declare your own.
 */

// Any value (vals, functions, objects, etc) can be declared to be implicit by
// using the, you guessed it, "implicit" keyword. Note we are using the Dog
// class from section 5 in these examples.
implicit val myImplicitInt = 100
implicit def myImplicitFunction(breed: String) = new Dog("Golden " + breed)

// By itself, implicit keyword doesn't change the behavior of the value, so
// above values can be used as usual.
myImplicitInt + 2                   // => 102
myImplicitFunction("Pitbull").breed // => "Golden Pitbull"

// The difference is that these values are now eligible to be used when another
// piece of code "needs" an implicit value. One such situation is implicit
// function arguments:
def sendGreetings(toWhom: String)(implicit howMany: Int) =
  s"Hello $toWhom, $howMany blessings to you and yours!"

// If we supply a value for "howMany", the function behaves as usual
sendGreetings("John")(1000)  // => "Hello John, 1000 blessings to you and yours!"

// But if we omit the implicit parameter, an implicit value of the same type is
// used, in this case, "myImplicitInt":
sendGreetings("Jane")  // => "Hello Jane, 100 blessings to you and yours!"

// Implicit function parameters enable us to simulate type classes in other
// functional languages. It is so often used that it gets its own shorthand. The
// following two lines mean the same thing:
def foo[T](implicit c: C[T]) = ...
def foo[T : C] = ...


// Another situation in which the compiler looks for an implicit is if you have
//   obj.method(...)
// but "obj" doesn't have "method" as a method. In this case, if there is an
// implicit conversion of type A => B, where A is the type of obj, and B has a
// method called "method", that conversion is applied. So having
// myImplicitFunction above in scope, we can say:
"Retriever".breed // => "Golden Retriever"
"Sheperd".bark    // => "Woof, woof!"

// Here the String is first converted to Dog using our function above, and then
// the appropriate method is called. This is an extremely powerful feature, but
// again, it is not to be used lightly. In fact, when you defined the implicit
// function above, your compiler should have given you a warning, that you
// shouldn't do this unless you really know what you're doing.


/////////////////////////////////////////////////
// 9. Остальное
/////////////////////////////////////////////////

// Importing things
import scala.collection.immutable.List

// Import all "sub packages"
import scala.collection.immutable._

// Import multiple classes in one statement
import scala.collection.immutable.{List, Map}

// Rename an import using '=>'
import scala.collection.immutable.{List => ImmutableList}

// Import all classes, except some. The following excludes Map and Set:
import scala.collection.immutable.{Map => _, Set => _, _}

// Your programs entry point is defined in an scala file using an object, with a
// single method, main:
object Application {
  def main(args: Array[String]): Unit = {
    // stuff goes here.
  }
}

// Files can contain multiple classes and objects. Compile with scalac




// Input and output

// To read a file line by line
import scala.io.Source
for(line <- Source.fromFile("myfile.txt").getLines())
  println(line)

// To write a file use Java's PrintWriter
val writer = new PrintWriter("myfile.txt")
writer.write("Writing line for line" + util.Properties.lineSeparator)
writer.write("Another line here" + util.Properties.lineSeparator)
writer.close()

```

## Further resources

* [Scala for the impatient](http://horstmann.com/scala/)
* [Twitter Scala school](http://twitter.github.io/scala_school/)
* [The scala documentation](http://docs.scala-lang.org/)
* [Try Scala in your browser](http://scalatutorials.com/tour/)
* Join the [Scala user group](https://groups.google.com/forum/#!forum/scala-user)
