4.1 프로토타입을 통한 객체지향
=============================
4.1.1 프로토타입이란 ?
-----------------------------
* 객체지향 개념 중에서도 '상속'을 구현하기 위한 '속성'(객체)

4.1.2 자바스크립트와 자바에서의 객체 생성
--------------------------------------
* 자바에서는 new 클래스명() vs 자바스크립트에서는 new Function명()
> 자바스크립트에서는 function 자체가 class + 생성자

<pre>
function Person(name, blog){
    this.name = name;
    this.blog = blog;
}

>> var obj = new Person('soojin', 'sBlog');
</pre>

4.1.3 this의 이해
--------------------
* this는 함수 호출 방법에 따라 달라진다.

1. 일반 함수로의 호출
2. 멤버함수로의 호출
3. call()함수를 이용한 호출
4. apply()함수를 이용한 호출

##### 1. 일반함수로의 호출
<pre>
function Say(something) {
	console.log(something);
}

Say("Hi"); // Window
</pre>

##### 2. 멤버함수로의 호출
<pre>
var obj = {
	say : function(something){  console.log(something); }
};

obj.say("Hi"); // obj
</pre>

##### 3,4 call(), apply()함수를 이용한 호출

<pre>
obj.say.call(obj, "Hi"); // obj
obj.say.apply(undefined, ["Hi"]); // Window

obj.say.call("Hi"); // obj

Say.call("Hi"); // Window
Say.apply("Hi"); // Window
</pre>

* 함수가 호출될 때는 내부적으로 call()함수로 변형되어서서 호출.
* call(), apply()의 경우 첫번째인자가 this.
> 첫번째 인자가 없다면 일반함수호출의 경우 null, 멤버함수 호출의 경우 해당 객체

4.1.6 프로토타입의 사용 예
-----------------------
* prototype : 다른 객체들과 공유되는 속성을 제공하는 객체

<pre>
function Person(name, blog) {
    this.name = name;
    this.blog = blog;
}

Person.prototype.getName = function(){
	return this.Name;
}

var objChild = new Person('soojin', 'Sblog');
console.log(objChild.getName()); // soojin  // 생성자를 통해 생성 된 객체들이 prototype을 공유한다.
</pre>

* prototype은 덧붙여도 실행이 가능하다

<pre>

Person.prototype.gender = "male";

objChild.gender = "male";

</pre>

4.1.7 프로토타입과 생성자
-----------------------------
* new 키워드로 생성자를 생성할 때, 생성자 역할을 하는 함수(Person)과 생성자 역할을 하는 함수의 proto객체가 생성되고,
그 둘은 서로 순환참조한다.

![Alt text](..\프로토타입1.PNG)
![Alt text](https://raw.githubusercontent.com/Laon63/Study_Javascript/%EC%9D%B4%EC%88%98%EC%A7%84/%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85.png)

* new로 생성된 새 객체와의 참조관계
![Alt text](D:\Study_Javascript-master\Study_Javascript-master\이수진\프로토타입과뉴오브젝트.PNG)
> 새 객체의 실제 부모는 생성자 함수 객체의 proto 객체이다.


<pre>

Person.prototype.gender = "male";

objChild.gender = "male";

objChild.gender = "female";

console.log(Person.prototype.gender); //male
console.log(objCild.gender); // female
</pre>

* prototype으로 상속받은 속성값은 각 객체에 추가되어 저장되기 때문에 해당 객체의 값을 변경한다고 하더라도, proto에서 참조하던 값이 변경되진 않는다.

![Alt text](D:\Study_Javascript-master\Study_Javascript-master\이수진\프로토타입과뉴오브젝트2.PNG)
> 이유는, 새로 생성된 객체가 proto를 내부링크로 참조하고있고, objCild객체에 gender라는 속성이 추가되어 저장되기 때문!!

* 부모의 속성에 접근하기 위해서는 constructor 로 접근한다.

<pre>

objChild.constructor.prototype.gender = "female";
console.log(Person.prototype.gender); //female
console.log(objCild.gender); // female
</pre>



4.1.8 객체 내의 속성 탐색 순서
-----------------------------
* 프로토타입 체인 : 자바스크립트의 모든 객체는 프로토타입이라는 다른 객체를 가르키는 내부링크를 가지고, null을 프로토타입으로 가지는 객체에서 끝난다.

* 먼저 객체 자체의 속성에서 찾은 다음, 프로토타입에 저장된 속성들을 검사하고, 모두 없으면 undefined.

<pre>
function Car(){
    this.wheel  = 4;
    this.beep = "BEEP";
}
Car.prototype.go = function(){
	alert(this.beep);
}

function Truck(){
	this.wheel = 6;
    this.beep = "Hi";
}

Truck.prototype = new Car();
console.log(Truck.wheel); // 6
console.log(Truck.go()); //"Hi";
</pre>

* hasOwnProperty // 현재 객체에 포함된 것

<pre>

var prop;

for(prop in Truck){
	if(Truck.hasOwnProperty(prop))
    	console.log(Truck[prop]); //6,"Hi"
}

</pre>

4.1.9 프로토타입의 장단점
-----------------------------

##### 장점
> 모든 객체가 하나의 객체를 공유하고 있어서 메모리를 줄일 수 있다.

##### 단점
> 이해하기가 어렵고, 복잡하다.