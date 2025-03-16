내부 클래스와 중첩 클래스: 개념과 차이점**



자바에서는 클래스 내부에 또 다른 클래스를 정의할 수 있으며, 이를 **중첩 클래스**(nested class)라고 합니다 . 중첩 클래스 중 static 키워드가 붙지 않은 것은 **내부 클래스**(inner class)라고 하고, static 키워드로 선언된 것은 **정적 중첩 클래스**(static nested class)라고 부릅니다 .



내부 클래스는 외부 클래스의 인스턴스에 종속적으로 존재합니다. 즉 **내부 클래스의 인스턴스는 반드시 특정 외부 클래스 인스턴스에 속하여 생성**되며 , 그 때문에 **외부 클래스의 모든 멤버(필드와 메서드)에 직접 접근 가능**합니다 (private 멤버도 접근할 수 있습니다) . 또한 내부 클래스는 인스턴스에 속한 클래스이므로 **자체적으로 static 멤버(정적 필드나 메소드)를 가질 수 없습니다** .



반면 **정적 중첩 클래스**는 외부 클래스의 이름 공간에 묶여 있을 뿐 **외부 클래스 인스턴스와 독립적으로 존재**합니다. 정적 클래스이므로 외부 클래스의 인스턴스에 대한 **암묵적 참조가 없으며**, **외부 인스턴스 멤버에는 직접 접근할 수 없습니다** (필요하다면 외부 클래스의 객체를 매개변수로 받아 접근해야 합니다). 정적 중첩 클래스는 일반적인 톱레벨(top-level) 클래스처럼 동작하지만, 외부 클래스와 논리적으로 관계된 헬퍼(helper) 클래스를 그 안에 묶어둘 수 있다는 장점이 있습니다 .



내부 클래스와 정적 중첩 클래스의 주요 차이점을 정리하면 다음과 같습니다:

• **내부 클래스 (non-static inner class)**:

• 외부 클래스의 한 **인스턴스에 속하여** 생성됩니다. 외부 클래스 없이 단독으로 생성될 수 없습니다.

• 외부 클래스의 모든 멤버에 **직접 접근**할 수 있습니다 (private 필드도 접근 가능) .

• 컴파일러가 내부 클래스당 하나의 **숨겨진 참조(OuterClass.this)**를 추가하여, 내부적으로 외부 인스턴스를 가리킵니다.

• 인스턴스에 종속적이므로 **정적 필드나 정적 메서드를 가질 수 없습니다** .

• **정적 중첩 클래스 (static nested class)**:

• 외부 클래스의 **정적 맥락**에 속하며, 외부 클래스 인스턴스와 독립적으로 존재합니다.

• 외부 클래스의 인스턴스 없이도 생성할 수 있습니다 (예: OuterClass.StaticNested nested = new OuterClass.StaticNested();).

• 외부 클래스의 인스턴스 멤버에는 **직접 접근할 수 없으며** 필요시 명시적으로 **외부 인스턴스의 참조**를 받아 사용해야 합니다 .

• 사실상 일반 클래스와 유사하게 동작하지만, 외부 클래스와 관련된 기능을 논리적으로 한 곳에 묶어둘 수 있습니다. 또한 **외부 인스턴스에 대한 참조를 갖지 않으므로 메모리 사용이 효율적**입니다 .



참고로, 내부 클래스는 선언 위치와 형태에 따라 **멤버 내부 클래스**, **지역 내부 클래스**, **익명 내부 클래스** 등으로도 구분됩니다 . 이들은 모두 비정적 내부 클래스의 일종이며, 필요에 따라 외부의 변수나 상태에 접근하는 용도로 활용됩니다 (예를 들어, 익명 클래스는 이벤트 처리기 구현 등에 자주 사용되었습니다).



아래 코드는 **내부 클래스와 정적 중첩 클래스의 차이**를 보여주는 예제입니다. 내부 클래스 Inner는 외부 클래스의 인스턴스 필드에 직접 접근할 수 있지만, 정적 중첩 클래스 StaticNested는 외부 인스턴스 참조를 통해서만 접근이 가능함을 볼 수 있습니다:

