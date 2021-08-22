# Optional이란?
## NPE
개발을 할 때 가장 많이 발생하는 예외 중 하나가 바로 NPE(NullPointerException)이다. NPE를 피하기 위해서는 null검사하는 로직을 추가해야 하는데, null 검샤를 해야하는 변수가 많은 경우 코드가 복잡해지고 로직이 상당히 번거롭다. 그렇기 때문에 null 대신 초기값을 사용하길 권장하기도 한다.

## Optional
Java8부터는 Optional 클래스를 사용해 NPE를 방지할 수 있도록 도와준다. Optional은 null이 올 수 있는 값을 감싸는 Wrapper 클래스로, 참조하더라도 NPE가 발생하지 않도록 도와준다. Optional 클래스는 value에 값을 저장하기 때문에 null이더라도 바로 NPE가 발생하지 않으며, 클래스이기 때문에 각종 메소드를 제공해준다.

    public final class Optional<T>{
        private final T value;
        
        ....
    }

만약 어떤 데이터가 null이 올 수 있는 경우 해당 값을 Optional로 감싸서 생성할 수 있다. 그리고 orElse 또는 orElseGet메소드를 이용해서 값이 없는 경우라도 안전하게 값을 가져올 수 있다.

    Optional<User> user = Optional.ofNullable(userMapper.getUserByEmail(email));
    String name = user.getName().orElse("annonymous");

