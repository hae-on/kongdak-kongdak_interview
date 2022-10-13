# CORS(Cross-Origin Resource Sharing)

## SOP(Same Origin Policy)

SOP는 다른 출처의 리소스를 사용하는 것을 제한하는 보안 방식입니다.

여기서 출처란 URL의 `Protocol`, `Host`, `Port`를 말합니다.

예를 들어 http://interview.com:30 이라는 주소가 있다고 합시다.

여기서 http는 프로토콜, interview.com은 호스트, 30은 포트라고 볼 수 있습니다.

즉, SOP는 `동일`한 프로토콜, 호스트, 포트의 리소스만 허용한다는 뜻입니다.

왜 다른 출처의 호출은 막아놓았을까요?

만약 `<script>`가 심어진 웹 페이지를 열었다고 가정해봅시다.

그 안에 정보를 탈취하는 요청이 들어있다면, 속수무책으로 당하겠죠.

따라서 다른 출처의 접근을 막기 위해 동일 출처 정책(SOP)가 등장했습니다.

## CORS란?

CORS는 `다른 출처`의 자원을 공유하는 것입니다.

매번 동일한 출처로만 요청과 리소스를 받을 수는 없겠죠.

다른 출처의 자원을 공유하기 위해 HTTP-header를 이용하고, 이를 통해 서버의 동의를 구할 수 있습니다.

이를 CORS(Cross-Origin Resource Sharing)라고 부릅니다.

## CORS의 기본 동작 과정

1. 클라이언트에서 HTTP 요청 헤더에 Origin을 담아 전달합니다.

다른 Origin의 리소스 요청시 클라이언트는 HTTP 요청을 보냅니다.

이때 요청 헤더에 Origin을 담아보냅니다.

2. 서버는 응답 헤더에 Access-Control-Allow-Origin을 담아 클라이언트로 전달합니다.

서버가 응답을 보낼 때, 허락하는 Origin을 클라이언트에 전달합니다.

3. 클라이언트에서 자신이 보냈던 요청의 Origin과 서버가 보내준 Access-Control-Allow-Origin을 비교합니다.

자신이 보냈던 요청의 Origin과 서버가 보내준 Access-Control-Allow-Origin을 비교하여 허용, 차단 여부를 결정합니다.

만약 유효하지 않다면 응답을 사용하지 않고 버립니다.

## CORS 정책 3가지

1. 단순 요청(Simple Request)

사전 요청(Preflight)없이 바로 요청을 보냅니다.

단순 요청(Simple Request)은 아래 조건을 만족해야 합니다.

- HTTP method가 다음 중 하나여야 합니다.

  - GET
  - HEAD
  - POST

- 다음 헤더 중 하나여야 합니다.

  - Accept
  - Accept-Language
  - Content-Language
  - Content-Type

- Content-Type은 다음 중 하나여야 합니다.

  - application/x-www-form-urlencoded
  - multipart/form-data
  - text/plain

2. 사전 요청(Preflight Request)

단순 요청이 아닌 요청은 사전 요청이라고 보면 됩니다.

OPTIONS 메서드를 통해 다른 도메인 리소스에 요청이 가능한지 확인하는 작업입니다.

요청이 가능하면 실제 요청을 보냅니다.

- Preflight Request(요청 헤더 목록)

  - Origin : 요청 출처
  - Access-Control-Request-Method : 실제 요청에서 어떤 메서드를 사용할 것인지 서버에게 알리기 위해 사용합니다.
  - Access-Control-Request-Headers : 실제 요청에서 어떤 헤더를 사용할 것인지 서버에게 알리기 위해 사용합니다.

- Preflight Response(응답 헤더 목록)

  - Access-Control-Allow-Origin : 브라우저가 해당 origin 자원에 접근할 수 있도록 허용합니다.
  - Access-Control-Allow-Methods : 사전 요청에 대한 응답으로 허용되는 메서드들을 나타냅니다.
  - Access-Control-Allow-Headers : 브라우저가 액세스할 수 있는 서버 화이트리스트 헤더를 사용합니다.
  - Access-Control-Max-Age : 얼마나 오랫동안 사전 요청이 캐싱 될 수 있는지 나타냅니다.

3. 인증정보요청(Credential Request)

인증 관련 헤더를 포함할 때 사용하는 요청입니다.

신용 정보는 쿠키, 인증 헤더, TLS 클라이언트 인증서가 될 수 있습니다.

- 클라이언트

  - 쿠키 또는 JWT 토큰을 담아 보낼 경우 credentials : include를 포함하여 보냅니다.

- 서버

  - Access-Control-Allow-Credentials : true일 때 클라이언트의 인증 포함 요청에 허용이 가능합니다.
