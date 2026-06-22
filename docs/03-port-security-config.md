# 03. Port-Security 설정 실습

## 정보 확인 항목

- **MaxSecureAddr** : 최대 허용 MAC Address 개수
- **CurrentAddr** : 현재 Secure MAC으로 등록된 개수
- **violation** : 위반 발생 횟수
- **action** : 위반 시 동작 (예: restrict)

## Secure (정적 MAC 지정) 설정

```text
en
conf t
int range fa0/1-20
 spanning-tree portfast
int fa0/20
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 0001.9779.02AE
 switchport port-security violation shutdown
 switchport port-security
int fa0/1
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00D0.BA58.EE9A
 switchport port-security violation shutdown
 switchport port-security
int fa0/2
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00D0.BACC.B6AD
 switchport port-security violation shutdown
 switchport port-security
int fa0/3
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00E0.F9DD.51B1
 switchport port-security violation shutdown
 switchport port-security
end
wr
```

## 정보 확인

```text
Switch#show port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
              (Count)        (Count)      (Count)
--------------------------------------------------------------------
Fa0/1        1              1            0                  Shutdown
Fa0/2        1              1            0                  Shutdown
Fa0/3        1              1            0                  Shutdown
Fa0/20       1              1            0                  Shutdown
----------------------------------------------------------------------
```

```text
Switch#show port-security interface fa0/1
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 1
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0
```

## fa0/3 포트 차단 (위반 발생 시)

```text
Switch#sh port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
              (Count)        (Count)      (Count)
--------------------------------------------------------------------
Fa0/1        1              1            0                  Shutdown
Fa0/2        1              1            0                  Shutdown
Fa0/3        1              1            1                  Shutdown
Fa0/20       1              1            0                  Shutdown
----------------------------------------------------------------------
```

```text
Switch#show port-security interface fa0/3
Port Security              : Enabled
Port Status                : Secure-shutdown
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 1
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 000D.BDAA.2468:1
Security Violation Count   : 1
```

## 복구 (shutdown → no shutdown)

```text
Switch#conf t
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#int fa0/3
Switch(config-if)#sh
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down
Switch(config-if)#no sh
Switch(config-if)#
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to up
```

## protect 모드 설정 예시

```text
en
conf t
int range fa0/1-20
 spanning-tree portfast
int fa0/20
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 0001.9779.02AE
 switchport port-security violation protect
 switchport port-security
int fa0/1
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00D0.BA58.EE9A
 switchport port-security violation protect
 switchport port-security
int fa0/2
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00D0.BACC.B6AD
 switchport port-security violation protect
 switchport port-security
int fa0/3
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00E0.F9DD.51B1
 switchport port-security violation protect
 switchport port-security
end
wr
```

> 📌 원본 문서의 `switchport port-protect`는 오타이며, 올바른 명령은
> `switchport port-security violation protect` + `switchport port-security` 입니다.

## restrict 모드 설정 예시 (SW1)

```text
en
conf t
interface fastethernet 0/1
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00D0.BA58.EE9A
 switchport port-security violation restrict
 switchport port-security
interface fastethernet 0/2
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00D0.BACC.B6AD
 switchport port-security violation restrict
 switchport port-security
interface fastethernet 0/3
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 00E0.F9DD.51B1
 switchport port-security violation restrict
 switchport port-security
interface fastethernet 0/20
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 0001.9779.02AE
 switchport port-security violation restrict
 switchport port-security
end
wr
```
