---
title: "[http error] 클라이언트 에러 응답 코드 종류"
date: 2021-02-14 00:26
categories: etc
---
## 클라이언트 에러 응답

- 참고문서 → [here](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)

* [400 Bad Request](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/400)

이 응답은 잘못된 문법으로 인하여 서버가 요청을 이해할 수 없음을 의미합니다.

* [`401 Unauthorized`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/401)

비록 HTTP 표준에서는 "미승인(unauthorized)"를 명확히 하고 있지만, 의미상 이 응답은 "비인증(unauthenticated)"을 의미합니다. 클라이언트는 요청한 응답을 받기 위해서는 반드시 스스로를 인증해야 합니다.

* `402 Payment Required`

이 응답 코드는 나중에 사용될 것을 대비해 예약되었습니다. 첫 목표로는 디지털 결제 시스템에 사용하기 위하여 만들어졌지만 지금 사용되고 있지는 않습니다.

* [`403 Forbidden`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/403)

클라이언트는 콘텐츠에 접근할 권리를 가지고 있지 않습니다. 예를들어 그들은 미승인이어서 서버는 거절을 위한 적절한 응답을 보냅니다. 401과 다른 점은 서버가 클라이언트가 누구인지 알고 있습니다.

* [`404 Not Found`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/404)

서버는 요청받은 리소스를 찾을 수 없습니다. 브라우저에서는 알려지지 않은 URL을 의미합니다. 이것은 API에서 종점은 적절하지만 리소스 자체는 존재하지 않음을 의미할 수도 있습니다. 서버들은 인증받지 않은 클라이언트로부터 리소스를 숨기기 위하여 이 응답을 403 대신에 전송할 수도 있습니다. 이 응답 코드는 웹에서 반복적으로 발생하기 때문에 가장 유명할지도 모릅니다.

* [`405 Method Not Allowed`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/405)

요청한 메소드는 서버에서 알고 있지만, 제거되었고 사용할 수 없습니다. 예를 들어, 어떤 API에서 리소스를 삭제하는 것을 금지할 수 있습니다. 필수적인 메소드인 `GET`과 `HEAD`는 제거될 수 없으며 이 에러 코드를 리턴할 수 없습니다.

* [`406 Not Acceptable`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/406)

