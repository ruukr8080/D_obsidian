
# ENUM 

한글은 어떻게 나올까 
### Employee

```java
package src.subject;  
  
public enum Employee {  
    정규직,  
    비정규직  
}


```

---



# interface
### Salary

```java
package src.subject;  
  
public interface Salary {  
    static final int MinWage = 9860;  
    int bonusPay();  
    int pay(Object o, int time);  
}

```


---

# class
### Member
```java
package src.subject;  
  
public class Member {  
    private long id;  
    private String name;  
    private String department;  
    private String phone;  
  
    public Member() {  
        this.id = id;  
        this.name = name;  
        this.department = department;  
        this.phone = phone;  
    }  
  
    @Override  
    public String toString() {  
        return "Member{" +  
                "id=" + id +  
                ", name='" + name + '\'' +  
                ", department='" + department + '\'' +  
                ", phone='" + phone + '\'' +  
                '}';  
    }  
  
    public long getId() {  
        return id;  
    }  
  
    public void setId(long id) {  
        this.id = id;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public String getDepartment() {  
        return department;  
    }  
  
    public void setDepartment(String department) {  
        this.department = department;  
    }  
  
    public String getPhone() {  
        return phone;  
    }  
  
    public void setPhone(String phone) {  
        this.phone = phone;  
    }  
}
```


### SalaryImp

```java

package src.subject;  
  
import static src.subject.Employee.정규직;  
  
public class SalaryImp implements Salary {  
    int MinWage = 9670;  
    int bonus;  
    int wage;  
  
  
    public int getWage() {  
        return wage;  
    }  
  
    public void setWage(int wage) {  
        this.wage = wage;  
    }  
  
  
  
    public int getMinWage() {  
        return MinWage;  
    }  
  
    public void setMinWage(int minWage) {  
        MinWage = minWage;  
    }  
  
    public int getBonus() {  
        return bonus;  
    }  
  
    public void setBonus(int bonus) {  
        this.bonus = bonus;  
    }  
    @Override  
    public int pay(Object o, int time) {  
        int pay= MinWage * time;;  
        if (o==정규직){  
            pay=MinWage*time;  
        }  
        return pay-(MinWage/10);  
    }  
  
    @Override  
    public int bonusPay() {  
        return +10000;  
    }  
  
  
}
```


# App

### Main


```java
package src.subject;  
  
import static src.subject.Employee.*;  
  
public class Main {  
    public static void main(String[] args) {  
  
        SalaryImp salaryImp = new SalaryImp();  
        Member member = new Member();  
        member.setName("kim");  
        System.out.println(salaryImp.pay(정규직,46));  
        System.out.println(salaryImp.pay(비정규직,46));  
        System.out.println(member.toString()+salaryImp.pay(정규직,46));  
        System.out.println(salaryImp.pay(정규직,46)+ salaryImp.bonusPay()+salaryImp.bonusPay());  
        System.out.println(salaryImp.pay(정규직,46));  
    }  
}


```

---

