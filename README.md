# 🔒 Cisco Switch Port-Security 실습 정리

![Cisco](https://img.shields.io/badge/Cisco-1BA0D7?style=flat&logo=cisco&logoColor=white)
![Packet Tracer](https://img.shields.io/badge/Packet%20Tracer-005073?style=flat&logo=cisco&logoColor=white)
![Network Security](https://img.shields.io/badge/Network-Security-green?style=flat)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![GitHub last commit](https://img.shields.io/github/last-commit/KSNAM97/cisco-port-security)
![GitHub repo stars](https://img.shields.io/github/stars/KSNAM97/cisco-port-security?style=social)

Cisco 스위치의 **Port-Security** 기능을 학습하고 실습한 내용을 정리한 저장소입니다.
MAC 주소 기반 포트 접근 제어, Violation 모드 비교, Sticky 자동 학습 기능까지 다룹니다.

> 💡 저장소 이름을 `cisco-port-security`가 아닌 다른 이름으로 만들면, 위 뱃지 URL의 `cisco-port-security` 부분도 똑같이 바꿔주세요.

## 📚 목차

| 챕터 | 내용 |
|------|------|
| [01. Port-Security 기본 개념](docs/01-port-security-basic.md) | Port-Security 동작 원리 |
| [02. Violation 모드 비교](docs/02-violation-modes.md) | protect / restrict / shutdown |
| [03. Port-Security 설정 실습](docs/03-port-security-config.md) | 설정 명령어 및 정보 확인 |
| [04. Port-Security Sticky](docs/04-port-security-sticky.md) | MAC 자동 학습 기능 |

## 🎯 학습 목표

- 스위치 포트에 연결 가능한 장비를 MAC 주소 기준으로 제한하는 방법 이해
- 위반(Violation) 발생 시 동작 모드(protect / restrict / shutdown)의 차이점 파악
- Sticky 기능을 통한 Secure MAC 주소 자동 학습 활용

## 🛠️ 실습 환경

- Cisco Packet Tracer / 실 장비
- Cisco IOS Switch (Catalyst 계열)

## 📄 License

This project is licensed under the MIT License.

## 🙋 Author

- GitHub: [@KSNAM97](https://github.com/KSNAM97)
