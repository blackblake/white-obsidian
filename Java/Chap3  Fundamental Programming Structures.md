### 1）A simple Java program

```java
public class FirstSample

{

  public static void main(String[] args)

  {

    System.out.println("We will not use 'Hello, World!'");  }

}
```

- 命名规则：类名的每个单词首字母都要大写
	
-  源代码的文件名必须与public类的类名相同 

### 2）输入
```cpp
Scanner input = new Scanner(System.in);

int a = input.nextInt();//也可以是nextDouble...
```

### 3）switch
- 与c++不同的是把`case :`换成了`case->`，而且不需要`break`;
	
- 新用法：通过switch结构返回一个值，并将该值赋给变量：
```cpp
int numLetters = switch(day){

       case MONDAY, FRIDAY, SUNDAY -> 6;

       case TUESDAY  -> 7;

       case THURSDAY, SATURDAY  -> 8;

       case WEDNESDAY   -> 9;

};
```


### 4）随机数生成

- 使用`Math.random()`方法，它返回一个0.0~1.0（不包括）之间的浮点数；
	
-  要得到10~99之间的整数，可以使用下面的表达式：
     `int number1 = 10 + (int)(Math.random()*90);`
	
-  生成随机的加或减运算
	也可以通过产生2个随机数（比如，0和1，0表示加法，1表示减法）确定：
       `int operator =  (int)(Math.random()*2);`

### 5）学到第41张