```
public class Outer {
    private String outerField = "Outer field";
    private static String staticOuterField = "Static outer field";

    class Inner {
        void accessMembers() {
            // 내부 클래스는 외부 인스턴스의 멤버에 직접 접근 가능
            System.out.println("Inner: " + outerField);
            System.out.println("Inner: " + staticOuterField);
        }
    }

    static class StaticNested {
        void accessMembers(Outer outer) {
            // 정적 중첩 클래스에서 외부 인스턴스의 멤버에 직접 접근 -> 컴파일 오류
            // System.out.println("StaticNested: " + outerField);  // Compile error [oai_citation_attribution:12‡docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html#:~:text=%2F%2F%20Compiler%20error%3A%20Cannot%20make,)

            // 외부 객체를 전달받으면 접근 가능
            System.out.println("StaticNested: " + outer.outerField);
            System.out.println("StaticNested: " + staticOuterField);
        }
    }

    public static void main(String[] args) {
        Outer outerObj = new Outer();
        Inner innerObj = outerObj.new Inner();            // 내부 클래스 객체 생성
        innerObj.accessMembers();                        // "Outer field", "Static outer field" 출력

        StaticNested staticObj = new StaticNested();     // 정적 중첩 클래스 객체 생성 (외부 인스턴스 불필요)
        staticObj.accessMembers(outerObj);               // "Outer field", "Static outer field" 출력
    }
}
```

위 코드에서 Inner 클래스의 accessMembers() 메소드는 outerField와 staticOuterField를 직접 참조합니다. 반면 StaticNested 클래스의 accessMembers(Outer outer) 메소드는 외부 클래스의 인스턴스를 매개변수로 받아와 outerField를 출력하고 있습니다. StaticNested 내부에서 outerField를 직접 사용하려고 하면 **컴파일 에러**가 발생하며 , 이는 정적 중첩 클래스가 외부의 인스턴스에 직접 접근할 수 없음을 나타냅니다. 정적 중첩 클래스는 대신 전달된 outer 객체나 정적 필드를 통해서만 외부 클래스에 접근할 수 있습니다.



**디자인 패턴에서의 내부 클래스 활용**



내부 클래스 (특히 정적 중첩 클래스)는 일부 디자인 패턴 구현에 활용되어 코드 구조를 개선하고 캡슐화를 강화합니다. 그 중 두 가지 대표적인 사례가 **빌더 패턴**과 **싱글턴 패턴**입니다. 아래에서는 이 패턴들에서 내부 클래스가 어떻게 쓰이는지 설명하고 예제를 보여드립니다.



**빌더 패턴 (Builder Pattern)과 내부 클래스**



**빌더 패턴**은 복잡한 객체를 생성하는 과정을 단계별로 구성하여 가독성을 높이고, 생성 과정에서의 일관성(consistency)을 보장하는 **생성(creational) 패턴**입니다. 생성자에 많은 매개변수를 전달하는 방식 (텔레스코핑 생성자 문제)을 해결하기 위해 고안되었으며, Joshua Bloch의 _Effective Java_에서도 권장되고 있습니다. 자바에서 빌더 패턴을 구현할 때는 보통 **외부 클래스 안에 정적 중첩 클래스로 빌더를 정의**하는 방법이 많이 활용됩니다 . 이렇게 하면 빌더가 외부 클래스와 논리적으로 묶여 있음을 나타내며, 빌더 클래스 코드가 외부 클래스 내부에 있기 때문에 외부 클래스의 private 생성자 등에 접근할 수 있는 이점이 있습니다 .



빌더 패턴 구현의 기본 구조는 다음과 같습니다:

1. **정적 내부 클래스**로 빌더를 만든다. 외부 클래스 이름이 Product라면 빌더 클래스는 ProductBuilder처럼 명명합니다 .

