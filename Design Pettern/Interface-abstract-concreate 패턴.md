# interface-abstract-concreate 패턴
이 패턴은 interface의 구현체에 중복된 코드가 생길 경우 사용할 수 있는 패턴입니다.

중복되는 코드가 있는 경우 인터페이스를 상속받는 추상 클레스를 만들어 추상 클래스에 공통되는 메소드를 작성해 줍니다. 

인터페이스의 메소드는 abstract public이기 때문에 추상 클래스에서 구현할 메소드만 구현하고 나머지는 딱히 뭔가를 해 줄 필요없이 추상 클래스를 상속받는 클래스로 넘어가게 됩니다. 

    interface A{

    }

    abstract public class B implements A{
    }

    class C extends B{

    }


이 경우 A interface가 B의 조상이 되기 때문에 
    A a = new B();
도 문제 없이 작동하게 됩니다.

