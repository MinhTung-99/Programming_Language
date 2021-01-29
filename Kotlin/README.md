# Inheritance
```kotlin
open class PerSon(
    open var name: String, var age: Int
){ //have to open to other class inheritance this class

    //used when object is be initialization
    init {
        println("I am person")
    }

    open fun talk(message: String){
        println(message)
    }
}

class Teacher(
    name: String, age: Int, var code: String
): PerSon(name, age){
    override var name = super.name.toUpperCase()

    override fun talk(message: String) {
        super.talk(message)
        println("I am a teacher")
    }
}

fun main() {
    val teacher = Teacher("Hang", 30, "12345")
    println("name = ${teacher.name}")
    teacher.talk("Hello")
}
```

# Interface
```kotlin
interface IAnimal{
    fun walk(){
        println("I can walk")
    }

    fun eat(){
        println("Animal can eat")
    }
}

interface IBird{
    fun fly(){
        println("I can fly")
    }

    fun eat(){
        println("Bird can eat")
    }
}

class Bat : IAnimal, IBird{
    override fun fly() {
        super.fly()
        println("I am bat")
    }

    override fun walk() {
        super.walk()
        println("I am bat")
    }

    override fun eat() {
        super<IAnimal>.eat()
        super<IBird>.eat()
    }
}

fun main() {
    val b = Bat()
    b.fly()
    b.walk()
    b.eat()
}
```

# Extensions
```kotlin
fun MutableList<String>.swap2Element(index1: Int, index2: Int){
    val temp = this[index1]
    this[index1] = this[index2]
    this[index2] = temp
}

class Caculation{
    companion object{
        fun PI(): Float = 3.14F //Float must have F => 3.14F
    }
}
fun Caculation.Companion.doublePI(): Float{
    return 2 * PI()
}

class A{
    fun methodX(){
        println("method X of class A")
    }

    private fun B.methodB2(){
        methodB()
        this.methodX()
        this@A.methodX()
    }
    fun callB2FromA(b: B){
        b.methodB2()
    }
}

class B{
    fun methodB(){
        println("method B of class B")
    }
    fun methodX(){
        println("method X oof class B")
    }
}

fun main() {
    val listOfString: MutableList<String> = mutableListOf("one","two","three")
    listOfString.swap2Element(0,2)
    println(listOfString)

    println(Caculation.doublePI())

    val b = B()
    val a = A()
    a.callB2FromA(b)
}
```

# Data, Nested, Inner
``` kotlin
data class User(var name: String, var age: Int)

class Fruit{
    private var apple = "apple"

    class Nested{ //can't call properties
        fun say(){
            println("I am a function inside a NESTED class")
        }
    }

    inner class Inner{
        fun getApple(){
            println("I am a function inside a INNER class. apple = $apple")
        }
    }
}

fun main() {
    val user = User("Teo", 20)
    println(user.toString()) //User(name=Teo, age=20)
    val backupUser = user.copy()
    println(backupUser) //User(name=Teo, age=20)
    if(user == backupUser){ //HERE
        println("IDENTICAL")
    }else{
        println("NOT IDENTICAL")
    }

    Fruit.Nested().say()
    val fruit = Fruit()
    fruit.Inner().getApple()
}
```

# ENUM
```kotlin
enum class CompassDirection{
    EAST,
    WEST,
    SOUTH,
    NORTH
}

enum class Color(
    val red: Int, val green: Int, val blue: Int
){
    RED(255,0,0),
    GREEN(0,255,0),
    BLUE(0,0,255)
}

enum class ActionState{
    ACTIVE{
        override fun reverseAction(): ActionState = INACTIVE
    },

    INACTIVE{
        override fun reverseAction(): ActionState = ACTIVE
    };

    abstract fun reverseAction(): ActionState
}

fun main() {
    println("North enum = ${CompassDirection.NORTH}")
    println(Color.BLUE.toString())
    println("Color details.Name = ${Color.BLUE.name}, oridinal: ${Color.GREEN.ordinal}")
    for(colorValue in Color.values()){
        println(colorValue)
    }

    println(ActionState.ACTIVE.reverseAction())
    println(ActionState.INACTIVE.reverseAction())
}
```

