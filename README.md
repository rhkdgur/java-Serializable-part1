# java-Serializable-part1
## 자바 직렬화의 의미와 필요성 part1

> 데이터 직렬화
>> 메모리를 디스크에 저장하거나 네트워크 통신에 사용하기 위한 형식으로 변환하는 것을 의미
>> (즉, Java 내부 시스템에서 사용되는 객체나 데이터를 외부에서 사용할 수 있도록 Byte 형태로 변환하는 것)

> 역직렬화 
>> 직렬화와 반대로 디스크에 저장한 데이터를 읽거나, 네트워크 통신으로 받은 데이터를 메모리에 쓸 수 있도록 다시 변환하는 것

## 직렬화는 왜 필요한가?<br>
설명 : 개발 언어로 무엇을 사용하던 사용하는 데이터들의 메모리 구조는 크게 2가지로 나뉜다.
> 값 형식 데이터(Value Type)
>> 우리가 흔히 선언해서 사용하는 int,float,char 등 , 값 형식 데이터들은 스택에 메모리가 쌓이고 직접 접근이 가능

> 참조 형식 데이터(Reference Type)
>> C#에서 Object 타입 혹은 C++에서 포인트 변수들이 여기에 해당한다. 해당 형식의 변수를 선언하면 힙 메모리가 할당 되고 스택에서는 이 힙 메모리를 참조하는(힙에 메모리 번지 주소를 가지고 있음)
>> 구조로 되어 있다.

*이 두가지 데이터 중에서 디스크에 저장하거나 통신에는 **값 형식 데이터**만 가능하다.*<br>

참조 형식 데이터(Reference Type)는 실제 데이터 값이 아닌 힙에 할당되어있는 메모리 번지 주소를 가지고 있기 때문에 저장,통신에 사용할 수 없다.<br>

## 참조 형식 데이터는 사용할 수 없는 이유<br>
>참조 형식의 데이터는 Class A를 선언하고 객체를 만들었을 때 A는 주소값을 가지게 된다.
>그리고 이 값을 파일로 포함하여 저장할 경우 주소값을 저장하게 되는 것이다.
>만약에 프로그램을 종료 후 다시 실행하게 된다면 주소 값은 남아 있지만 실제 데이터는 사라지기에 가져올 수가 없다.
>>네트워크 통신 또한 그러하다
>>각 pc 마다 사용하고 있는 메모리 공간 주소는 다르다. 그렇기 때문에 다른 pc로 전송한 A 객체 주소 값은 무의미하다.
>>왜냐하면 받은 pc에서의 메모리 주소는 A 객체 주소와 전혀 다른 값으로 사용 되기 때문이다.

*그렇기에 직렬화를 쓰는 이유는 사용하고 있는 데이터들을 파일 저장 혹은 데이터 통신에서 파싱 할 수 있는 유의미한 데이터로 만들기 위함이다.*

## 직렬화를 주로 어디에 사용하는 것인가?
> JVM의 메모리에만 상주되어 있는 객체 데이터를 그대로 **영속화(Persistence)** 가 필요할때 사용 된다.
> 시스템이 종료되더라도 없어지지 않는 장점을 가지며 영속화된 데이터이기 때문에 네트워크 전송도 가능하다.
> 필요할 때 직렬화된 객체 데이터를 가져와서 역질려화하여 객체를 바로 사용할 수 있게 된다.
* Servlet Session
  - Servlet 기반 세션은 java 직렬화를 지원하고 있다. 단순히 세션을 서블릿 메모리 위에서 운용한다면 직렬화가 필요 없지만, 파일로 저장하거나 세션 클러스터링, DB를 저장하는 옵션등을
  - 선택하게 되면 세션 자체가 직렬화되어 저장,전달 된다.

* Cache
  - 캐시할 부분을 직렬화된 데이터를 저장해서 사용한다. 주로 DB를 조회한 후 가져온 데이터 객체 같은 경우 실시간 형태로 요구하는 데이터가 아니라면, 메모리, 외부 저장소, 파일 등을 저장소를 이용해서
  - 데이터 객체를 저장한 후 동일한 요청이 오면 DB를 다시 요청하는 것이 아니라 저장된 객체를 찾아서 응답하게 하는 형태를 캐시를 사용한다 라고 표현한다.
  - 캐시를 이용하면 DB리소스를 절약할 수 있기 때문에 많은 시스템에서 자주 활용된다.

* Java RMI(Remote Method Invocation)
  - 원격 시스템의 메서드를 호출 할때 전달하는 객체를 직렬화 하여 사용
  - 메세지 (객체)를 전달 받은 원격 시스템에서는 객체를 역질렬화 하여 사용

