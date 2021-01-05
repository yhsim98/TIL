# DI(의존성 주입)

## 의존성이란?
함수 또는 클래스가 작동하기 위해서 다른 객체를 필요로 할 때 의존성을 가진다고 한다.

결합도가 높은 상태로써 코드의 재활용성이 떨어지고, 유닛 테스트 작성이 어렵고, 한 객체를 수정했을 때 다른 객체도 수정해줘야하는 문제가 발생할 수 있다

## DI를 했을 때 얻는 이점
1. 코드의 재사용 가능성을 높인다
2. 유닛 테스트 작성이 용이해진다
3. 객체 간의 의존성을 줄여준다
4. 객체 간의 결합도를 낮춰 유연한 코드를 작성할 수 있다 

## 의존성 주입 방법 
 필요한 클래스를 직접 생성하는 것이 아닌 외부에서 생성하여 주입받아 객체 간 관계를 포함에서 사용관계로 바꾸어 주면 된다
    
    //직접 생성하는 의존성이 높은 상태
    class Dependency{
        private Class c;
        public Dependency(){
            c = new Class();        
        }
    }

    //외부에서 받는 의존성 주입
    class Dependency{
        private Class c;

        public Dependency(Class c){
            this.c = c;
        }

        public setC(Class c){
            this.c = c;
        }
    }