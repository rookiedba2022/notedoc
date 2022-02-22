## 三、类加载器

1. 虚拟机自带的加载器
2. 启动类（根）加载器
3. 扩展类加载器
4. 应用程序加载器

```java
public class Car {
    public int age;
    
    public static void main(String[] args) {
        Car car1 = new Car();
        Car car2 = new Car();
        
    }
}
```

