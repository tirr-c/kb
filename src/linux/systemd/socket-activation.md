# 소켓 기반 서비스 활성화
systemd는 리소스를 [**유닛**][systemd.unit.5]<sub>unit</sub>이라는 단위로 관리한다. 어... 말이
이상하네. Unit이 단위라는 뜻 아닌가? 아무튼 그렇다. 가장 많이 접하는 유닛은 아마
[서비스][systemd.service.5]일 것이다. 그 외에도 [타이머][systemd.timer.5],
[마운트 지점][systemd.mount.5], [스왑 파일][systemd.swap.5] 등등 다양한 유닛들이 있다. 이것들은
나중에 천천히 다뤄볼 기회가 있을 것이다...

이 문서에서는, 그런 유닛들 중에서도 [소켓 유닛][systemd.socket.5]을 살펴볼 것이다.

[systemd.unit.5]: https://www.freedesktop.org/software/systemd/man/systemd.unit.html
[systemd.service.5]: https://www.freedesktop.org/software/systemd/man/systemd.service.html
[systemd.timer.5]: https://www.freedesktop.org/software/systemd/man/systemd.timer.html
[systemd.mount.5]: https://www.freedesktop.org/software/systemd/man/systemd.mount.html
[systemd.swap.5]: https://www.freedesktop.org/software/systemd/man/systemd.swap.html
[systemd.socket.5]: https://www.freedesktop.org/software/systemd/man/systemd.socket.html

## 유닉스 소켓
여러분! 소켓이 무엇입니까? 언제나 그리운...지는 잘 모르겠고, 대충 설명하자면 무언가와 정보를
주고받기 위한 창구 같은 것이다. 소켓의 종류에는 여러 가지가 있지만 설명하기 시작하면 너무 길어지니까
여기서는 생략하기로 하고, 많디많은 소켓들 중에 **이름이 있는**, **리스닝**, **유닉스** 소켓에
관해서만 살펴보자.

용어가 뭔가 있어 보인다. 근데 그게 뭐예요?
- **유닉스** 소켓: 정확히 표현하면 **UNIX 영역의 소켓**<sub>UNIX domain socket</sub>이다. 이러한
  종류의 소켓은 머신 내부에서 접근할 수 있게 파일으로 표현된다. 와! 파일이래. 완전 유닉스스럽네요.
- **이름이 있는**<sub>named</sub>: 파일에 이름이 있다는 말은 리눅스 파일시스템 상에서 경로로 접근이
  가능하다는 말이다.
- **리스닝**<sub>listening</sub>: (작성 필요)

(작성 중)

## 소켓 기반 활성화
(작성 필요)

## 소켓을 서비스로 넘기기
(작성 필요)
