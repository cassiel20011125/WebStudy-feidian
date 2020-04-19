## 第六章

1. 对象的Configurable、Enumerable、Writable、Value属性分别表示什么

   ```javascript
   [[configurable]] //能否通过delete删除属性从而重新定义属性，能否修改属性的特姓，或者能否把属性修改为访问器属性
   [[enumerable]] //能否通过for-in循环返回属性
   [[writable]] //表示能否修改属性的值
   [[value]] //包含这个属性的数据值
   ```

2. 解释下面代码的输出结果

``` javascript
var panda = {
    _name: 'panda',
    age: 20
}

Object.defineProperty(panda,"name",{
    get: function() {
        return this._name;
    },
    set: function(newVal) {
        if(newVal !== this._name) {
            this._name = newVal;
            console.log('该更新了')
        }
    }
})
console.log(panda.name);
panda.name = 'lc';
//先创建了一个拥有name、age两个属性的panda对象，然后定义了一个访问器属性name，包含用于返回name值的get函数和用于判定的set函数，接着通过对象的方法访问name属性并输出，下一个就是修改name属性值，与this指向的原来的name值不同，执行输出"该更新了"
```

[例] 创建一个拥有属性：name为panda，age为20；拥有方法：showMe（该函数打印该对象的两个属性）的对象。（3、4、5、7题的例子 ）

3. 使用工厂模式创建对象（怎么创建、优点、缺点）

   ```javascript
   //用函数来封装以特定接口创建对象，createPerson()函数
   //可以无数次调用，解决了创建多个相似对象的问题
   //每次都返回一个包含三属性一方法的对象，没有解决对象识别问题(怎么知道对象的类型)
   ```

   

4. 使用构造函数模式创建对象（怎么创建、优点、缺点）

   ```javascript
   //Person()函数
   //可以将实例标识为一种特定类型
   //没有封装性
   ```

5. 使用原型模式创建对象（怎么创建、优点、缺点）

   ```javascript
   //先创建一个函数，利用这个函数的prototype属性指向的原型对象，以原型对象为基础创建新的对象
   //可以让所有的对象实例共享它所包含的属性和方法，并且可以在实力上重新定义属性而不改变原型
   //由于共享，对于引用类型，在实例上进行修改，也会改变原型的值
   ```

   

6. 下面的代码输出什么
``` javascript
function Panda() {
}
Panda.prototype.name = 'panda';
var panda1 = new Panda();
console.log(panda1.hasOwnProperty('name'))//false
console.log('name' in panda1)//true
```

7. 使用组合构造函数模式和原型模式创建对象

   ```javascript
   function student(name, id, grade){
       this.name = name;
       this.id = id;
       this.grade = grade;
       this.subject = ["Math", "English"];
   }
   
   student.prototype = {
       constructor : student,
       sayGrade : function(){
           alert(this.grade);
       }
   }
   
   var student1 = new student("Wangdong", "18", "90");
   var student2 = new student("Cassiel", "18", "88");
   student1.subject.push("Chinese");
   alert(student1.subject); //Math, English
   alert(student2.subject); //Math, Endlish, Chinese
   alert(student1.subject == student2.subject); //false
   alert(student1.sayGrade == student2.sayGrade); //true
   ```

   

8. 理解原型链后说明下面的代码输出什么
``` javascript
function Bigbear() {
    this.bear = 'bear';
}

Bigbear.prototype.getBear = function() {
    return this.bear;
}

function Panda() {
    this.panda = 'panda';
}

Panda.prototype = new Bigbear();

Panda.prototype.getPanda = function() {
    return this.panda;
}

var animal = new Panda();
console.log(animal.getBear()); //bear
console.log(animal.getPanda()); //panda
```