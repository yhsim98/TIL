# Builder 패턴
class를 설계하다 보면, 필수로 받아야 할 인지들이 있고, 선택적으로 받아야 할 인자들이 있는 경우가 있다. 

필수적으로 받아야 할 인자를 받기 위해 생성자의 매개변수를 넣는다. 또한 선택적 인자를 받기 위해 추가적인 생성자를 만든다. 이것을 이펙티브 자바에서 점층적 생성자 패턴이라 말한다.

점층적 생성자 패턴에는 몇 가지 단점이 있는데  
1. 인자들이 많아질 수록 생성자의 숫자 역시 많아진다
2. 매개변수의 변수명으로 원하는 정보를 설명하지만, 같은 자료형의 경우 변수명을 바꿔 넣는 실수가 생길 수 있다
3. 읽기 어려운 코드가 된다  
이다.

이러한 단점을 보완하기 위해, 자바 빈즈 패턴이 대안점으로 나왔다. setter를 사용하여 변수를 초기화하는 패턴이다. 

가독성은 나아지지만 코드량이 많이 늘어나게 되고 무엇보다 **객체 일관성**이 깨지게 된다. 

객체 일관성이 깨진다는 것은 객체가 한번 생성될 때 그 객체가 변할 여지가 존재한다는 뜻이다. 

멀티스레드 환경의 경우 큰 단점이 될 수 있고 예상치 못한 결과가 생길 수 있다.

그래서 대안으로 나온것이 Builder 패턴이다.

정보들은 자바 빈즈 패턴처럼 받되, 데이터 일관성을 위해 정보들을 다 받은 후 객체생성을 해준다.
``
    public class Example{
        private string s;
        private int i;

        public static class Builder{
            private String s;
            private int i;

            // 반드시 초기화해야 하는 친구
            public Builder(String s){
                this.s = s;
            }

            // 선택적으로 초기화해도 되는 친구
            public Builder setI(int i){
                this.i = i;
                return this;
            }

            public Example build(){
                Example example = new Example();
                example.s = s;
                example.i = i;
                return example
            }
        }
    }


    public method(){
        Example example = new Builder(s)
            .setI(i)
            .builder();
    }
``
setter에 리턴자료형을 Builder 객체를 지정함으로써, 메서드 체이닝 기법을 적용했고, 정보를 다 넣은 경우 build() 메서드로 객체생성을 한다.

이렇게 해주면 데이터 일관성, 객체불변성 등을 만족시켜준다, 또한 코드 가독성 역시 올라가게 된다.

하지만 Builder 객체를 하나 더 만드는 것이므로 때에 따라 성능이 낮아질 수 있다.

Class 설계할 때, 받아야할 인자들이 많거나, 선택적 인자들이 많거나 또한 추가될 인자들이 많은 경우 Builder 패턴을 고려한다면, 좀 더 나은 설계를 할 수 있다.

# lombok @Builder
lombok 라이브러리의 @Builder를 사용하면 자동으로 builder처리를 해준다. 

클래스 위에 붙이면 전체 멤버변수에 대해 builder가 적용되고 선택적 인자들에 적용시키고자 한다면 생성자를 만들어 위에 붙여주면 된다.