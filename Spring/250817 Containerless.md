# Containerless

컨테이너가 필요 없다는 의미가 아니라, **컨테이너를 신경쓸 필요 없게 해 준다**는 뜻.
(Serverless의 개념과 비슷)
- Serverless: 서버에 대한 설치, 관리를 개발자가 신경 안써도 서버 애플리케이션 배포, 운영 가능
- Containerless: 개발자가 컨테이너를 신경 안쓰고 개발, 운영 가능하도록 해 주는 것.

## 스프링 이전부터 존재했던 개념들

### Servlet
- 어떤 기능을 처리해 주는 web component(다이내믹 컨텐츠를 만드는 일을 함)가 자바 용어 중에는 servlet에 해당된다.
- 서블릿은 request-response 모델 애플리케이션을 호스팅하는 서버(주로 웹 서버)의 기능을 확장하는 자바 클래스다.

### 웹 서버, 웹 애플리케이션, 웹 컨테이너
<img width="703" height="385" alt="image" src="https://github.com/user-attachments/assets/aa6ff136-a355-4f26-b8b2-943abc4666e2" />

- 웹 서버: 클라이언트가 보낸 HTTP 요청을 처리하고, 요청에 대한 HTTP 응답을 보내 준다.
- 웹 애플리케이션: 서블릿의 집합
- 웹 컨테이너(Servlet Container)
  - 웹 컨테이너는 웹 서버 중 서블릿과 상호작용하는 부분을 의미한다.
  - 서블릿의 라이프사이클을 관리한다.
  - 클라이언트에서 요처이 들어오면 어떤 웹 컴포넌트가 요청을 처리할 것인지를 정해준다.(handling, maping)
  - 서블릿 컨테이너의 예: tomcat

## Spring Container
서블릿에 대해 불만을 가진('*서블릿은 너무 제한적이다. 좀 더 나은 방법으로 개발 할 수 있었으면 좋겠다*') 사람들이 만든 것이 Spring 프레임워크이다.

<img width="993" height="510" alt="image" src="https://github.com/user-attachments/assets/788a8c02-181d-46d4-9d1c-2543f41aac5f" />

- Spring Container는 서블릿 컨테이너 뒤쪽에 존재.
- 자바 Bean에서 따온 Bean이라는 용어 사용. 어떤 기능을 담당하는, 스프링 컨테이너에 들어가는 컴포넌트를 Bean이라고 한다.
- 서블릿을 통해 웹으로 들어온 요청을 받아서 Spring 컨테이너에 다시 넘겨준다. 그럼 스프링 컨테이너가 그 작업을 Bean들이 처리할 수 있도록 해 준다.

## Spring Boot가 탄생한 배경

Q. 그럼 Spring 컨테이너가 서블릿 컨테이너를 대체하면 안 될까?  
A. 안 된다. 자바의 표준 웹 기술을 사용하려면 서블릿 컨테이너가 존재해야 한다.


- 우리는 스프링으로 애플리케이션 개발하는 데만(Bean들을 어떻게 구성하고 스프링 컨테이너에 어떻게 담을 것인지) 집중하고 싶은데, 아주 간단한 애플리케이션이라도 동작시키려면 Servlet 컨테이너를 띄워야 한다.
- 그런데 Servlet 컨테이너를 띄우는 게 간단하지 않고, 신경써야 할 게 너무 많다.(빌드, classloader, logging 등)
- 서블릿 컨테이너는 표준 스택이고, 이걸 구현한 제품을 사용해야 하는데(ex) Tomcat) 각 제품마다 구성, 배포, 설정 방법이 다 다르다.
- 그래서 진입 장벽과 학습 장벽이 높고, 학습한다 해도 그다지 유용하지 않다. 또, 프로젝트 초기에 세팅하고 나면 다시 잊어버리게 된다.

➡️ *서블릿 컨테이너 신경쓸 필요가 없었으면 좋겠다*는 아이디어에서 Containerless 애플리케이션 아키텍처(스프링 부트❗)가 생겨났다.

<img width="1001" height="471" alt="image" src="https://github.com/user-attachments/assets/814df25f-7f22-4578-946e-e7b0255ee425" />


스프링부트에서는 서블릿 컨테이너를 띄우는 작업도 직접 할 필요 없고, main 메소드를 호출하는 것만으로 서블릿 컨테이너와 관련된 모든 필요한 작업이 수행된다.

= **독립실행형(Standalone)** 애플리케이션


---

참고:
- 토비의 스프링 부트 - 이해와 원리 섹션 2, 7강
- https://docs.oracle.com/cd/E19226-01/820-7627/bnafe/index.html
- https://medium.com/@koteswararao25800/what-is-servlet-ee5216d3d3cc
