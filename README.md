# bluetooth-6.17

English | [한국어](#한국어)

## English

### Overview

This repository contains an out-of-tree Bluetooth driver tree adapted for
Ubuntu 24.04 HWE kernels, specifically tested with `6.17.0-20-generic`.

The current port updates the code for newer kernel Bluetooth APIs, including:

- `hdev->quirks` to `hdev->quirk_flags`
- `asm/unaligned.h` to `linux/unaligned.h`
- removal of legacy `cmd_timeout` assignments that no longer exist in
  `struct hci_dev` on the target kernel

### Tested Environment

- OS: Ubuntu 24.04
- Kernel: `6.17.0-20-generic`
- DKMS package name: `btusb`
- DKMS package version: `4.2`

### Modules

The local `Makefile` builds:

- `btusb`
- `ath3k`
- `btrtl`

The current `dkms.conf` installs:

- `btusb`
- `ath3k`

### Build

```bash
make
```

To build for a specific kernel:

```bash
make KVER=6.17.0-20-generic
```

### DKMS Install

Copy or sync this source tree to `/usr/src/btusb-4.2`, then run:

```bash
sudo dkms build -m btusb -v 4.2 -k 6.17.0-20-generic --force
sudo dkms install -m btusb -v 4.2 -k 6.17.0-20-generic --force
```

To reload the modules immediately:

```bash
sudo modprobe -r btusb ath3k
sudo modprobe btusb
```

### Verification

Check that the DKMS-installed modules are active:

```bash
modinfo -n btusb
modinfo -n ath3k
dkms status
```

Expected module paths should point to:

```text
/lib/modules/<kernel>/updates/dkms/
```

### Notes

- This repository is intended for kernel compatibility work on Ubuntu 24.04
  HWE `6.17`.
- Runtime Bluetooth functionality should still be validated on real hardware
  after installation.

### Acknowledgements

This repository is based on the earlier `bluetooth-6.8` work published by
[jeremyb31](https://github.com/jeremyb31). Credit goes to JeremyB31 for the
original out-of-tree packaging and compatibility work that this port builds on.

---

## 한국어

### 개요

이 저장소는 Ubuntu 24.04 HWE 커널용으로 맞춘 외부 Bluetooth 드라이버 트리이며,
현재 `6.17.0-20-generic` 커널에서 확인했습니다.

현재 포팅 내용은 최신 커널 Bluetooth API 변경에 맞춘 것입니다.

- `hdev->quirks`를 `hdev->quirk_flags`로 변경
- `asm/unaligned.h`를 `linux/unaligned.h`로 변경
- 대상 커널의 `struct hci_dev`에서 제거된 legacy `cmd_timeout`
  할당 제거

### 테스트 환경

- OS: Ubuntu 24.04
- 커널: `6.17.0-20-generic`
- DKMS 패키지 이름: `btusb`
- DKMS 패키지 버전: `4.2`

### 모듈 구성

현재 로컬 `Makefile`이 빌드하는 모듈:

- `btusb`
- `ath3k`
- `btrtl`

현재 `dkms.conf`가 설치하는 모듈:

- `btusb`
- `ath3k`

### 빌드

```bash
make
```

특정 커널용으로 빌드하려면:

```bash
make KVER=6.17.0-20-generic
```

### DKMS 설치

이 소스 트리를 `/usr/src/btusb-4.2`로 복사하거나 동기화한 뒤 아래를 실행합니다.

```bash
sudo dkms build -m btusb -v 4.2 -k 6.17.0-20-generic --force
sudo dkms install -m btusb -v 4.2 -k 6.17.0-20-generic --force
```

바로 모듈을 다시 로드하려면:

```bash
sudo modprobe -r btusb ath3k
sudo modprobe btusb
```

### 검증

DKMS로 설치된 모듈이 실제로 사용 중인지 확인:

```bash
modinfo -n btusb
modinfo -n ath3k
dkms status
```

예상 경로:

```text
/lib/modules/<kernel>/updates/dkms/
```

### 참고

- 이 저장소는 Ubuntu 24.04 HWE `6.17` 커널 호환 작업을 위한 것입니다.
- 설치 후 실제 Bluetooth 동작은 장비에서 별도로 확인하는 것이 좋습니다.

### 감사의 말

이 저장소는 [jeremyb31](https://github.com/jeremyb31)이 공개한 기존
`bluetooth-6.8` 작업을 바탕으로 합니다. 원래의 외부 모듈 패키징과 호환성
작업을 제공한 JeremyB31에게 감사를 표합니다.
