# 04. Port-Security Sticky

## 개념

- **Port-Security Sticky**는 Switch가 특정 Port로 처음 입력되는 장비의 Source MAC Address를 **자동으로 학습**하여 Secure MAC Address로 등록하는 기능이다.
- 일반 Port-Security에서는 관리자가 허용할 MAC Address를 직접 입력해야 한다. 하지만 Port가 많고 PC가 여러 대인 환경에서는 모든 MAC Address를 직접 확인해서 입력하는 것이 번거롭다. 이때 사용하는 기능이 **Sticky**이다.
- Sticky가 설정된 Port로 Frame이 입력되면 Switch는 Source MAC Address를 확인하고, 아직 Secure MAC Address가 등록되어 있지 않다면 **처음 입력된 Source MAC Address를 자동으로 Secure MAC Address로 등록**한다.

## 설정

```text
en
conf t
int range fa0/1-20
 spanning-tree portfast
 spanning-tree bpduguard enable
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation shutdown
 switchport port-security
end
wr
```

## 정보 확인 — show port-security

```text
Switch#sh port-security
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
              (Count)        (Count)      (Count)
--------------------------------------------------------------------
Fa0/1        1              0            0                  Shutdown
Fa0/2        1              0            0                  Shutdown
Fa0/3        1              1            0                  Shutdown
Fa0/4        1              0            0                  Shutdown
...
Fa0/19       1              0            0                  Shutdown
Fa0/20       1              1            0                  Shutdown
----------------------------------------------------------------------
```

## 정보 확인 — show port-security address

```text
Switch#sh port-security address
        Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan   Mac Address       Type           Ports     Remaining Age (mins)
----   -----------       ----           -----     --------------------
1      00D0.BA58.EE9A    SecureSticky   Fa0/1     -
1      00D0.BACC.B6AD    SecureSticky   Fa0/2     -
1      00E0.F9DD.51B1    SecureSticky   Fa0/3     -
1      0001.9779.02AE    SecureSticky   Fa0/20    -
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port) : 0
Max Addresses limit in System (excluding one mac per port) : 1024
```

## maximum 값 변경 실습

이미 지정한 `switchport port-security maximum` 값을 변경하는 예시이다.

```text
en
conf t
int fa0/3
 switchport port-security maximum 2
end
wr
```

### 변경 후 확인

```text
Switch#sh port-security address
        Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan   Mac Address       Type           Ports     Remaining Age (mins)
----   -----------       ----           -----     --------------------
1      00D0.BA58.EE9A    SecureSticky   Fa0/1     -
1      00D0.BACC.B6AD    SecureSticky   Fa0/2     -
1      000D.BDAA.2468    SecureSticky   Fa0/3     -
1      00E0.F9DD.51B1    SecureSticky   Fa0/3     -
1      0001.9779.02AE    SecureSticky   Fa0/20    -
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port) : 1
Max Addresses limit in System (excluding one mac per port) : 1024
```

> 📌 maximum 값을 2로 늘리자 Fa0/3 포트에 두 개의 MAC(`000D.BDAA.2468`, `00E0.F9DD.51B1`)이 모두 등록된 것을 확인할 수 있다.
