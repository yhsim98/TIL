# AWS CloudFront
AWS에서 제공하는 CDN(Content Delivery Network) 서비스이다. CDN 서비스를 사용하면 서비스 대기 시간과 성능이 개선되어 이미지, 오디오, 비디오 및 일반 웹 페이지 등을 최종 사용자에게 빠르게 제공할 수 있다.

Cloud Front는 엣지 로케이션이라고 하는 데이터 센터의 전 세계 네트워크를 통해 콘텐츠를 제공

사용자가 콘텐츠를 요청하면 지연 시간이 가장 낮은 엣지 로케이션으로 요청이 라우팅되어 가능한 최고의 성능으로 콘텐츠가 제공된다.


## CDN
사용자가 원격지에 있는 서버(Origin Server)로 부터 Content(ex Web Object, Video, Music, Image, Document 등)를 다운로드 받을때 가까이 있는 서버에서 받는 것보다 시간이 오래 걸린다. 
그러므로 사용자와 가까운 곳에 위치한 Cache Server에 해당 Content를 저장(캐싱)하고 Content 요청시에 Cache Server가 응답을 주는 기술이다.

## S3 버킷에 CloudFront 추가
Amazon S3 버킷에 객체를 저장하는 경우 사용자에게 S3에서 객체를 직접 가져오거나, S3에서 객체를 가져와 사용자들에게 배포하도록 CloudFront를 구성할 수 있다.

사용자가 객체에 자주 엑세스하는 경우 CloudFront를 사용하는 것이 비용 효율적일 수 있다.

가격도 더 싸고 객체가 사용자 측에 더 가깝게 저장되므로 다운로드 속도도 더 빠르기 때문이다.


## 설정값
### 배포 생성
* 원본 도메인
    * CDN은 원본 서버에서 각 지역의 서버로 배포해주는 서비스이다
    * 원본 서버로 사용할 도메인 이름을 선택하면 된다
* 원본 경로
    * Aws 리소스나 사용자 지정 Origin에 있는 디렉터리에서 콘텐츠를 요청하게 하려면 /뒤에 디렉터리 경로를 입력한다
    * 예를 들어 /production을 선택했다면 {원본 도메인}/production/{---} 으로 요청이 된다
* 이름 
    * AWS서비스를 선택할 경우 자동으로 들어간다
    * 원본(Origin)을 고유하게 구별하는 문자열
* S3 버킷 엑세스
    * yse면 cdn에서만 s3에 접속가능
    * no면 직접 접속 가능

### 배포 설정 - 캐시
* path Pattern
    * CloudFront로 파일을 가져올 규칙
    * 새 배포를 만들 때 기본값은 *(모든 파일)이며, 이 값은 여기서 수정할 수는 없고, 배포 생성 후 추가할 수 있다
* Viewer Protocol Policy
    * 사용자가 CloudFront 콘텐츠를 액세스 할 때의 프로토콜 설정
    * HTTPS only 등
* Allowed Http Methods
    * CloudFront에서 Origin을 처리하기 위해 허용할 HTTP 메소드
* Field-level Encryption Config
    * 중요한 데이터에 대한 보안을 위해 필드 레벨 암호화 적용 가능
* Cache Based on Selected Request Headers
    * CloudFront에서 지정된 헤더의 값을 기반으로 객체를 캐싱할지 여부를 지정합니다
        * None
            * 항상 캐시
        * WhiteList
            * 지정된 값을 기반으로 객체를 캐시합니다
        * 모두
            * 캐싱하지 않고 모든 요청을 origin 으로 전송합니다
* Object Caching
    * Origin 서버에서 객체에 Cache-Control 헤더를 추가하여 


