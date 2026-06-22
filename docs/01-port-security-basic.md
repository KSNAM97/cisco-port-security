# 01. Port-Security 기본 개념

> 관리자는 자신이 관리하는 모든 네트워크 환경의 물리적인 구성도와 경로 등을 모두 파악해야 한다.

## 개념

- **Port-Security**는 Switch의 특정 Port에 접속할 수 있는 장비를 **MAC Address 기준으로 제한**하는 보안 기능이다.
- Switch Port로 Frame이 들어오면 Switch는 해당 Frame의 **Source MAC Address**를 확인한다.
- 관리자가 허용한 MAC Address라면 통신을 허용하고, 허용되지 않은 MAC Address가 들어오면 정책에 따라 **트래픽을 차단**하거나 **`err-disable` 상태로 전환**한다.

## 동작 흐름

```text
[Frame 수신] → [Source MAC 확인]
       ├── 허용된 MAC  → 통신 허용
       └── 미허용 MAC  → 정책에 따라 처리 (drop / err-disable)
```
