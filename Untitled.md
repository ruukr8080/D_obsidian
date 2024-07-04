
# Interface
```java
package src.abstractQuiz;  
  
public interface Go {  
    void go();  
    void stop();  
  
}
```
# Class
```java
package src.abstractQuiz;  
  
import lombok.Data;  
  
import java.util.List;  
  
@Data  
public class Conveyance implements Go {  
    List<String> b = List.of("bicycle", "Bus", "subway", "plain");  
  
    @Override  
    public void go() {  
        System.out.println("gogo");  
    }  
  
    @Override  
    public void stop() {  
        System.out.println("--------------------------------------------------------------------------"+"\n"+"--------------------------------------------------------------------------"+"\n"+"--------------------------------------------------------------------------"+"\n"+"--------------------------------------------------------------------------"+"\n"+"--------------------------------------------------------------------------"+"\n"+"--------------------------------------------------------------------------"+"\n"+"stop");  
    }  
}
```

# Main
```java
package src.abstractQuiz;  
  
import java.util.Scanner;  
  
public class Main {  
    public static void main(String[] args) {  
        Conveyance c = new Conveyance();  
        Scanner sc = new Scanner(System.in);  
        c.go();  
        int i = sc.nextInt();  
        if (i == 1 || i == 2) {  
            System.out.println(i == 1 ? c.b.get(0) : c.b.get(1));  
        } else if (i == 3 || i == 4) {  
            System.out.println(i == 3 ? c.b.get(2) : c.b.get(3));  
        }  
        c.stop();  
    }  
}
```