# Delegation
```kotlin
//Delegation: this class is authorization for other class

interface IBird{
    fun flyAndFindingFood()
}

class Eagle(val age: Int): IBird{
    override fun flyAndFindingFood() {
        println("I am an eagle. I am $age years old. I am flying and finding food")
    }
}

class Cuckoo(b: IBird): IBird by b //class Eagle is authorization for class Cuckoo

fun main() {
    val eagle = Eagle(2)
    val cuckoo = Cuckoo(eagle)
    cuckoo.flyAndFindingFood()
}
```

# Delegated properties
```kotlin
class User{
    var todayTask: String by DelegateUser()
}

class DelegateUser{
    operator fun getValue(user: User, property: KProperty<*>): String {
        return "$user, thanks for delegating ${property.name} to me"
    }

    operator fun setValue(user: User, property: KProperty<*>, value: String) {
        println("assigned to ${property.name} in $user. Dtails: $value")
    }

}

fun main() {
    var user = User()
    println(user.todayTask)
    user.todayTask = "work"
}
```

# Lambda, anonymous
```kotlin
fun sum(x: Int, y: Int): Int{
    return x + y
}

//lambda functions
val sum1 = {x: Int, y: Int -> x + y}
val sum2: (Int, Int) -> Int = {x,y -> x+y}

//Anonymous Functions
var sum = fun(x: Int, y: Int): Int{
    return x + y
}
var sayHello = fun(){
    println("Hello")
}

var ints = intArrayOf(1,2,3,4,5)
ints.forEach { eachNumber -> println(eachNumber) }
```

# Higher-Order Functions
```kotlin
//Higher-Order Functions: lấy function như một parameters hoặc return một fuction
class Lock {
    fun lock() {
        println("I locked the process")
    }

    fun unlock(){
        println("I unlocked the process")
    }
}
var bodyFunction = fun(): Int{
    val taskId = Random().nextInt()
    println("This is the body function. TaskId = $taskId")
    return taskId
}
fun doTask(lock: Lock, body: () -> Int): Int{
    lock.lock()
    try{
        return body()
    }
    finally {
        lock.unlock()
    }
}

val compare: (Int, Int) -> Boolean = {x, y -> x < y}
fun getMaxValueInCollection(collection: Collection<Int>, less: (Int, Int) -> Boolean): Int?{
    var maxValue: Int? = null
    for(item in  collection)
        if(maxValue == null || less(maxValue, item))
            maxValue = item
    return maxValue
}

fun main() {
    doTask(Lock(), bodyFunction) //I locked the process
                                //This is the body function. TaskId = 789472525
                               //I unlocked the process

    var ints: Collection<Int> = listOf(1,5,3,4,6)
    var maxValue = getMaxValueInCollection(ints, compare)
    println("maxValue = $maxValue")//maxValue = 6
}
```

# Destructuring declarations
```kotlin
//Destructuring declarations only work with data class
data class Customer(var name: String, var age: Int)

//Function that return multiple values
data class Result(val x: Int, val y: Int)
fun vector(): Result{
    return Result(1,2)
}

fun main() {
    val customer = Customer("TEO", 20)
    var(name, age) = customer
    println("Name = $name, age = $age")

    val customer2 = Customer("TY", 30)
    val customers = listOf<Customer>(customer, customer2)
    for ((name, age) in customers){
        println("Name = $name, age = $age")
    }

    var(x1, y1) = vector()
    println("x = $x1, y = $y1")

    //map
    var userObject = mapOf<String, Any>("name" to "Tung", "age" to 22)
    for((key, value) in userObject){
        println("key = $key, value = $value")
    }
}
```

# Ranges
```kotlin
fun main() {
    for(i in 1..10)
        print("$i ") //1 2 3 4 5 6 7 8 9 10
    for(i in 10 downTo 1) //10 9 8 7 6 5 4 3 2 1
        print("$i ")
    for(i in 1 until 10)
        print("$i ") //1 2 3 4 5 6 7 8 9

    val ints = listOf<Int>(10,20,30,40)
    if(30 in ints){
        println("this number exist in the list")
    }
    if(30 !in ints){
        println("this number not exist in the list")
    }

    //step
    for(i in 0..10 step 2)
        print("$i ") //0 2 4 6 8 10
    for(i in 10 downTo 0 step 2)
        print("$i ") //10 8 6 4 2 0
}
```