2. 빌더 클래스는 **외부 클래스와 동일한 필드들**을 가집니다. 이 중 필수 값들은 빌더의 생성자에서 받고, 선택적인 값들은 기본값으로 초기화해 둘 수 있습니다.

3. 빌더 클래스에 선택적 필드에 대한 설정 메서드를 정의합니다 (예: setAge(int age) 혹은 더 흔하게 메서드 체이닝을 위한 age(int age) 형태). 이 메서드들은 값을 설정한 뒤 **자신(Builder 객체)**을 반환하여 연속 호출이 가능하게 합니다 .

4. 외부 클래스에는 빌더를 매개변수로 받는 **private 생성자**를 만들어, 빌더가 가진 값으로 외부 클래스의 필드들을 초기화합니다. (외부 클래스의 public 생성자는 막아 두고, 오로지 빌더를 통해서만 객체를 생성하도록 유도합니다.)

5. 빌더 클래스에 build() 메서드를 정의하여, 앞의 private 생성자를 호출해 완성된 **외부 클래스 객체를 생성 및 반환**합니다 .

6. 클라이언트는 빌더를 사용하여 다음과 같이 객체를 생성합니다: new Product.ProductBuilder(필수값들).옵션설정(...).build(). 이렇게 하면 객체 생성 과정이 읽기 쉽고, 필요한 값만 설정하여 **일관된 상태의 객체**를 얻을 수 있습니다.



아래는 Person 클래스를 빌더 패턴으로 구현한 예제입니다. Person은 이름(name)은 필수, 나이(age)와 주소(address)는 선택값이라고 가정합니다. 내부에 정적 클래스 PersonBuilder를 두어 Person 객체 생성을 도와줍니다:

```
public class Person {
    // Person의 필드들 (모두 private)
    private final String name;
    private final int age;
    private final String address;

    // private 생성자: Builder를 통해서만 호출됨
    private Person(PersonBuilder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.address = builder.address;
    }

    // 정적 빌더 클래스
    public static class PersonBuilder {
        // 필요한 필드들 (외부 클래스와 동일한 이름 사용)
        private final String name;   // 필수 항목
        private int age = 0;         // 선택 항목 (기본값 0)
        private String address = ""; // 선택 항목 (기본값 빈 문자열)

        public PersonBuilder(String name) {
            this.name = name;        // 필수 값 설정 (이 값 없이는 Person 생성 불가)
        }

        public PersonBuilder age(int age) {
            this.age = age;          // 선택적 값 설정
            return this;             // 자기 자신을 반환하여 체이닝 지원
        }

        public PersonBuilder address(String address) {
            this.address = address;
            return this;
        }

        public Person build() {
            // 바깥의 Person 객체를 생성하여 반환
            return new Person(this);
        }
    }

    // 객체 내용 확인용 (toString 오버라이드)
    @Override
    public String toString() {
        return "Person{name=" + name + 
               ", age=" + age + 
               ", address=" + address + "}";
    }
}
```

위 Person 클래스에서는 PersonBuilder를 통해 Person의 인스턴스를 만들 수 있습니다. Person 생성자의 접근 제어자가 private이므로 직접 new Person(...) 하는 것은 불가능하고, PersonBuilder를 통해서만 객체를 생성하게 합니다. 사용 예시는 다음과 같습니다:

```
// 빌더를 이용한 Person 객체 생성 예
Person person = new Person.PersonBuilder("Alice")
                    .age(30)
                    .address("Seoul")
                    .build();
System.out.println(person);
// 출력: Person{name=Alice, age=30, address=Seoul}
```

위와 같이 빌더를 사용하면, 필요한 필드만 설정하면서 객체를 읽기 쉽게 생성할 수 있습니다. 필수 값인 name을 PersonBuilder 생성자에서 받아 설정하고, 선택 값들은 원하는 대로 age(...), address(...) 메서드로 지정한 뒤 build()를 호출하면 최종 Person 객체를 얻습니다. 이 패턴의 장점은 다음과 같습니다:

