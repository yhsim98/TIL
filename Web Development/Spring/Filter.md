# 필터
HTTP 요청과 응답을 변경할 수 있는 재사용 가능한 코드이며, 서블릿 2.3 규약에 새로 추가되었다. 필터는 객체의 형태로 존재하며 클라이언트로부터 오는 요청(request)과 최종자원(서블랏, jsp, ) 사이에 위치하며 **클라이언트의 요청 정보를 알맞게 변경**할 수 있으며, 또한 필터는 최종 자원과 클라이언트로 가는 응답(response) 사이에 위치하여 **최종 자원의 요청 결과를 알맞게 변경**할 수 있다.

# 필터 관련 인터페이스 및 클래스
필터를 구현하는데 핵심적인 역할을 하는 인터페이스 및 클래스는 3개가 있다.

1. javax.servlet.Filter 인터페이스
    * 클라이언트와 최종 자원 사이에 위치하는 필터를 나타내는 객체가 구현해야 하는 인터페이스
2. javax.servlet.ServletRequestWrapper 클래스
    * 필터가 요청을 변경한 결과를 저장할 래퍼 클래스를 나타냄
3. javax.servlet.ServletResponseWrapper 클래스
    * 필터가 응답을 변경한 결과를 저장할 래퍼 클래스를 나타냄

## Filter interface
* public void init(FilterConfig filterConfig) throw ServletException
    * 필터를 웹 컨테이너에 생성한 후 초기화할 때 호출
* public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws ......
    * 체인을 따라 다음에 존재하는 필터로 이동한다. 체인의 가장 마지막에는 클라이언트가 요청한 최종 자원이 위치한다
* public void destroy() 
    * 필터가 웹 컨테이너에서 삭제될 때 호출한다

위 메소드에서 doFilter() 메소드가 필터의 역할을 한다. 서블릿 컨테이너는 사용자가 특정한 자원 요청 시 그 자원 사이에 필터가 존재하는 경우 그 필터 객체의 doFilter() 메소드를 호출하며, 바로 그 시점부터 필터가 작용하기 시작한다.

    public void doFilter(ServletRequest request,
                          ServletResponse response,
                          FilterChain chain)
                          throws IOException, ServletException {
        chain.doFilter(request, response);       
    }

만약 클라이언트의 자원 요청이 필터를 거치는 경우, **클라이언트의 요청이 있을 때마다 doFilter() 메소드가 호출**된다. 위 코드에서 doFilter() 메소드의 세 번째 파라미터로 FilterChain 객체를 전달받는데, 이는 클라이언트가 요청한 자원에 이르기까지 클라이언트 요청이 거쳐가게 될 필터 체인을 나타낸다. **FilterChaing을 사용하여 필터는 체인에 있는 다음 필터에 변경한 요청과 응답을 건내줄 수 있다.**


# 설정하기
WEB-INF에 있는 web.xml 파일을 통해 설정해야 한다.

    <web-app>
     
     <filter>
        <filter-name>HighlightFilter</filter-name>
        <filter-class>javacan.filter.HighlightFilter</filter-class>
        <init-param>
            <param-name>paramName</param-name>
            <param-value>value</param-value>
        </init-param>
    </filter>
     
        <filter-mapping>
            <filter-name>HighlightFilter</filter-name>
            <url-pattern>*.txt</url-pattern>
        </filter-mapping>
     
    </web-app>

* filter : 어플리케이션에서 사용될 필터를 지정
* filter-name : 특정 자원에 대해서 어떤 필터를 사용할지 지정
* init-param : init 메소드가 호출될 때 전달되는 파라미터 값, 주로 필터를 사용하기 전에 초기화해야하는 객체나 자원을 할당할 때 필여한 정보를 제공하기 위해 사용된다
* filter-mapping : 클라이언트가 요청한 특정 URI에 대해 필터링 할 때 사용
    * '/'로 시작하고 '/*'로 끝나는 url-pattern은 경로 매핑 시 사용됨
    * '*'로 시작하는 url-pattern은 확장자에 대한 매핑 시 사용됨
    * 나머지 다른 문자열은 매핑을 위해 사용됨