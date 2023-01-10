# DEEP DIVE : HTTPS와 TLS #2. TLS 핸드쉐이크

## TLS (Transport Layer Security)

- SSL(Secure Socket Layer) 1.0버전부터 시작해서 SSL 2.0, SSL 3.0, TLS 1.0을 거치고 TLS 1.3버전까지 올라감
- SSL에서 TLS로 명칭이 변경
- 전송 계층에서 보안을 제공하는 프로토콜
- 클라이언트와 서버가 통신 할 때 제3자가 메시지를 도청하거나 변조하지 못하도록 함

## TLS Handshake (TLS 1.3)

![Untitled](https://user-images.githubusercontent.com/47595515/211579215-7ca7ef3e-616d-49c4-b5ee-c2053b4f652f.png)

1. Client Hello
    - 클라이언트가 TLS버전, 암호화 스위트(Cipher Suite), 랜덤값(무작위 문자열), 임시 DH 매개변수를 서버에 보냄
2. Server Hello, EncryptedExtensions, Certificate, CertificateVerify
    - 서버는 클라이언트로부터 받은 옵션을 확인
    - 서버와 클라이언트 둘다 지원하는 가장 높은 TLS 버전을 식별하여 결정
    - Cipher Suite 지원 여부 확인
    - SSL 인증서, 서버 랜덤값, 임시 DH 매개변수를 클라이언트에 보냄
    - 클라이언트와 서버가 서로 교환한 DH 매개변수를 이용하여 세션키를 생성
3. Finished
    - 세션키를 기반으로 대칭 암호화된 통신이 시작 (보안 세션 시작)

<aside>
💡 키교환 알고리즘으로는 대표적으로 RSA와 DH가 존재

- RSA
    - RSA를 사용할 경우 SSL 인증서 안에 서버가 생성한 공개키를 포함
    - 취약점이 존재하기 때문에 TLS 1.3부터는 공식적으로 지원하지 않음
- DH
    - 클라이언트와 서버가 각각 생성한 매개변수를 교환하여 키를 생성
    - 매개변수는 중간에 노출되어도 상관 없음
    - 주로 타원곡선을 이용한 ECDHE(Elliptic Curve Diffie-Hellman Exchange)를 사용
</aside>

## DH 매개변수

![Untitled 1](https://user-images.githubusercontent.com/47595515/211579205-bc5786f0-d1ab-446b-832a-d6f767cdd8d1.png)

- a, b는 Alice와 Bob만이 알고있는 비밀값
- g, p는 공개값
- Alice가 g, p, a로 만든 매개변수 A를 공개값들과 함께 Bob에게 전송
- Bob은 g, p, b로 매개변수 B를 만들고 Alice에게 전송
- Alice와 Bob은 서로에게서 받은 매개변수로 공통의 키값인 K를 생성 가능

## 타원곡선 암호화 방법

![Untitled 2](https://user-images.githubusercontent.com/47595515/211579207-cabf7efd-ca2a-4954-b56b-17d02018e86d.png)

- 개인 키 보유자만 알 수 있는 타원곡선을 그림
- 타원곡선을 기반으로 교차점 생성
- 교차점의 수를 기반으로 암호를 설정하는 방법

## Cipher Suite

- 프로토콜, AEAD Cipher Mode, Hash 알고리즘이 나열된 규약
- 암호제품군이라고도 불리며 TLS 1.3에는 5개가 있다
    - TLS_AES_128_GCM_SHA256
    - TLS_AES_256_GCM_SHA384
    - TLS_CHACHA20_POLY1305_SHA256
    - TLS_AES_128_CCM_SHA256
    - TLS_AES_128_CCM_8_SHA256
- TLS_AES_128_GCM_SHA256에는 3가지 규약이 들어 있음을 의미
    - TLS는 프로토콜
    - AES_128_GCM은 AEAD Cipher Mode
    - SHA256은 Hash 알고리즘

### AEAD Cipher Mode

- 데이터 암호화 알고리즘
- AES_128_GCM이라는 것은 128비트 키를 사용하는 AES 알고리즘과 병렬 계산에 용이한 암호화 알고리즘 GCM이 결합된 알고리즘을 뜻함

### Hash 알고리즘

- 데이터를 추정하기 힘들게 더 작고, 섞여 있는 조각으로 만드는 알고리즘
- SHA-256인 경우 해시함수의 결과값이 256비트인 알고리즘

  ![Untitled 3](https://user-images.githubusercontent.com/47595515/211579212-1bce5a6e-7dd6-4415-bd65-a0b32dc4e605.png)
    
- Hash 알고리즘이 TLS에서 어떻게 쓰이는가
    - 인증서가 올바른 인증서인지 확인할때 사용하는 전자서명에 사용

      ![Untitled 4](https://user-images.githubusercontent.com/47595515/211579214-e46a5c33-b62a-472e-aa75-bff3ad46b2c2.png)
        
        1. 인증 생성작업 : 전자 서명을 만드는데 서명되는 메시지를 해싱
        2. 인증 확인작업 : 메시지를 복호화해서 해시를 서로 비교해 올바른 메시지인지 확인

## 인증서

- 주체, 공개키를 포함하는 단순한 데이터 파일
- 주체 : 인증서를 발급한 CA, 도메인, 웹사이트 소유자, 인증서 소유자
- 자신이 직접 SSL 인증서를 만들 수도 있지만 보통은 인증기관인 CA에서 발급한 SSL 인증서를 기반으로 인증작업 수행
- 주체는 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지 확인할 때 쓰임
- 공개키는 처음 인증작업을 수행할 때 쓰임

## CA (Certificate Authority)

- 인증서를 발급하는 기업들
- 서비스의 도메인, 공개키와 같은 정보를 서비스가 CA로부터 인증서를 구입할 때 제출해야함
- 다양한 유형의 인증서가 존재

    ![CA](https://user-images.githubusercontent.com/47595515/211579218-e68c7c97-23fe-42fa-aba8-73dcace37d66.PNG)
    
    - 단일 도메인 : 단 하나의 도메인에 적용되는 인증서
    - 와일드카드 : 도메인의 하위 도메인도 포함하는 인증서
    - 멀티 도메인 : 관련되지 않은 다수의 도메인에 적용될 수 있는 인증서

## RSA의 취약점

- RSA 키 교환 과정
    
    <img width="1047" alt="RSA" src="https://user-images.githubusercontent.com/47595515/211579220-11fd5bab-6fd5-4fc6-b571-67f101b502f0.PNG">
    
    - 서버에서 공개키와 개인키를 생성하고 SSL인증서를 통해 공개키를 클라이언트에게 전송
    - 클라이언트는 공개키로 세션키를 암호화 하여 서버에 전송
    - 서버는 세션키를 복호화 하여 통신에 사용
- PFS(Perfect Forward Secrecy)를 지원하지 않음
    - 제3자가 서버의 개인키가 탈취할 경우 SSL Handshake 과정의 모든 데이터를 복호화 하여 볼 수 있다
    - ECDHE 방식이 PFS를 지원
        - DH 방식은 세션키를 안전하고 간단하게 생성가능하기 때문에 주기적으로 세션키를 변경하여 일정주기마다 다른 세션키를 가짐
        - 공격자가 특정 시점의 데이터를 복호화하고 싶을 경우 특정시점의 세션키가 있어야 함

## 0-RTT

- 세션키가 생성된 이후 그 사이트를 재방문한다면 미리 만들어 놓은 세션키(PSK, Pre-Shared Key)를 기반으로 연결을 생성하기 때문에 인증에 드는 비용이 없다

    <img width="913" alt="0-RTT" src="https://user-images.githubusercontent.com/47595515/211579194-6b6c704d-f6b5-46f3-bbe6-da34dcb8b1a1.PNG">
    
- 민감한 금융정보를 다루는데 이점이 있고 HTTP/2를 구현할 수 있다
    - HTTP/2는 HTTPS위에서만 돌아간다