• **가독성 향상**: 매개변수가 많은 생성자를 사용하는 대신, 어떤 값이 어떤 필드를 채우는지 명시적으로 보여주므로 코드를 읽기 쉽습니다.

• **객체의 일관성**: 객체를 완전히 생성하기 전까지는 Person 인스턴스를 만들지 않으므로, 불완전한 상태의 객체가 생기는 것을 방지합니다. (예를 들어 age나 address를 설정하지 않으면 기본값이 적용된 상태로 생성됨)

• **캡슐화**: 빌더를 외부 클래스 안에 두면 외부 클래스의 구현 세부사항(예: private 생성자나 필드)을 빌더가 사용할 수 있어 객체 생성 로직을 외부로 노출하지 않으면서도 유연하게 객체를 구성할 수 있습니다 .



**싱글턴 패턴 (Singleton Pattern)과 내부 클래스**



**싱글턴 패턴**은 애플리케이션 전역에서 **오직 하나의 인스턴스만 존재해야 하는 클래스**를 구현하는 패턴입니다. Java에서 싱글턴을 구현하는 여러 방법이 있는데, 그 중 **정적 내부 클래스를 활용한 방식**은 구현이 간결하면서도 지연 초기화(lazy initialization)와 스레드 안전성을 모두 만족하기 때문에 자주 언급됩니다 . 이 기법은 _Bill Pugh Singleton_ 구현으로도 알려져 있습니다.



구현 원리는 다음과 같습니다: 외부 클래스(Singleton) 안에 private static으로 선언된 내부 클래스(SingletonHolder 등)를 만들고, 그 클래스 안에 외부 클래스의 유일한 인스턴스를 static 필드로 선언합니다. 외부 클래스의 getInstance() 메서드에서는 이 내부 클래스의 static 필드를 반환하도록 합니다. 이렇게 하면 **외부 클래스 로딩 시에는 내부 클래스가 로드되지 않고**, 최초로 getInstance()를 호출할 때 비로소 내부 클래스가 로드되면서 해당 인스턴스를 생성합니다 . JVM의 클래스 로딩 특성을 이용하여 **스레드 안전한 지연 초기화**를 구현하는 것입니다. 아래 코드로 예를 보겠습니다:

```
public class Singleton {
    private Singleton() {
        // private 생성자: 외부에서 인스턴스 생성 불가
    }

    // 정적 내부 클래스가 유일한 Singleton 인스턴스를 보관
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        // 내부 클래스가 로드되면서 INSTANCE가 초기화됨
        return SingletonHolder.INSTANCE;
    }
}
```

위 Singleton 클래스에서는 SingletonHolder라는 정적 내부 클래스가 INSTANCE라는 Singleton 타입 정적 변수를 가지고 있습니다. 이 변수는 SingletonHolder 클래스 로딩 시 한 번 초기화되며, Singleton.getInstance() 호출시 SingletonHolder.INSTANCE를 반환합니다. 이 구현의 핵심은 **JVM이 클래스 로딩을 지연시킨다는 점**입니다 . SingletonHolder는 getInstance()가 호출되기 전까지는 로딩되지 않으므로 INSTANCE 초기화도 이루어지지 않습니다. 최초 호출 시 클래스 로딩과 초기화가 한꺼번에 일어나 단일 인스턴스가 만들어지며, JVM의 클래스 로더는 한 번에 하나의 클래스를 초기화하므로 별도의 동기화 없이도 **Thread-Safe**하게 싱글턴이 생성됩니다. 이후에는 동일한 인스턴스를 계속 반환하므로 전역에서 하나의 객체가 유지됩니다.



이러한 정적 내부 클래스 방식 이외에도 싱글턴 구현은 enum을 이용하는 방법, 이중 체크 락(Double-checked locking)을 사용하는 방법 등이 있지만, 내부 클래스를 사용하는 방식은 구현이 단순하고 효과적이라는 장점이 있습니다.



**내부 클래스와 메모리 누수 (Memory Leak)**



