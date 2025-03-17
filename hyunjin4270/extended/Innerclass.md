# 내부 클래스 (Inner class)

내부 클래스란, 하나의 클래스 내부에 선언된 또 다른 클래스를 의미한다.
즉, 외부 클래스에 종속된 형태로 존재하며, 특정한 관계를 가지는 클래스들을 논리적으로 그룹화할 때 사용한다.

## 내부클래스의 정의

내부클래스는 하나의 클래스 내부에서 선언된 또 다른 클래스로, 일반적으로 외부 클래스(Outer Class)와 논리적으로 밀접한 괸계를 가진다.

```java
class OuterClass {
    class InnerClass {
        // 내부 클래스의 멤버
    }
}
```

내부부클래스는 외부 클래스의 멤버처럼 동작하며, **외부 클래스의 변수와 메서드에도 접근이 가능**하다.

## 내부클래스를 쓰는 이유

내부클래스를 사용하는 이유는 **외부 클래스와 강한 연관성을 가지는 클래스를 논리적으로 그룹화**하고, 불필요한 클래스 노출을 방지하기 위함이다.
내부클래스를 사용하면 특정한 맥락에서만 의미가 있는 클래스를 외부 클래스 내부에 선언할 수 있어 코드의 구조가 명확해진다.

```java
class ButtonClickListener implements ActionListener {
    public void actionPerformed(ActionEvent e) {
        System.out.println("버튼이 클릭되었습니다!");
    }
}

class MainApp {
    public static void main(String[] args) {
        JButton button = new JButton("클릭");
        button.addActionListener(new ButtonClickListener());
    }
}

```

위 코드는 버튼 클릭 이벤트를 처리하는 클래스다. 만약 이 버튼 클릭 이벤트를 처리하는 클래스가 main외에 다른 곳에서도 쓰일 가능성이 있는 상태다.
말을 줄여, 불필요한 노출을 하고 있는 상태이다. 이 경우와 같은 상태를 현재 쓰이는 MainApp클래스 안에 버튼 클릭 이벤트 클래스를 내부 클래스로 선언하면 된다.

```java
import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

class MainApp {
    public static void main(String[] args) {
        JButton button = new JButton("클릭");

        class ButtonClickListener implements ActionListener {
            public void actionPerformed(ActionEvent e) {
                System.out.println("버튼이 클릭되었습니다!");
            }
        }

        button.addActionListener(new ButtonClickListener());
    }
}
```

MainApp 클래스의 내부클래스로 만들면서 버튼 클릭 클래스가 외부로 보이지 않게끔, 불필요한 노출을 방지를 하였다.
또한 내부클래스와 MainApp 클래스가 어떤 관계인지 직관적으로 만들어 코드의 가독성 또한 높였다.

## 내부클래스를 쓰면 적절한 상황

- 외부 클래스의 특정 기능을 보조하는 경우
    - 외부 클래스의 데이터를 활용하면서, 외부 클래스에서만 필요할 때 사용된다.
- 캡슐화가 필요한 경우
    - 특정 클래스를 외부에서 직접 접근하지 못하도록 하고 싶을 때 사용된다.
- 콜백(Callback) 구현이 필요한 경우
    - 특정 객체에서만 동작해야하는 기능을 정의할 때 유용하다.
- 일회성 객체 생성이 필요한 경우
    - 클래스가 특정 기능을 수행하고 더 이상 사용되지 않을 경우, 내부클래스를 활용하면 코드가 간결해진다.

## 장점

- 캡슐화 강화
    - 외부 클래스에 직접 노출할 필요 없는 클래스를 내부에서 관리할 수 있어 보안성이 높아진다.
- 가독성 및 유지보수성 향상
    - 관련된 코드들을 한 곳에 모아서 유지보수하기 쉬워진다.
- 코드의 논리적 그룹화
    - 특정 클래스가 외부 클래스와 함께 동작할 경우, 두 클래스를 하나로 묶어 직관적인 코드 구조를 만들어 코드리딩이 편해진다.

## 단점

- 코드 복잡성 증가
    - 논리적 그룹화가 되어있지만, 외부클래스 입장에서는 클래스의 개념이 한번 더 들어가 외부 클래스에 대한 가독성이 떨어질 수 있다.
- 메모리 누수 위험성 증가
    - **비정적 내부클래스**는 외부 클래스의 인스턴스를 암묵적으로 참조하고 있으므로 메모리 누수가 발생될 위험이 존재한다.
- 디버깅 및 재사용의 어려움
    - 내부클래스는 외부 클래스와 강한 결합을 가지고 있어 독립적인 테스팅이나 재사용에 어려움이 있다.

