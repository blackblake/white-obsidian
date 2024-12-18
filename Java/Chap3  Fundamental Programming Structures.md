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

### 5）E.g. 猜数字

```java
int magic = (int)(Math.random()*101)+100;
      Scanner sc = new Scanner(System.in);
      System.out.print("请输入你猜的数：");
      int guess = sc.nextInt(); 
      while(guess != magic){
          if(guess > magic)
                 System.out.print("错误!太大，请重猜：");
         else
           System.out.print("错误!太小，请重猜：");
        guess = sc.nextInt();
      }
     System.out.println("恭喜你，答对了！\n该数是："+magic);//这个输出注意一下
```

### 6）for循环

1. 条件语句可以使用**多个变量**：
```java
for(int i = 0, j = 10 ; i < j ; i++, j--) {...}
```
2. for循环中的一部分或全部**可为空**，循环体也可为空，但 **分号不能省略**
```java
for ( ;  ; ){
    // 这实际是一个无限循环
}
```


### 7）break
break语句是用来跳出while、do、for或switch结构的执行，该语句有两种格式：
    `break;  //只能跳出当前循环`
	`break label; //多用于跳出多重循环`
![[Pasted image 20241218021103.png|300]]

>PS：continue也有 continue label; 的用法

### 8）