내부 클래스를 사용할 때는 **메모리 누수**(memory leak)에 주의해야 합니다. 앞서 설명했듯이 **비정적 내부 클래스는 자신을 생성한 외부 클래스 인스턴스에 대한 숨은 참조를 갖고 있습니다** . 따라서 내부 클래스 객체가 살아있는 한, 그 내부 클래스가 가리키는 외부 객체도 가비지 컬렉터(GC)의 수거 대상이 되지 않아 메모리에 계속 남아 있게 됩니다 . 이는 의도하지 않은 메모리 누수를 야기할 수 있습니다.



일반적으로 **내부 클래스 인스턴스의 생명주기가 외부 클래스 인스턴스보다 길어질 때** 메모리 누수가 문제가 됩니다. 예를 들어, 어떤 객체의 내부 클래스를 생성하여 **전역적인 장소(예: static 컬렉션이나 싱글턴 리스트 등)에 보관**해 두면, 비록 외부 객체는 더 이상 사용되지 않더라도 내부 클래스가 외부를 참조하고 있기 때문에 외부 객체가 해제되지 않습니다. 아래 코드는 이러한 상황을 보여줍니다:

```
import java.util.List;
import java.util.ArrayList;

class OuterLeakDemo {
    // 큰 데이터를 갖는 외부 클래스 (가비지 컬렉션 대상이 되길 기대)
    private int[] largeData = new int[10000]; 

    class Inner {
        void doSomething() {
            System.out.println("Inner is doing something.");
        }
    }
}

class MemoryLeakTest {
    static List<Object> store = new ArrayList<>();

    public static void main(String[] args) {
        OuterLeakDemo outer = new OuterLeakDemo();
        OuterLeakDemo.Inner innerObj = outer.new Inner();
        store.add(innerObj);   // 내부 클래스 객체를 static 리스트에 저장
        outer = null;          // 외부 객체에 대한 직접 참조 해제

        System.out.println("End of program");
    }
}
```

위 코드에서 OuterLeakDemo의 인스턴스 outer는 main 메소드에서 지역 변수로 생성되었습니다. 그 내부 클래스 Inner의 인스턴스 innerObj를 static 리스트인 store에 추가한 후 outer 변수를 null로 설정하면, 이제 OuterLeakDemo 객체는 직접 참조하는 곳이 없어집니다. 겉보기에는 outer가 가비지 컬렉션 될 수 있어야 하지만, innerObj가 여전히 살아 있고 store 리스트에 의해 프로그램 종료까지 참조됩니다. 그리고 innerObj는 비정적 내부 클래스이므로 자신을 생성한 OuterLeakDemo 인스턴스를 숨겨진 참조로 붙잡고 있습니다 . 그 결과 OuterLeakDemo 인스턴스는 main이 끝난 뒤에도 메모리에 남아서 제거되지 않는 **메모리 누수**가 발생합니다.



이러한 문제를 **방지하는 가장 쉬운 방법은 내부 클래스를 가능하면 정적(static) 클래스로 선언**하는 것입니다. 내부 클래스가 정적이 되면 더 이상 외부 인스턴스에 대한 자동 참조를 가지지 않으므로, 내부 클래스 객체가 오래 유지되더라도 외부 객체를 붙잡지 않게 됩니다 . 위 코드를 수정해서 Inner 클래스를 정적 중첩 클래스로 바꾸면 문제를 해결할 수 있습니다:

```
import java.util.List;
import java.util.ArrayList;

class OuterNoLeakDemo {
    private int[] largeData = new int[10000];

    static class InnerStatic {
        void doSomething() {
            System.out.println("InnerStatic is doing something.");
        }
    }
}

class MemoryLeakTestFixed {
    static List<Object> store = new ArrayList<>();

    public static void main(String[] args) {
        OuterNoLeakDemo outer2 = new OuterNoLeakDemo();
        OuterNoLeakDemo.InnerStatic innerObj2 = new OuterNoLeakDemo.InnerStatic();
        store.add(innerObj2);   // 정적 내부 클래스 객체를 저장
        outer2 = null;          // OuterNoLeakDemo 객체에 대한 참조 해제

        System.out.println("End of program (fixed)");
    }
}
```

