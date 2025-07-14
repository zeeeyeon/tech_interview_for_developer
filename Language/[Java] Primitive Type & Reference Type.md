[Java] 원시 타입 & 참조 타입

## 원시 타입 (Primitive Type)
<hr>

- 메모리에 값 자체(value)를 저장하는 타입
- Stack 영역에 직접 저장됨 (가벼움, 빠름)
- 값은 변경될 수 있음 int x = 3; x = 5;
- 래퍼 클래스 통해 객체화 가능 (박싱 가능)
- null 할당 불가 (유효한 값 - 초기화 값을 가져야 함)

```
int a = 5;
int b = 5;
System.out.println(a == b); // true
```

## 참조 타입 (Reference Type)
<hr>

- 객체의 메모리 주소(참조값)를 저장하는 타입
- 참조는 Stack, 객체는 Heap 저장
- 참조값은 변경 가능, 내부 필드도 변경 가능
- null 할당 가능 (객체가 없음을 나타냄)

```
String s1 = "hello";
String s2 = new String("hello");
System.out.println(s1 == s2);         // false (주소 비교)
System.out.println(s1.equals(s2));    // true (값 비교)
```

### 원시 타입과 참조 타입에 대해 설명해보세요.
<hr>

> - 자바의 데이터 타입은 크게 원시 타입(Primitive Type)과 참조 타입(Reference Type)으로 나뉩니다.
> - 원시 타입은 int, double, boolean 같은 기본 타입으로, 값 자체를 Stack 메모리에 저장합니다.
> - 반면, 참조 타입은 배열, String, 클래스 같은 객체들이며, Heap에 생성된 객체의 주소(참조값)를 Stack에 저장합니다.
> - 가장 큰 차이점은 복사 시의 동작입니다. 원시 타입은 값을 복사하므로 서로 영향을 주지 않지만, 참조 타입은 주소를 복사하기 때문에 같은 객체를 공유하게 됩니다.
> - 예를 들어 List<String> a = new ArrayList<>(); List<String> b = a;처럼 참조 타입을 복사하면, a와 b는 같은 리스트 객체를 가리키고, 하나를 수정하면 다른 쪽도 함께 바뀝니다.
> - 실무에서는 이 차이 때문에 깊은 복사(Deep Copy)나 불변 객체(Immutable Object)를 적극적으로 활용하게 됩니다.
> - 그리고 ==과 .equals() 비교 방식에서도 원시 타입은 값 비교, 참조 타입은 주소 비교가 기본이기 때문에 자주 주의합니다.

