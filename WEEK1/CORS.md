# CORS는 뭔가요?

피싱 사이트로 접속했을 때 HTML, CSS, **JavaScript** 코드를 브라우저에 다운로드 하게 됩니다. 이 때 다운로드 된 자바스크립트 코드로 로그인했던 세션 또는 토큰을 탈취하고 악의적인 동작을 수행할 수 있습니다.

이것을 방지하기 위해 **다른 출처(도메인, 프로토콜, 포트 번호)인 경우** 브라우저에서는 **SOP(Same-Origin Policy 동일 출처 정책)** 정책에 따라 요청이 가지 못하도록 막고 있습니다.

하지만 이러한 제한 사항이 있으면, 다른 출처의 리소스를 필요로 하는 웹 애플리케이션은 제대로 작동하지 않습니다. 이 때 필요한 것이 CORS입니다. CORS는 Cross-Origin Resource Sharing의 약자로, 다른 출처간에 리소스를 공유할 수 있도록 하는 정책입니다.

## 출처(Origin)란?

![cors](https://github.com/ValueWith/ValueWith_Study/assets/110911811/1d20ce96-9e8b-40a4-8d1d-d276eb014c9d)

http의 포트 번호인 80번과 https의 포트 번호인 443번은 생략될 수 있습니다.

CORS에서 말하는 출처란 웹 리소스가 어디에서 왔는지 확인하는 식별자로 웹 리소스의 URL 구조에서 **확인할 수 있는** 호스트, 프로토콜, 포트 번호 3가지 요소를 가지고 판단합니다.

세 가지 요소가 모두 같아야 같은 출처로 인정되며, 하나라도 다르면 브라우저는 그것을 다른 출처로 간주합니다.

❗️개발 환경에 한해서는 프론트엔드에서도 프록시 설정을 통해 CORS를 우회할 수 있습니다

---

## 출처 허용

CORS 에러를 해결하기 위해서는 **요청을 받는 서버에서 자원 공유를 허락할 출처들을 미리 명시**해주어야 합니다.

브라우저에서는 다른 출처로 요청을 보낼 때, 요청하는 쪽의 프로토콜, 도메인, 포트 정보를 Origin이라는 header에 담아 전송합니다.

이 요청을 받은 서버는 Response의 header에 지정된 Access-Control-Allow-Origin 정보를 담아 보내게 되는데 이 때 브라우저가 요청하는 쪽에서 보낸 출처가 Access-Control-Allow-Origin에도 있으면 안전한 요청으로 간주하고 응답 데이터를 불러오게 됩니다.

## 프리플라이트 요청(Preflight Request)

단순 요청이 아닌 경우 서버의 데이터에 영향을 줄 수 있는 요청들이기 때문에 허용된 출처에서 허용된 메서드로 전송하고 있는지 한 번 더 검증하는 과정을 거치게 됩니다. 본 요청을 보내기 전 요청이 안전한지 확인하기 위해 프리플라이트 요청을 OPTIONS 메서드를 이용해 보내게 됩니다.

프리플라이트 요청의 헤더에는 다음과 같은 것들이 담기게 됩니다.

- Origin(출처)
- Access-Control-Request-Method(본 요청의 메서드)
- Access-Control-Request-Headers(본 요청의 추가헤더)

프리플라이트 요청을 받은 서버는 응답에 다음과 같은 것들을 담아 회신합니다.

- Access-Control-Allow-Origin(서버 측 허가 출처)
- Access-Control-Allow-Methods(서버 측 허가 메서드)
- Access-Control-Allow-Headers(서버 측 허가 헤더)
- Access-Control-Max-Age(프리플라이트 응답 캐시 기간)

프리플라이트 요청에서 응답 스테이터스 200이 떨어지게 되면 본 요청을 보내게 됩니다.

## 단순 요청(Simple Request)

특정 조건을 **모두** 만족했을 경우 프라플라이트 요청(프리플라이트 요청은 다음 단락에서 설명)을 생략하고 서버로 데이터를 전송할 수 있습니다. 이를 단순 요청이라고 부르는데, 단순 요청의 조건은 다음과 같습니다.

1. GET, HEAD, POST 중 하나의 메서드
2. 유저 에이전트(브라우저)가 자동으로 설정한 헤더 외 수동으로 설정한 헤더가 다음 중 하나인 경우
   1. Accept, Accept-Language, Content-Language, Content-Type
   2. 단 Content-Type은 다음 중 하나여야 합니다.
   - `application/x-www-form-urlencoded`
   - `multipart/form-data`
   - `text/plain`

## 인증정보 포함 요청(Credentialed Request)

토큰 등 사용자 식별 정보가 담긴 요청에 대해서는 보다 엄격하게 관리하게 되는데

요청하는 쪽에서는 header에 credentials 항목을 true로 세팅하고, 받는 쪽에서는 와일드카드를 사용하는 것이 아니라 보내는 쪽의 출처를 정확히 명시하고 Access-Control-Allow-Origin를 true로 세팅해야 합니다.
