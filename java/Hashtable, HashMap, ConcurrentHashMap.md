## Hashtable vs HashMap vs ConcurrentHashMap

> `Hashtable` vs `HashMap` vs `ConcurrentHashMap` </br>

</br>

#### 🔘OAuth 등장배경



---
#### 🔘용어정리

**Client** : OAuth 2.0을 사용해 서드파티 로그인 기능을 구현할 자사 또는 개인 애플리케이션 서버다.

**Resource Owner** : 서드파티 애플리케이션 (Google, Facebook 등)에 이미 개인정보를 저장(회원가입)하고 있으며 Client가 제공하는 서비스를 이용하려는 사용자, `Resource`는 개인정보라고 생각하면 된다.

**Resource Server** : 사용자의 개인정보를 가지고있는 애플리케이션 (Google, Facebook 등) 서버다. Client는 Token을 이 서버로 넘겨 개인정보를 응답 받을 수 있다.

**Authorization Server** : `Client`가 `Resource Server`의 서비스를 사용할 수 있게 인증하고 토큰을 발행해주는 서버(인증 서버)

**Access Token** : `Resource Server`와 `Client`사이에 인증을 완료하여 `Resource Server`로 부터 받은 권한부여의 결과이며, `Client`는 `Access Token`을 이용해서 `Resource Server`에 접근 및 서비스를 요청할 수 있다.

**Refresh Token** : 

---
#### 🔘OAuth 프로세스

