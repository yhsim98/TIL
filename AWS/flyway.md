# Flyway
Flyway는 오픈소스 DB 마이그레이션 tool이다.  
마치 git처럼 개발시 사용하는 Local의 DB의 schema의 변경이력관리, 배포시 사용하는 DB schema와 비교 및 변경사항 자동 적용 등등 여러가지 일들을 해준다.

특정 위치에 있는 SQL 파일들이나 Java classes 들을 마이그레이션 가능하다.  
spring 프로젝트의 경우 resources의 db.migrastion 디렉토리가 기본 경로로 설정되어 있다.  

# File naming
![](http://www.popit.kr/wp-content/uploads/2016/11/SqlMigrationNaming-600x185.png)
파일의 이름은 V혹은 R로 시작해야 한다.

* V의 경우는 버젼 마이그레이션이라는 뜻이다. 

