# JSON(JavaScript Object Notation)
* 경량의 DATA-교환 형식
* Javascript 객체를 만들 때 사용하는 표현식을 의미한다
* JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아서, 최근에는 JSON이 XML을 대체해서 데이터 전송 등에 많이 사용한다
* 특정 언어에 종속되지 않으며, 대부분의 프로그래밍 언어에서 JSON 포맷의 데이터를 핸들링 할 수 있는 라이브러리를 제공하고 있다

# JSON 형식
## name-value 형식의 쌍
여러 가지 언어들에서 object, hashtable, struct로 실현되었다.

    { "string" : "value",
      "string" : "value" }

## 값들의 순서화된 리스트 형식
여러 가지 언어들에서 array, list로 실현되었다.

    [ "value", "value" ]

# JSON 라이브러리 - Jackson
Jackson은 JSON 형태를 Java 객체로, Java 객체를 JSON 형태로 변환해주는 Java용 JSON 라이브러리이다.

가장 많이 사용하는 JSON 라이브러리이다.

xml설정에 mvc:annotation-driven 태그를 추가하면 Spring MVC에 필요한 Bean들을 자동으로 등록해 준다.

DispacherServlet은 url-pattern을 "/" 와 같이 설정하게 되면서 tomcat의 server.xml에 정의되어 있는 url-pattern "/"을 무시하게 된다. 그럼으로 DefaultServlet은 더 이상 동작하지 않게 된다.  
Spring에서는 이를 해결하기 위해 mvc:default-servlet-handler 태그를 추가하면 된다.


# XML(eXtensible Markup Language)
* Data를 저장하고 전달 하기 위한 언어이다
* 인간/기계 모두에게 읽기 편한 언어이다
* XML은 데이터의 구조와 의미를 설명한다


# XML과 HTML의 차이
* XML
    * Data를 전달하는 것에 포커스를 맞춘 언어
    * 사용자가 마음대로 Tag를 정의할 수 있다
* HTML 
    * Data를 표현하는 것에 포커스를 맞춘 언어
    * 미리 정의된 Tag만 사용할 수 있다

# XML Tree 구조
* XML문서는 "root"에서 시작해서 "leaves"로 뻗어가는 트리구조이다
* XML 버전과 문자 인코딩을 정의하는 선언부(prolog)
* XML는 어떠한 데이터를 설명하기 위해 이름을 임의로 작성한 Tag로 데이터를 감싼다

        <customer>
            <name></name>
            <addr></adder>
            <phone></phone>
        </customer>



# Spring MVC기반 RESTful 웹 서비스 구현 절차
1. RESTful 웹 서비스를 처리할 RestfulController 클래스 작성 및 Spring Bean으로 등록

2. 요청을 처리할 메서드에 @RequestMapping, @RequestBody, @ResponseBody 어노테이션을 선언한다

3. REST Client Tool을 사용하여 각각의 메서드를 테스트한다

4. Ajax 통신을 하여 RESTful 웹 서비스를 호출하는 HTML 페이지를 작성한다