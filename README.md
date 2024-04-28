# web_oAuth2.0
basic study about web oAuth 2.0 (ft.Udemy [생활코딩] WEB - OAuth 2.0)

## OT
oAuth과 관련해서 3가지의 참여자가 존재합니다.

![image](https://github.com/SAMEZ-0129/web_oAuth2.0/assets/81644075/dd12e7f7-56da-4f11-aacd-f62cb1a18809)
서비스를 제공하는 나 / 내 서비스를 사용하는 사용자(user) / 사용자가 활용하는 타 서비스(외부 서비스)

ex. 내가 운영하는 서비스에서 사용자의 페이스북/구글/네이버 계정 아이디와 비밀번호를 전달받아(저장해서) 내 서비스에 로그인 할 수 있게 해줌

이와 같은 방법은 매우 간단하면서도 타 서비스의 기능을 모두 활용 가능하다는 점에서 매우 유용합니다.

하지만, 내 아이디와 비밀번호를 처음보는 서비스에 그냥 제공하는 것은 매우 찝찝한 일이고, 보안에 안좋은 영향을 준다 (신뢰가 없기에). 또한, 구글/페이스북/네이버 같은 타 서비스도 자신들의 사용자의 게정 정보를 신뢰할 수 없는 서비스에 제공한다는 것은 불만족스러운 일이 아닐 수 없다. 

이러한 상황을 위해 존재하는 것이 oAuth인 것이다. 그렇다면, oAuth를 활용하면 무엇이 다른가?

기존에는 사용자의 아이디/비밀번호를 내 서비스가 보관하고 있었다면, 이제는 구글같은 서비스에서 사용자의 요청에 의해 accessToken이라는 일종의 비밀번호를 대신하는 인증서를 발급합니다. 

accessToken의 장점으로는
- 본인들의 서비스의 실제 아이디/비밀번호가 아님
- 본인들의 서비스의 모든 기능을 제공하지 않고, 필요한 기능만 제공할 수 있다
- 전달받은 accessToken을 통해서 구글같은 서비스에 접근해서 데이터를 가져오거나 수정, 생성, 삭제 등이 가능하다

## 본문
### oAuth에 등장하는 3개의 주체

우리가 만든 서비스 = 내것,
우리의 서비스를 사용하는 사람 = 유저,
유저가 사용하는 타 서비스 = 그들

 그들(타 서비스)를 우리가 원하는 자원을 갖는 서버라는 의미에서 **'리소스 서버'** 라고 부름

리소스 서버에 가입이 되어있는 유저 = **리소스 소유자**

우리의 서비스 =  **클라이언트**

추가로 리소스 서버는 우리가 원하는 데이터를 가진 서버를 의미하고 authorization server라는 인증서버는 인증과 관련된 처리를 위해 존재하는 서버임(oAuth 공식 메뉴얼)

이번 강의에서는 두 서버를 구분하지 않고 편의상 합쳐서 리소스 서버라고 부르자

### oAuth를 등록하는 절차 - 등록
클라이언트가 리소스 서버를 이용하기 위해서는 리소스 서버로부터 승인을 사전에 받아야 한다 = 등록

서비스마다 등록하는 방법의 차이는 있지만, 공통적인 부분은 ClientID와 ClientSecret 그리고 Authorized Redirect URLs를 전달 받는 것은 동일하다

ClientID: 우리가 만든 애플리케이션의 식별자/ID

ClientSecret: 해당 ID의 비밀번호 (ClientID는 외부에 노출되어도 이슈가 아니지만, ClientSecret은 절대 노출되선 안된다)

Authorized Redirect URLs: 리소스 서버가 권한을 부여하는 과정에서 Authorize Code라는 값을 전달해 주는데, 해당 값을 전달받을 주소(리소스 서버는 해당 URL이 아닌 다른 곳에서의 요청은 무시)

### 인증 받는 방법
등록하는 절차를 통해 리소스 서버와 클라이언트는 ClientID와 ClientSecret을 서로 공통적으로 인지하고 있는 상황

![image](https://github.com/SAMEZ-0129/web_oAuth2.0/assets/81644075/26244e7e-4d0f-4926-b80c-c5529a21c637)

리소스 서버에 특정 기능만 사용이 필요한 상황이면 모든 기능에 대한 인증을 받는 것이 아닌, 필요한 인증만 받는게 클라이언트와 리소스 서버 서로 이득

유저가 클라이언트로 로그인을 한다고 가정하면, 해당 기능을 위해 서비스(구글,페이스북 등)의 로그인 버튼을 만들고 사진과 같이 리소스 서버로 인증을 요청하는 방식을 사용한다

유저가 로그인을 진행 > 요청하는 서비스에 유저가 로그인되어있지 않을 경우, 로그인화면 노출 / 로그인된 상태라면 ClientID값을 확인하고 해당 값과 동일한 값이 서버에 있는지 확인 > 서버가 가지고 있는 해당 클라이언트의 정보 중 redirect URL과 실제 요청이 이루어지고 있는 redirect URL을 비교한다 > 같을 경우 클라이언트가 요청하는 권한을 부여할 것인지 유저한테 물어본다(아래 이미지 참고)

![image](https://github.com/SAMEZ-0129/web_oAuth2.0/assets/81644075/62e6c3f5-9812-4a0a-8bc7-2927c80e9bd1)

권한을 부여하는것에 클라이언트가 동의할 경우, 리소스 서버는 해당 작업을 기록해서 저장한다 (유저의 아이디가 1이라고 가정)

![image](https://github.com/SAMEZ-0129/web_oAuth2.0/assets/81644075/028c9c01-e7de-4ccc-ab79-a4e2cb1b70ae)

### 리소스 서버의 승인

이제 리소스 서버의 승인이 필효하다. 리소스 서버는 사용자로부터 인증을 받았으니, 인증을 받은 것을 증명하는 authorization code:3 이라는 임시 비밀번호를 생성하고 리소스 사용자한테 전달한다.

이때 리다이렉트 하라는 명령과 주소를 같이 보내게 된다. 

![image](https://github.com/SAMEZ-0129/web_oAuth2.0/assets/81644075/809478b1-a11a-4ba9-9ce5-03503828a734)

해당 주소로 이동하게 될 경우 인증 코드가 3이라는 정보가 포함되어 이동하게 되고, 클라이언트는 이를 전달받게 된다.

이제 클라이언트는 전달받은 정보를 바탕으로 리소스 서버에 직접 접속하게 되는데, 이 때 접속하는 링크에는 사용자 정보가 포함되어 전달되며 요청을 하게 된다. 

리소스 서버는 클라이언트가 전달한 정보를 바탕으로 서버에 저장된 정보와 일치하는지 확인하고 일치할 경우 Access Token을 발급하는 과정으로 넘어간다.

### Access Token 발급
리소스 서버는 authorization code값을 통해 클라이언트와 유저에 대한 인증을 마쳤기 때문에, 해당 정보는 리소스 서버와 클라이언트에서 삭제한다.

그리고 리소스 서버는 Access Token:4 라는 정보를 생성한다.

![image](https://github.com/SAMEZ-0129/web_oAuth2.0/assets/81644075/9c465035-fafc-4d56-8307-7c71c40c0284)

해당 Access Token을 클라이언트에 전달하고 앞으로 해당 인증 토큰값으로 요청을 하면 기존에 저장된 정보를 확인해서 권한을 허용하게 되는 것.

### API 호출
궁극적으로 Client는 리소스 서버가 가진 어떤 기능을 사용하기 위함이다.

이를 가능하게 해주는 것이 바로 API(Application Programming Interface)라고 한다.

리소스 서버는 특정 API를 사용하는 것에 대한 설명을 문서로 가지고 있습니다. (ex. 개발자센터, API 문서)

문서를 학습해서 API를 통해 리소스를 요청하는 방법을 학습하는 것이 중요하다. 

### Refresh Token
Access Token은 수명이 있습니다. 해당 수명이 끝나면, API를 접속할 때 데이터를 받을 수 없다.

그렇다고 매번 새로 인증받기에는 매우 불편하다: 이때 사용하는 것이 Refresh Token이다.

![image](https://github.com/SAMEZ-0129/web_oAuth2.0/assets/81644075/2fd3d13b-7f5c-4342-a1b9-a2f99f195e62)

일반적으로 Access Token이 발급될 때 Refresh Token도 같이 발급된다. 이후 Access Token이 만료될 경우(유효하지 않은 토큰 에러를 리턴받으면) 가지고 있던 있던 Refresh Token을 통해 Access Token을 재발급해서 API를 계속 사용할 수 있도록 해준다.

(구글의 Refreshing an access token 이라는 문서 처럼 액세스 토큰을 리프레시 하는 방법이 따로 존재하며, 서비스 마다 재발급 하는 방법은 상이할 수 있다)

## 요약
1. 서비스 개발자/서버
Client가 어떤 서비스를 제공하려고 할 때 기능을 가지고 있는 Resource Server(예를 들어 Facebook 등)의 해당 기능을 이용하려는 것입니다. 이를 통해 Client는 사용자인 Resource Owner가 API를 사용할 수 있도록 합니다.

그 API를 이용하려면 먼저 Resource Sever로 등록이 필요합니다. 등록을 통해서 Client는 Client ID와 Client Secret을 받게 됩니다. 동시에 redirect URL도 받습니다.

2. 사용자/유저
앞서 내용을 남긴 개발자 입장은 사용자 입장에서 신경쓰지 않습니다. 개발자 입장은 단지 서비스를 제공하기 위해서 다른 누군가의 API를 사용할 것이고, 이를 위해서 등록을 했을 뿐입니다.

먼저 누군가가 Client가 제공하는 서비스를 이용하고 싶어한다고 가정하겠습니다. Client는 서비스 이용을 위해 Client에 접속하려고 하는데, 이와 같은 움직임이 발생하면 사용자는 선택권을 갖게 됩니다. 만약 사용자가 Resource Server에 로그인되어있지 않다면, 로그인을 요구한 후 사용자가 이용하려는 Client의 서비스가 Resource Server에서 제공하는 기능에 접근하려고함을 알려줍니다. 이때 사용자가 수락하면 사용자는 결국 Resource Server에 있는 API를 Client를 통해 사용하겠다는 의미가 됩니다.

3. Resource Server/제3자 서비스
앞서 개발자 입장에 내용을 적지는 않았지만, 개발자나 Resource Server는 모두 사용자의 정보에 대한 보안에 신경쓰려합니다. Resource Server는 마지막에 Access Token을 발급할 것인데, 이 Access Token이 OAuth의 핵심임을 알고 있습니다. 사용자의 ID와 Password를 직접 다루지 않게 해서 보안에 신경쓴다는 의미입니다.

다시 돌아와서 순서대로 살펴보면, 사용자는 궁극적으로 Resource Server의 API를 사용하려는 것이고 그렇게 하기까지 보안에 신경쓴 움직임이 이뤄져야 합니다. 먼저 Resource Server는 Authorization Code라는 임시 비밀번호를 생성하고 사용자의 웹 브라우저에게 Redirection할 것을 명령합니다. Authorization Code를 갖고 Client에게 다시 연결하면, Client는 Resource Server에게 직접 접속을 시도합니다.

Resource Server는 지금까지 움직임에서 많은 정보를 알고 있습니다. Client로부터 넘겨받은 Authorization Code, Client Secret 두 가지를 가지고 자신이 갖고 있었던 것과 일치하는지 확인합니다. Authorization Code, Redirect URL, Cliend ID, Client Secret 모두가 일치하면 다음 단계로 넘어갑니다.

여기서 잠깐 Authorization Code의 흐름을 다시 살펴보면 아래와 같습니다.

Authorization Code는 Resource Server로부터 생성되어 사용자에게 전달합니다.
동시에 Resource Server는 사용자의 웹 브라우저에게 Authorization Code를 갖고 Redirection할 것을 명령합니다.
Client는 웹 브라우저로부터 받은 Authorization Code를 갖고 Resource Server에 접근합니다.
마지막으로 Resource Server는 그동안 전달되었던 정보를 가지고 일치하는지 여부를 판단합니다.
Authorization Code의 사용은 끝나고 이제 소멸됩니다. 그리고 Resource Server는 Client에게 Access Token을 보내 Access Token을 가지고 있는 사용자가 API를 이용할 수 있도록 허용합니다.
