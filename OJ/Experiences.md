1. **（1）如果用IDE写代码的时候，本来应该有提示（即可以用tab打出来）的变量/对象等，如果没有提示了，一定要当心，基本上是出错了.**
```java

public LinkedListDeque() {  
    private Node sentinel=new Node(null,10,null);  
    ...
}  
  
public void addFirst(T item) {  
    Node newNode = new Node(sentinel,item,sentinel.next)  
    ...
}
```

  比如上例，我在addFirst函数中输入sentinel时，本来应该如下显示：![[Pasted image 20240807131626.png]]
**但是却没有显示，所以我发觉应该是出问题了。果然，我把哨兵结点sentinel的定义放在LinkedListDeque这个函数里了，所以它变成了局部变量，只在LinkedListDeque函数中有生命**

2. 