이 응답은 서버가 [서버 주도 콘텐츠 협상](https://developer.mozilla.org/ko/docs/Web/HTTP/Content_negotiation#%EC%84%9C%EB%B2%84_%EC%A3%BC%EB%8F%84_%EC%BB%A8%ED%85%90%EC%B8%A0_%ED%98%91%EC%83%81) 을 수행한 이후, 사용자 에이전트에서 정해준 규격에 따른 어떠한 콘텐츠도 찾지 않았을 때, 웹서버가 보냅니다.

* [`407 Proxy Authentication Required`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/407)

이것은 401과 비슷하지만 프록시에 의해 완료된 인증이 필요합니다.

* [`408 Request Timeout`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/408)

이 응답은 요청을 한지 시간이 오래된 연결에 일부 서버가 전송하며, 어떨 때에는 이전에 클라이언트로부터 어떠한 요청이 없었다고 하더라도 보내지기도 합니다. 이것은 서버가 사용되지 않는 연결을 끊고 싶어한다는 것을 의미합니다. 이 응답은 특정 몇몇 브라우저에서 빈번하게 보이는데, Chrome, Firefox 27+, 또는 IE9와 같은 웹서핑 속도를 올리기 위해 HTTP 사전 연결 메카니즘을 사용하는 브라우저들이 해당됩니다. 또한 일부 서버는 이 메시지를 보내지 않고 연결을 끊어버리기도 합니다.

* [`409 Conflict`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/409)

이 응답은 요청이 현재 서버의 상태와 충돌될 때 보냅니다.

* [`410 Gone`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/410)

이 응답은 요청한 콘텐츠가 서버에서 영구적으로 삭제되었으며, 전달해 줄 수 있는 주소 역시 존재하지 않을 때 보냅니다. 클라이언트가 그들의 캐쉬와 리소스에 대한 링크를 지우기를 기대합니다. HTTP 기술 사양은 이 상태 코드가 "일시적인, 홍보용 서비스"에 사용되기를 기대합니다. API는 알려진 리소스가 이 상태 코드와 함께 삭제되었다고 강요해서는 안된다.

* [`411 Length Required`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/411)

서버에서 필요로 하는 `Content-Length` 헤더 필드가 정의되지 않은 요청이 들어왔기 때문에 서버가 요청을 거절합니다.

* [`412 Precondition Failed`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/412)

클라이언트의 헤더에 있는 전제조건은 서버의 전제조건에 적절하지 않습니다.

* [`413 Payload Too Large`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/413)

요청 엔티티는 서버에서 정의한 한계보다 큽니다; 서버는 연결을 끊거나 혹은 `Retry-After` 헤더 필드로 돌려보낼 것이다.

* [`414 URI Too Long`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/414)

클라이언트가 요청한 URI는 서버에서 처리하지 않기로 한 길이보다 깁니다.

* [`415 Unsupported Media Type``](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/415)

요청한 미디어 포맷은 서버에서 지원하지 않습니다, 서버는 해당 요청을 거절할 것입니다.

* [`416 Requested Range Not Satisfiable`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/416)

헤더 필드에 요청한 지정 범위를 만족시킬 수 없습니다; 범위가 타겟 URI 데이터의 크기를 벗어났을 가능성이 있습니다.

* [`417 Expectation Failed`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/417)

이 응답 코드는 `Expect` 요청 헤더 필드로 요청한 예상이 서버에서는 적당하지 않음을 알려줍니다.

* [`418 I'm a teapot`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/418)

서버는 커피를 찻 주전자에 끓이는 것을 거절합니다.

* [`421 Misdirected Request`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/421)

서버로 유도된 요청은 응답을 생성할 수 없습니다. 이것은 서버에서 요청 URI와 연결된 스킴과 권한을 구성하여 응답을 생성할 수 없을 때 보내집니다.

* [`422 Unprocessable Entity`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/422)

([`WebDAV`](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))요청은 잘 만들어졌지만, 문법 오류로 인하여 따를 수 없습니다.

* [`423 Locked`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/423)

([`WebDAV`](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))리소스는 접근하는 것이 잠겨있습니다.

* [`424 Failed Dependency`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/424)

([`WebDAV`](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))이전 요청이 실패하였기 때문에 지금의 요청도 실패하였습니다.

* [`426 Upgrade Required`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/426)

서버는 지금의 프로토콜을 사용하여 요청을 처리하는 것을 거절하였지만, 클라이언트가 다른 프로토콜로 업그레이드를 하면 처리를 할지도 모릅니다. 서버는 `[Upgrade](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Upgrade)` 헤더와 필요로 하는 프로토콜을 알려주기 위해 426 응답에 보냅니다.

* [`428 Precondition Required`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/428)

오리진 서버는 요청이 조건적이어야 합니다. 클라이언트가 리소스를 GET해서, 수정하고, 그리고 PUT으로 서버에 돌려놓는 동안 서드파티가 서버의 상태를 수정하여 발생하는 충돌인 '업데이트 상실'을 예방하기 위한 목적입니다.

* [`429 Too Many Requests`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/429)

사용자가 지정된 시간에 너무 많은 요청을 보냈습니다("rate limiting").

* [`431 Request Header Fields Too Large`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/431)

요청한 헤더 필드가 너무 크기 때문에 서버는 요청을 처리하지 않을 것입니다. 요청은 크기를 줄인 다음에 다시 전송해야 합니다.

* [`451 Unavailable For Legal Reasons`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/451)

사용자가 요청한 것은 정부에 의해 검열된 웹 페이지와 같은 불법적인 리소스입니다.

## 서버 에러 응답

* [`500 Internal Server Error`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/500)` → 참고 문서 [here](https://dualist.tistory.com/90)

서버가 처리 방법을 모르는 상황이 발생했습니다. 서버는 아직 처리 방법을 알 수 없습니다.

* [`501 Not Implemented`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/501)

요청 방법은 서버에서 지원되지 않으므로 처리할 수 없습니다. 서버가 지원해야 하는 유일한 방법은 `GET`와 `HEAD`이다. 이 코드는 반환하면 안됩니다.

* [`502 Bad Gateway`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/502)

이 오류 응답은 서버가 요청을 처리하는 데 필요한 응답을 얻기 위해 게이트웨이로 작업하는 동안 잘못된 응답을 수신했음을 의미합니다.

* [`503 Service Unavailable`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/503)

서버가 요청을 처리할 준비가 되지 않았습니다. 일반적인 원인은 유지보수를 위해 작동이 중단되거나 과부하가 걸렸을 때 입니다. 이 응답과 함께 문제를 설명하는 사용자 친화적인 페이지가 전송되어야 한다는 점에 유의하십시오. 이 응답은 임시 조건에 사용되어야 하며, `Retry-After:` HTTP 헤더는 가능하면 서비스를 복구하기 전 예상 시간을 포함해야 합니다. 웹마스터는 또한 이러한 일시적인 조건 응답을 캐시하지 않아야 하므로 이 응답과 함께 전송되는 캐싱 관련 헤더에 대해서도 주의해야 합니다.

* [`504 Gateway Timeout`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/504)

이 오류 응답은 서버가 게이트웨이 역할을 하고 있으며 적시에 응답을 받을 수 없을 때 주어집니다.

* [`505 HTTP Version Not Supported`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/505)

요청에 사용된 HTTP 버전은 서버에서 지원되지 않습니다.

* [`506 Variant Also Negotiates`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/506)

서버에 내부 구성 오류가 있다. 즉, 요청을 위한 투명한 컨텐츠 협상이 순환 참조로 이어진다.

* [`507 Insufficient Storage`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/507)

서버에 내부 구성 오류가 있다. 즉, 선택한 가변 리소스는 투명한 콘텐츠 협상에 참여하도록 구성되므로 협상 프로세스의 적절한 종료 지점이 아닙니다.

* [`508 Loop Detected`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/508`)

([`WebDAV`](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))서버가 요청을 처리하는 동안 무한 루프를 감지했습니다.

* [`510 Not Extended`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/510)

서버가 요청을 이행하려면 요청에 대한 추가 확장이 필요합니다.

* [`511 Network Authentication Required`](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/511)

511 상태 코드는 클라이언트가 네트워크 액세스를 얻기 위해 인증을 받아야 할 필요가 있음을 나타냅니다.
