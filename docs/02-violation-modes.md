# 02. Violation 모드 비교 (protect / restrict / shutdown)

## 모드별 동작 비교표

| 구분 | protect | restrict | shutdown (기본값) |
|------|---------|----------|-------------------|
| 위반 프레임 처리 | drop (차단) | drop (차단) | drop (차단) |
| 포트 상태 | up 유지 | up 유지 | err-disabled로 셧다운 |
| Syslog 메시지 | 없음 | 발생 | 발생 |
| SNMP trap | 없음 | 발생 | 발생 |
| 위반 카운터 | 증가 안 함 | 증가 | 증가 |
| 관리자 복구 | 불필요 | 불필요 | 필요 |

## 모드별 특징

- **protect**: 위반 트래픽을 조용히 버리기만 하고 기록/알림이 없어 추적이 불가능하다. 실무에서는 잘 권장되지 않음.
- **restrict**: 위반 트래픽을 버리면서 알림과 카운팅을 수행한다. 정상 트래픽은 계속 통과.
- **shutdown**: 가장 강력. 위반 시 포트 전체를 막아버린다. 복구하려면 `shutdown` 후 `no shutdown`, 또는 `errdisable recovery` 설정 필요.

## 한눈 요약

- **차단 강도**: `shutdown > restrict > protect`
- **알림 여부**: `restrict = shutdown` (있음), `protect` (없음)
- **복구 필요**: `shutdown`만 필요
- **기본값**: port-security maximum `1`, violation `shutdown`

## 명령어 흐름

```text
en
conf t
int fa0/x
switchport mode access
switchport port-security maximum ?
switchport port-security mac-address
switchport port-security violation ?
```

- `protect` : 관리자가 지정하지 않은 MAC Address 접근 시 해당 트래픽을 폐기
- `restrict` : protect 기능에 log-message 출력까지 수행
- `shutdown` : 관리자가 지정하지 않은 MAC Address 접근 시 err-disable 상태로 전환
- `switchport port-security` : 이 명령을 설정하면 port-security 기능이 활성화된다.
