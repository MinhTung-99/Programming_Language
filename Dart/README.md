# vriable, string, array
```dart
String s = "Hello";
int x = 10;
var y = "abc";

List<int> numbers = [1,2,3,4,5];
for (int i = 0; i < numbers.length; i++) {
    print(numbers[i]);
}
numbers.forEach((element) {
print(element);
});
```

# class

```dart
class Car{
  String name;
  int yearOfProduction;
  String _code; //private

  // Car(String name, int yearOfProduction){
  //   this.name = name;
  //   this.yearOfProduction = yearOfProduction;
  // }

  //Car(this.name, this.yearOfProduction);

  Car({
    @required this.name, //@required = not null(warning)
    this.yearOfProduction = 2020 //default parameter
  });

  void doingSomething(String message){
    print("I am doing $message");
    this.handleEvent();//must have it here to run this function in main
  }

  void sayHello({String name}){
    print("Hello $name");
  }

  //function as a variable
  Function handleEvent;

  @override
  String toString() {
    // TODO: implement toString
    return "${this.name} - ${this.yearOfProduction}";
  }
}

void main() {
  var car = Car(name: "Mercedes-Maybach S-Class Saloon", yearOfProduction: 2020);
  car.handleEvent = (){
    print("Handle in main");
  };
  car.doingSomething("my homework");
  car.sayHello(name: "Teo");

  runApp(
      Center(
        child: Text("${car.toString()}", textDirection: TextDirection.ltr,),
      )
  );
}
```