수정된 코드에서는 InnerStatic 클래스가 static으로 선언되었기 때문에 innerObj2는 더 이상 outer2에 대한 숨은 참조를 갖고 있지 않습니다. 따라서 outer2 변수를 null로 처리한 시점 이후에는 OuterNoLeakDemo 인스턴스가 참조되지 않으므로 가비지 컬렉터가 이를 제대로 수거할 수 있습니다. innerObj2는 여전히 store에 남아 프로그램 종료까지 살아있지만, OuterNoLeakDemo 객체와는 연관이 없으므로 메모리 누수가 발생하지 않습니다. 이처럼 **정적 중첩 클래스를 사용하면 내부 클래스가 외부 객체의 메모리 해제를 방해하지 않게 되어 메모리 관리가 쉬워집니다** .



만약 내부 클래스가 반드시 외부 클래스의 인스턴스 필드나 메서드에 접근해야 해서 **정적으로 만들 수 없는 경우**, 메모리 누수를 방지하기 위한 몇 가지 전략이 있습니다:

• 내부 클래스 객체를 사용한 후에는 **외부 객체에 대한 참조를 명시적으로 해제**합니다. 예를 들어 내부 클래스 객체를 다른 객체의 필드나 컬렉션에 담았다면, 더 이상 필요 없을 때 그 객체를 컬렉션에서 제거하거나 참조를 null로 만드는 방식으로 연결을 끊습니다. 이벤트 리스너의 콜백으로 내부 클래스를 사용한 경우, 이벤트 소스에서 해당 리스너를 해제(remove)하여 참조를 제거해야 합니다.

• 설계상 내부 클래스의 수명이 외부 클래스보다 길어질 수밖에 없다면, **약한 참조(WeakReference)**를 활용하는 것을 고려합니다. WeakReference로 외부 객체를 참조하면, 내부 클래스가 외부를 참조하더라도 GC는 외부 객체를 수거할 수 있습니다 (약한 참조만 남은 객체는 가비지 컬렉션 대상이 됩니다). 이 기법은 특히 GUI 애플리케이션이나 Android 개발에서 자주 활용됩니다. 예를 들어 Android의 Handler나 Runnable을 Activity의 내부 클래스로 작성하면 Activity를 오래 붙잡아 둘 위험이 있는데, 이를 해결하기 위해 해당 클래스들을 **static으로 선언하고**, 내부에서 Activity를 참조해야 하면 WeakReference<Activity>를 사용하는 패턴이 권장됩니다 .

• 가능하다면 **내부 클래스의 설계 자체를 변경**하여, 외부 인스턴스에 대한 강한 참조를 유지하지 않도록 합니다. 예를 들어 내부 클래스가 외부의 일부 데이터만 필요로 한다면, 필요한 데이터만 생성자나 메서드 인자로 전달받고 외부 전체를 참조하지 않도록 구성할 수 있습니다. 또는 내부 클래스를 아예 외부 클래스의 정적 메서드에서 만들어지는 별도의 객체로 정의하여 외부와의 생명주기 결합을 끊는 방법도 고려할 수 있습니다.



요약하면, **내부 클래스 사용 시에는 그 객체의 생명주기가 외부 객체의 생명주기와 적절히 관리되도록 신경써야 합니다**. 특별히 비정적 내부 클래스는 항상 외부를 참조한다는 점을 기억하고, 불필요하게 오래 살아남는 내부 객체가 없다면 일반적으로 문제는 없지만, 만약 내부 객체가 외부보다 오래 지속될 가능성이 있다면 설계를 개선하거나 정적 클래스로 변경하는 것이 좋습니다. 이런 점을 유의하면 내부 클래스는 매우 유용하게 사용할 수 있는 도구이며, 논리적인 클래스 구조를 만들고 캡슐화를 강화하는데 큰 도움이 됩니다.