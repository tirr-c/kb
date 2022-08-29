# 커널 EFI 껍데기와 통합형 커널 이미지

## 여러분, 리눅스 부팅은 어떻게 하고 계십니까?
UEFI 펌웨어의 시대가 온 지도 꽤 됐지만, 부팅할 때 리눅스를 메모리에 올리는 방법으로는 여전히 GRUB
같은 부트로더를 사용하는 방식이 가장 널리 사용되고 있을 것이다. BIOS 펌웨어를 사용하던 시절부터 계속
사용된 유서 깊은 방식이죠. GRUB을 사용하면 BIOS 펌웨어와 UEFI 펌웨어의 차이를 따로 기억하지 않아도
되기 때문에 편하기도 하다(GRUB이 다 알아서 해 주니까). 요즘은 `systemd-boot` 같은 UEFI 펌웨어용
부트로더도 선택지가 꽤 있기 때문에 GRUB을 쓰지 않는 시스템도 많겠지만, 그래도 부트로더를 쓰는 경우가
많다는 것은 확실하다.

하지만... UEFI 환경에서는 **부트로더를 따로 쓸 필요가 없다**는 사실을 아십니까?

## 커널 EFI 껍데기
놀랍게도 요즘 리눅스 커널은 **스스로 메모리에 올라가서 실행될 수 있다!** 아는 분들은 아시겠지만
리눅스 커널은 (압축된) ELF 파일이죠. 펌웨어는 ELF 파일을 메모리에 올려 실행하는 방법을 모르기 때문에
별도의 부트로더를 사용해 메모리에 올려야 한다는 것이 상식이었다. 그런데 어떻게 커널이 스스로
메모리에 올라간다는 말인가?

해답은 리눅스 커널을 **EFI 껍데기**<sub>stub</sub>로 감싸는 것이다. 이 껍데기 코드는 속에 들어있는
커널과 함께 펌웨어에 의해 메모리에 올라간 후, UEFI 실행 환경 속에서 리눅스 커널을 실행할 준비를 한
뒤 제어를 (함께 메모리에 올라와 있던) 리눅스 커널에게 넘겨준다. UEFI 펌웨어는 껍데기만 보고 실행
파일을 메모리에 올려서 실행했는데, 실제로 실행된 건 껍데기 속 리눅스 커널인 셈이다. 이게 뭔 트로이
목마도 아니고... 하지만 리눅스 유저 입장에서는 착한 트로이 목마죠.

어찌 보면 EFI 껍데기 코드가 부트로더 역할을 한다고 볼 수도 있다. 커널이 압축돼 있다면 압축 해제도 해
주고, 커널 주소공간 뒤섞기<sub>KASLR</sub>도 해 주고, 커널 명령 인자에 `initrd=`가 지정돼 있다면
그것도 같이 메모리에 올려 준다. 커널이 제어를 넘겨받은 뒤에는 이렇게 껍데기 코드가 올려 준 데이터가
주변에 다 있고 커널은 그걸 먹기만 하면 된다.

그럼 리눅스 커널을 EFI 껍데기로 감싸는 건 어떻게 할까? 커널을 빌드할 때 `CONFIG_EFI_STUB`을 `y`로
설정해 주면 알아서 된다. 참 쉽죠? 요즘 배포판들은 다 이렇게 빌드해서 배포해줄 것이다.

## 통합형 커널 이미지
앞에서 커널이 EFI 껍데기를 만들 수 있다고 설명했지만, 그것이 유일한 껍데기인 것은 아니다! 대표적으로
[systemd가 EFI 껍데기를 제공한다][systemd-stub.7]. 이 껍데기는 커널이 제공하는 것과는 달리 지원하는
기능이 더 많다.
- 부팅에 사용할 initrd를 포함시킬 수 있다. 여기에는 CPU 마이크로코드도 포함된다.
- 부팅할 때 기본으로 사용할 커널 명령 인자를 지정할 수 있다.
- 부팅 중에 화면에 표시할 스플래시 이미지를 포함시킬 수 있다.
- 부팅할 배포판에 관한 설명([`os-release`][os-release.5])을 포함시킬 수 있다.

그러니까 이거 하나만 있으면 다른 파일을 참조할 필요 없이 부팅이 되는 것이다. 이렇게 부팅에 필요한
것들을 하나의 EFI 실행 파일으로 묶은 것을 **통합형 커널 이미지**<sub>unified kernel image</sub>라고
한다. Systemd에서 지정한 [Boot Loader Specification]을 지키는 부트로더는 규칙에 맞는 커널 이미지를
발견한 경우 이미지에 포함된 `os-release` 정보를 사용해 부트 엔트리를 생성하게 된다.

통합형 커널 이미지는 initramfs를 만들면서 함께 만드는 경우가 많다. [`dracut`][dracut.8]이 `--uefi`
옵션을 통해 커널 이미지를 만드는 것을 지원하고, 아치 리눅스에서는 [`mkinitcpio`][mkinitcpio.8]를
사용해 만들 수도 있다.

[systemd-stub.7]: https://www.freedesktop.org/software/systemd/man/systemd-stub.html
[os-release.5]: https://www.freedesktop.org/software/systemd/man/os-release.html
[Boot Loader Specification]: https://systemd.io/BOOT_LOADER_SPECIFICATION/
[dracut.8]: https://man.archlinux.org/man/dracut.8
[mkinitcpio.8]: https://man.archlinux.org/man/mkinitcpio.8

### 참고 자료
- 본문에 걸린 링크들
- [`admin-guide/efi-stub.rst`][efi-stub] (커널 문서)
- [Unified kernel image][unified-kernel-image] (ArchWiki)

[efi-stub]: https://docs.kernel.org/admin-guide/efi-stub.html
[unified-kernel-image]: https://wiki.archlinux.org/title/Unified_kernel_image
