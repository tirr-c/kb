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

## 소켓
여러분! 소켓이 무엇입니까? 언제나 그리운...지는 잘 모르겠고, 대충 설명하자면 무언가와 정보를
주고받기 위한 창구 같은 것이다. 소켓의 종류에는 여러 가지가 있지만 설명하기 시작하면 너무 길어지니까
자주 쓰는 소켓 유형을 몇 개... 두 개 정도 살펴보자.

### `AF_INET`
이러한 종류의 소켓은 IP(인터넷 프로토콜)를 사용한다. 이 소켓은 하나 이상의 네트워크 장치에 연결되어
IP 위에서 구현된 TCP 또는 UDP를 사용해 다른 소켓과 통신할 수 있다.

### `AF_UNIX` (`AF_LOCAL`)
이러한 종류의 소켓은 운영체제(여기서는 리눅스)의 도움을 받아 같은 머신 안의 다른 소켓과 통신할 때
쓰인다. 일반적으로 파일시스템 위에 소켓 유형의 파일으로 존재하나, 파일시스템에 등록되지 않고 이름만
가질 수도 있으며, 이름 없이 파일 디스크립터를 통해서만 존재하는 경우도 있다.

(작성 중)

## 소켓 기반 활성화
systemd는 리스닝 소켓을 만들어 그 소켓에 접속 요청이 들어오면 특정 서비스를 실행시킨 후 그 서비스에
리스닝 소켓을 넘기는 기능을 가지고 있다. 이것을 **소켓 기반 활성화**<sub>socket-based
activation</sub>라고 한다. 이걸 어디에 쓰느냐? 어떤 머신 안에서 유저나 다른 프로그램에게 서비스를
제공하는 프로그램... 알기 쉬운 예로 도커를 들어 볼까요. 도커 데몬은 리스닝 소켓을 열어놓고, 그
소켓으로 접속한 프로그램들의 요청을 받아 처리한다. 한번 파일시스템에 살고 있는 작은 소켓을 만나보자.
```
[여기에 ls 입력]
```

저 파일에 쓰기 권한을 갖고 있다면 소켓에 접속해 도커 데몬에게 명령을 내릴 수 있다. 지금은 `root`만
가능하겠네요.

일반적으로는 도커 데몬이 소켓을 열지만, 이걸 systemd가 대신 열 수 있고, 따라서 도커 데몬을 필요할
때만<sub>on-demand</sub> 실행할 수 있다.

(작성 중)

## 소켓을 서비스로 넘기기
(작성 필요)
