# 🌐Process Management 1

## 🌐프로세스의 생성

- 부모 프로세스가 자식 프로세스를 생성한다.
- 부모 프로세스는 여러 자식 프로세스를 생성할 수 있기 때문에 프로세스의 **트리**를 형성한다.
- 프로세스는 자원이 필요한데, 아래 두 가지 방식으로 자원을 사용한다.
  - 운영체제로부터 자원을 받는다.
  - 부모와 자원을 공유한다.
- 자원의 공유 모델
  - 부모와 자식이 모든 자원을 공유하는 모델.
  - 자원의 일부를 공유하는 모델.
  - 전혀 공유하지 않는 모델.
  - 보통 자원을 공유하지 않고 자원을 두고 경쟁한다.
- 수행 모델
  - 부모와 자식이 공존하며 수행되는 모델.
  - 자식이 종료될 때 까지 부모가 기다리는 모델.

### 주소 공간

- 자식은 부모의 공간을 복사한다.(binary and OS data)
  - 운영체제의 PCB같은 자원들도 복사
- 복제하더라도 자식은 새로운 프로그램을 덮어 씌울 수 있다.

### 유닉스의 예

- fork() 시스템 콜이 새로운 프로세스를 생성한다.
  - 프로세스 생성 시 운영체제에게 생성을 요청하기 때문에 시스템 콜이다.
  - 부모를 그대로 복사한다.
  - 주소 공간을 할당한다.
- fork() 다음에 이어지는 exec() 시스템 콜을 사용해서 새로운 프로그램을 덮어 씌울 수 있다(새로운 프로그램을 메모리에 올릴 수 있다.).
- 덮어씌우지 않을 수도 있다. 

## 🌐프로세스의 종료

- 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려준다.(exit, 예) C언어에서 중괄호가 닫히는 것)
  - 자식이 부모에게 output 데이터를 보낸다.
  - 프로세스의 각종 자원들이 운영체제에게 반납된다.
  - 자발적인 종료이다.
  - 프로세스 종료 시 자식 프로세스가 먼저 종료된다.
- 부모 프로세스가 자식의 수행을 종료시킨다.(abort)
  - 자식이 할당 자원의 한계치를 넘어선 경우
  - 자식에게 할당된 일(task)가 더 이상 필요하지 않은 경우
  - 부모가 종료(하는 걍우
    - 운영체제는 부모 프로세스가 종료되는 경우 자식이 더 이상 수행되도록 두지 않고 종료시킨다.
    - 단계적인 종료
    - 비자발적인 종료이다.
