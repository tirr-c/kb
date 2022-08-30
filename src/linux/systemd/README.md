# systemd
[systemd]는 리눅스 유저스페이스에서 시스템의 기반을 제공하고 관리하는 도구 집합이다. 제일 유명한
기능이 init process일 것이다. systemd 시스템에서 htop 등으로 프로세스 목록을 찍어 보면 맨 위에 위엄
넘치는 `systemd`의 모습을 확인할 수 있다.

\[여기에 스크린샷 입력\]

systemd가 하는 일은 다양한데...
- 시스템 서비스의 실행과 종속성 관리 ([`systemctl`][systemctl.1])
- 시스템 로그 관리 ([`journalctl`][journalctl.1])
- cron 대체 ([`systemd.timer`][systemd.timer.5])
- 네트워크 설정 ([`networkctl`][networkctl.1])
- DNS 설정 및 리졸버 ([`resolvectl`][resolvectl.1])
- NTP 데몬 ([`timedatectl`][timedatectl.1])
- 파일시스템 마운트 관리 (`/etc/fstab`, [`systemd.mount`][systemd.mount.5])
- 부트로더 ([`bootctl`][bootctl.1], [`systemd-stub`][systemd-stub.7]. 여기에 관해서는
  [통합형 커널 이미지](../efi-stub.md)에서 잠깐 언급했다.)
- 디스크 암호화 토큰 관리 ([`systemd-cryptenroll`][systemd-cryptenroll.1])
- 등등...

기능이 참 많죠? 아무튼 리눅스 시스템의 만능 도구 같은 녀석이다. 이런 all-in-one 스타일 때문에 유닉스
철학 좋아하는 사람들은 systemd를 별로 안 좋아하지만, 실제로 이걸 쓰면 시스템 관리가 훨씬 편한데 굳이
그런 걸 신경써야 할까? 시스템 관리자, 사람이 편해지는 게 더 중요하다.

[systemd]: https://systemd.io/
[systemctl.1]: https://www.freedesktop.org/software/systemd/man/systemctl.html
[journalctl.1]: https://www.freedesktop.org/software/systemd/man/journalctl.html
[systemd.timer.5]: https://www.freedesktop.org/software/systemd/man/systemd.timer.html
[networkctl.1]: https://www.freedesktop.org/software/systemd/man/networkctl.html
[resolvectl.1]: https://www.freedesktop.org/software/systemd/man/resolvectl.html
[timedatectl.1]: https://www.freedesktop.org/software/systemd/man/timedatectl.html
[systemd.mount.5]: https://www.freedesktop.org/software/systemd/man/systemd.mount.html
[bootctl.1]: https://www.freedesktop.org/software/systemd/man/bootctl.html
[systemd-stub.7]: https://www.freedesktop.org/software/systemd/man/systemd-stub.html
[systemd-cryptenroll.1]: https://www.freedesktop.org/software/systemd/man/systemd-cryptenroll.html

---

아무튼 systemd는 할 수 있는 게 많은 만큼 기본적인 부분 외에도 깊게 들어가면 끝없이 배울 수 있다.
아래 목록은 내가 몇 년간 이 시스템을 써 오면서 배운 것들을 글로 정리해 둔 것들이다.

- [소켓 기반 서비스 활성화](./socket-activation.md) (socket-based activation of service units)
