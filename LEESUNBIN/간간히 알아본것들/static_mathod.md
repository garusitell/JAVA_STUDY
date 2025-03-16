원시 타입 전달(값에 의한 전달, Pass by Value)은 메소드를 호출할 때, 해당 변수에 저장된 **실제 값**이 복사되어 전달되는 방식입니다. 좀 더 자세히 설명하면:

• **원시 타입이란?**

Java에서 int, double, char, boolean 등의 기본 자료형을 말합니다. 이들은 객체가 아니라 실제 값(데이터)을 직접 저장합니다.

• **값 복사 방식:**

메소드를 호출할 때, 원본 변수에 저장된 값이 복사되어 메소드의 매개변수로 전달됩니다. 이때, 매개변수는 원본 변수와는 별개의 저장 공간을 갖게 됩니다.

예를 들어, 다음과 같이 작성된 메소드에서:

```
static void change(int x){
    x = 1000;
    System.out.println("change() : x = " + x);
}
```

메인 메소드에서 change(d.x)를 호출하면, d.x의 값인 10이 복사되어 x라는 새로운 변수에 저장됩니다. 그래서 메소드 내부에서 x의 값을 1000으로 변경해도, 원래의 d.x 값에는 아무런 영향이 없습니다.



• **메모리 관점에서:**

• d.x라는 원본 변수는 메인 메소드의 스택 영역에 위치합니다.

• change() 메소드에 전달된 x도 별도의 스택 영역에 생성된 변수입니다.

• 이 둘은 서로 다른 저장 공간을 사용하므로, 한쪽에서 값을 바꿔도 다른 쪽에는 영향을 주지 않습니다.

• **실제 예제 코드로 이해하기:**

```
public class PrimitiveParamEx {
    public static void main(String[] args) {
        int num = 10;
        System.out.println("main() : num = " + num);

        change(num); // num의 값 10이 복사되어 전달됨
        System.out.println("After change(num)");
        System.out.println("main() : num = " + num);  // 여전히 10
    }

    static void change(int x) {
        x = 1000;  // x만 1000으로 변경됨
        System.out.println("change() : x = " + x);
    }
}
```

위 코드에서 change() 메소드 내부에서 x 값을 변경해도, main() 메소드의 num 값은 영향을 받지 않고 그대로 10을 유지합니다.



요약하자면, 원시 타입 전달은 **변수에 저장된 실제 값 자체를 복사하여 전달**하는 방식이기 때문에, 메소드 내부에서 복사된 값이 변경되어도 원본 변수에는 아무런 변화가 없게 됩니다.