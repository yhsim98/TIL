# MVC 패턴의 개념
모델 뷰 컨트롤러(Model View Controller)는 소프트웨어 공학에서 사용되는 아키텍쳐 패턴으로 MVC패턴의 주 목적은 Business logic과 Presentation logic을 분리하기 위함이다.

* MVC 패턴을 사용하면, 사용자 인터페이스로부터 비즈니스 로직을 분리하여 애플리케이션의 시각적 요소나 그 이면에서 실행되는 비즈니스 로직을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있다

# MVC 패턴의 개념
![](https://t1.daumcdn.net/cfile/tistory/220988495395039D11)

클라이언트가 컨트롤러에 요청을 하면 컨트롤러는 모델을 호출하고, 모델이 호출을 받아서 처리하고 결과를 컨트롤러에 넘겨주면 컨트롤러는 뷰에 화면 생성요청하고 뷰가 결과화면을 컨트롤러에 돌려주면 컨트롤러가 브라우져에 결과화면을 전달한다

# 각각의 MVC 컴포넌트의 역할
* 모델(Model) 컴포넌트
    * 데이터 저장소와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다루는 일을 한다
    * 여러 개의 데이터 변경 작업을 하나의 작업으로 묶는 트랜잭션을 다루는 일도 한다
    * DAO 클래스, Service 클래스에 해당한다
* 뷰 컴포넌트
    * 모델이 처리한 데이터나 그 작업 결과를 가지고 사용자에게 출력할 화면을 만드는 일을 한다
    * 생성한 화면은 웹 브라우져가 출력하고, 뷰 컴포넌트는 HTML과 CSS, Java Script를 사용하여 웹 브라우저가 출력할 UI를 만든다
    * Html과 JSP를 사용하여 작성할 수 있다
* 컨트롤러 컴포넌트
    * 클라이언트의 요청을 받았을 때 그 요청에 대해 실제 업무를 수행하는 모델 컴포넌트를 호출하는 일을 한다
    * 클라이언트가 보낸 데이터가 있다면, 모델을 호출할 때 전달하기 쉽게 데이터를 적절히 가공하는 일을 한다
    * 모델이 업무 수행을 완료하면, 그 결과를 가지고 화면을 생성하도록 뷰에게 전달(클라이언트 요청에 대해 모델과 뷰를 결정하여 전달)
    * Servlet과 JSP를 사용하여 작성할 수 있다

# 모델2 아키텍쳐 개념
* 모델1 아키텍쳐 : Controller 역할을 JSP가 담당함
* 모델2 아키텍쳐 : Controller 역할을 Servlet이 담당함

![](https://t1.daumcdn.net/cfile/tistory/193EEF2E4CD60D2B29)

1. 웹 브라우저가 웹 애플리케이션 실행을 요청하면, 웹 서버가 그 요청을 받아서 서블릿 컨테이너(톰캣서버)에 넘겨준다

서블릿 컨테이너는 URL을 확인하여 그 요청을 처리할 서블릿을 찾아서 실행한다.

2. 서블릿은 실제 업무를 처리하는 모델 자바 객체의 메서드를 호출한다.  
만약 웹 브라우저가 보낸 데이터를 저장하거나 변경해야 한다면 그 데이터를 가공하여 VO 객체를 생성하고, 모델 객체의 메서드를 호출할 때 인자값으로 넘긴다

모델 객체는 엔터프라이즈 자바빈일 수도 있고, 일반 자바 객체일 수도 있다.

3. 모델 객체는 JDBC를 사용하여 매개변수로 넘어온 값 객체를 데이터베이스에 저장하거나, 데이터베이스로부터 질의 결과를 가져와서 VO 객체로 만들어 반환한다.

4. 서블릿은 모델 객체로부터 반환받은 값을 JSP에 전달한다

5. JSP는 서블릿으로 부터 전달받은 값 객체를 참조하여 웹 브라우저가 출력할 결과 화면을 만들고, 웹 브라우저에 출력함으로써 요청 처리를 완료한다.

6. 웹 브라우저는 서버로부터 받은 응답 내용을 화면에 출력한다.

# Front Controller 패턴 아키텍쳐
![](https://www.javatpoint.com/images/designpattern/front-controller-pattern.png)
* Front Controller는 클라이언트가 보낸 요청을 받아서 공통적인 작업을 먼저 수행한다
* Front Controller는 적절한 세부 Controller에게 작업을 위임한다
* 각각의 애플리케이션 Controller는 클라이언트에게 보낼 뷰를 선택해서 최종 결과를 생성하는 작업을 한다
* Front Controller패턴은 인증이나 권한 체크처럼 모든 요청에 대하여 공통적으로 처리해야 하는 로직이 있을 경우 전체적으로 클라이언트의 요청을 중앙 집중저긍로 처리하고자 할 경우에 사용한다

# Spring MVC의 특징
* Spring은 DI나 AOP같은 기능뿐만 아니라, 서블릿 기반의 웹 개발을 위한 MVC 프레임워크를 제공한다
* Spring MVC는 모델2 아키텍쳐와 Front Controller 패턴을 프레임워크 차원에서 제공한다
* Spring MVC 프레임워크는 Spring을 기반으로 하고 있기 때문에 Spring이 제공하는 트랜잭션 처리나 DI 및 AOP등을 쉽게 사용할 수 있다


# Spring MVC와 Front Controller 패턴
* 대부분의 MVC 프레임워크들은 Front Controller 패턴을 적용해서 구현한다
* Spring MVC도 Front Controller 역할을 하는 DispatcherServlet이라는 클래스를 계층의 맨 앞단에 놓고, 서버로 들어오는 모든 요청을 받아서 처리하도록 구성한다
* 예외가 발생했을 때 일관된 방식으로 처리하는 것도 Front Controller의 역할이다


# DispatherServlet 클래스
* Front Controller 패턴 적용해서 만들어진 클래스
* 개발자가 사용하기 위해서는 web.xml에 설정을 해야 한다
* 클라이언트로부터의 모든 요청을 전달 받음
* Controller나 View와 같은 Spring MVC의 구성요소를 이용하여 클라이언트에게 서비스를 제공

# Spring MVC의 주요 구성 요소

## DispacherServlet 
클라이언트의 요청을 받아서 Controller에게 클라이언트의 요청을 전달하고, 리턴한 결과값을 View에게 전달하여 알밪은 응답을 생성한다

## HandlerMapping
URL과 요청 정보를 기준으로 어떤 핸들러 객체를 사용할지 결정하는 객체이며, DispacherServlet은 하나 이상의 핸들러 매핑을 가질 수 있따

## Controller 
클라이언트의 요청을 처리한 뒤, Model를 호출하고 그 결과를 DispacherServlet에게 알려 준다

## ModelAndView 
Controller가 처리한 데이터 및 화면에 대한 정보를 보유한 객체

## View
Controller의 처리 결과 화면에 대한 정보를 보유한 객체

## ViewResolver 
Controller가 리턴한 뷰 이름을 기반으로 Controller 처리 결과를 생성할 뷰를 결정

# ViewResolver
Controller가 리턴한 뷰 이름을 기반으로 Controller의 실행결과를 어떤 view에서 보여줄 것인지 결정하는 기능을 제공한다.  

InternalResourceViewResolver는 JSP를 사용하여 view를 생성한다.

    <bean id="viewResolver"
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/" />
		<property name="suffix" value=".jsp" />
	</bean>

prefix : Controller가 리턴한 view 이름 앞에 붙을 접두어
suffix : Controller가 리턴한 view 이름 뒤에 붙은 확장자

Controller가 "hello"를 리턴했다면 위 예시의 경우 "/hello.jsp"가 된다.


# Model 클래스
View에 데이터를 전달한다
* Controller에서 Service를 호출한 결과를 받아서 view에게 전달하기 위해, 전달받은 결과를 Model 객체에 저장한다
* Model addAttribute(string name, object value) 
    * value객체를 name이름으로 저장하고, view코드에서는 name으로 지정한 이름을 통해서 value를 사용한다

# Spring MVC의 주요 구성 요소의 요청 처리 과정(2)
1. 클라이언트의 요청이 DispacherServlet에게 전달된다
2. DispacherServlet은 HandlerMapping을 사용하여 클라이언트의 요청을 처리한 Controller를 획득한다.
3. DispacherServlet은 Controller 객체를 이용하여 클라이언트의 요청을 처리한다.
4. Controller는 클라이언트 요청 처리 결과와 View 페이지 정보를 담은 ModelAndView 객체를 반환한다.
5. DispacherServlet은 ViewResolver로부터 응답 결과를 생성할 View 객체를 구한다.
6. View는 클라이언트에게 전달할 응답을 생성한다.

# Spring MVC 기반 웹 어플리케이션 작성 절차
1. 클라이언트의 요청을 받는 DispathcerServlet를 web.xml에 설정한다
2. 클라이언트의 요청을 처리한 Contriller를 작성한다
3. Spring Bean으로 Controller를 등록한다
4. JSP를 이용한 View영역의 코드를 작성한다
5. 브라우져 상에서 JSP를 실행한다


# EL(Expression Language)
* <% %>과 같은 스크립팅 태그를 JSP에서 없애, 자바 코드를 최대한 없앨 수 있다
* EL 표현식은 중괄호로 묶고 앞에 달러($)기호를 붙으며, 도트 연산자를 사용한다
* EL은 저장 객체의 출력을 단순화하는 용도로 사용되므로, 저장 객체를 출력할 때도 스크립팅을 전혀 쓰지 않는다

    <%=request.getParameter("name")%> -->
    {$param.name}
    
    <% UserVO user = (UserVO)request.getAttribute("user");
        out.println(user); %> -->
    ${user}


* EL은 기본적으로 4가지 scope(page, request, session, application)의 객체에 접근하여 출력을 처리한다
* EL에서는 해당 값이 null이거나 공백일 경우에는 아무 내용도 표시하지 않고 에러도 발생하지 않는다
* EL은 JSP에서 기본으로 지원하고, JSTL은 따로 설치해야 한다

# JSTL(Java Standard Tag Library)


