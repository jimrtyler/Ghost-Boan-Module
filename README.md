# 👻 Ghost 보안 모듈
**PowerShell 기반 Windows 및 Azure 보안 강화 도구**

> **Windows 엔드포인트 및 Azure 환경을 위한 사전 예방적 보안 강화.** Ghost는 불필요한 서비스와 프로토콜을 비활성화하여 일반적인 공격 벡터를 줄이는 데 도움이 되는 PowerShell 기반 강화 기능을 제공합니다.

## ⚠️ 중요한 면책 조항

**테스트가 필요합니다**: 항상 비프로덕션 환경에서 Ghost를 먼저 테스트하십시오. 서비스를 비활성화하면 합법적인 비즈니스 기능에 영향을 줄 수 있습니다.

**보장 없음**: Ghost가 일반적인 공격 벡터를 대상으로 하지만, 어떤 보안 도구도 모든 공격을 방지할 수는 없습니다. 이는 포괄적인 보안 전략의 한 구성 요소입니다.

**운영상 영향**: 일부 기능은 시스템 기능에 영향을 줄 수 있습니다. 배포 전에 각 설정을 신중하게 검토하십시오.

**전문적 평가**: 프로덕션 환경의 경우 설정이 조직의 요구 사항과 일치하는지 확인하기 위해 보안 전문가와 상의하십시오.

## 📊 보안 환경

랜섬웨어 피해는 **2025년 570억 달러**에 도달했으며, 연구에 따르면 많은 성공적인 공격이 기본 Windows 서비스와 잘못된 구성을 악용하고 있습니다. 일반적인 공격 벡터에는 다음이 포함됩니다:

- **랜섬웨어 사고의 90%**가 RDP 악용을 포함
- **SMBv1 취약점**이 WannaCry 및 NotPetya와 같은 공격을 가능하게 함
- **문서 매크로**는 여전히 악성코드 전달의 주요 방법으로 남아있음
- **USB 기반 공격**은 에어 갭 네트워크를 계속 표적으로 삼고 있음
- **PowerShell 남용**이 최근 몇 년간 크게 증가함

## 🛡️ Ghost 보안 기능

Ghost는 **16개의 Windows 강화 기능**과 **Azure 보안 통합**을 제공합니다:

### Windows 엔드포인트 강화

| 기능 | 목적 | 고려사항 |
|----------|---------|----------------|
| `Set-RDP` | 원격 데스크톱 액세스 관리 | 원격 관리에 영향을 줄 수 있음 |
| `Set-SMBv1` | 레거시 SMB 프로토콜 제어 | 매우 오래된 시스템에 필요 |
| `Set-AutoRun` | AutoPlay/AutoRun 제어 | 사용자 편의성에 영향을 줄 수 있음 |
| `Set-USBStorage` | USB 저장 장치 제한 | 합법적인 USB 사용에 영향을 줄 수 있음 |
| `Set-Macros` | Office 매크로 실행 제어 | 매크로 지원 문서에 영향을 줄 수 있음 |
| `Set-PSRemoting` | PowerShell 원격 연결 관리 | 원격 관리에 영향을 줄 수 있음 |
| `Set-WinRM` | Windows Remote Management 제어 | 원격 관리에 영향을 줄 수 있음 |
| `Set-LLMNR` | 이름 확인 프로토콜 관리 | 일반적으로 비활성화하기에 안전 |
| `Set-NetBIOS` | TCP/IP상 NetBIOS 제어 | 레거시 애플리케이션에 영향을 줄 수 있음 |
| `Set-AdminShares` | 관리자 공유 관리 | 원격 파일 액세스에 영향을 줄 수 있음 |
| `Set-Telemetry` | 데이터 수집 제어 | 진단 기능에 영향을 줄 수 있음 |
| `Set-GuestAccount` | 게스트 계정 관리 | 일반적으로 비활성화하기에 안전 |
| `Set-ICMP` | ping 응답 제어 | 네트워크 진단에 영향을 줄 수 있음 |
| `Set-RemoteAssistance` | 원격 지원 관리 | 헬프데스크 운영에 영향을 줄 수 있음 |
| `Set-NetworkDiscovery` | 네트워크 검색 제어 | 네트워크 브라우징에 영향을 줄 수 있음 |
| `Set-Firewall` | Windows 방화벽 관리 | 네트워크 보안에 중요 |

### Azure 클라우드 보안

| 기능 | 목적 | 요구사항 |
|----------|---------|--------------|
| `Set-AzureSecurityDefaults` | 기본 Azure AD 보안 활성화 | Microsoft Graph 권한 |
| `Set-AzureConditionalAccess` | 액세스 정책 구성 | Azure AD P1/P2 라이선스 |
| `Set-AzurePrivilegedUsers` | 권한 있는 계정 감사 | 전역 관리자 권한 |

### 기업 배포 옵션

| 방법 | 사용 사례 | 요구사항 |
|--------|----------|--------------|
| **직접 실행** | 테스트, 소규모 환경 | 로컬 관리자 권한 |
| **그룹 정책** | 도메인 환경 | 도메인 관리자, GP 관리 |
| **Microsoft Intune** | 클라우드 관리 장치 | Intune 라이선스, Graph API |

## 🚀 빠른 시작

### 보안 평가
```powershell
# Ghost 모듈 로드
IEX(Invoke-WebRequest 'https://raw.githubusercontent.com/jimrtyler/Ghost/main/Ghost.ps1')

# 현재 보안 자세 확인
Get-Ghost
```

### 기본 강화 (먼저 테스트)
```powershell
# 필수 강화 - 먼저 실험실 환경에서 테스트
Set-Ghost -SMBv1 -AutoRun -Macros

# 변경 사항 검토
Get-Ghost
```

### 기업 배포
```powershell
# 그룹 정책 배포 (도메인 환경)
Set-Ghost -SMBv1 -AutoRun -GroupPolicy

# Intune 배포 (클라우드 관리 장치)
Set-Ghost -SMBv1 -RDP -USBStorage -Intune
```

## 📋 설치 방법

### 옵션 1: 직접 다운로드 (테스트)
```powershell
IEX(Invoke-WebRequest 'https://raw.githubusercontent.com/jimrtyler/Ghost/main/Ghost.ps1')
```

### 옵션 2: 모듈 설치
```powershell
# PowerShell Gallery에서 설치 (사용 가능한 경우)
Install-Module Ghost -Scope CurrentUser
Import-Module Ghost
```

### 옵션 3: 기업 배포
```powershell
# 그룹 정책 배포를 위한 네트워크 위치로 복사
# 클라우드 배포를 위한 Intune PowerShell 스크립트 구성
```

## 💼 사용 사례 예제

### 중소기업
```powershell
# 최소한의 영향으로 기본 보호
Set-Ghost -SMBv1 -AutoRun -Macros -ICMP
```

### 의료 환경
```powershell
# HIPAA 중심 강화
Set-Ghost -SMBv1 -RDP -USBStorage -AdminShares -Telemetry
```

### 금융 서비스
```powershell
# 고보안 구성
Set-Ghost -RDP -SMBv1 -AutoRun -USBStorage -Macros -PSRemoting -AdminShares
```

### 클라우드 우선 조직
```powershell
# Intune 관리 배포
Connect-IntuneGhost -Interactive
Set-Ghost -SMBv1 -RDP -AutoRun -Macros -Intune
```

## 🔍 기능 세부 정보

### 핵심 강화 기능

#### 네트워크 서비스
- **RDP**: 원격 데스크톱 액세스를 차단하거나 포트를 무작위화
- **SMBv1**: 레거시 파일 공유 프로토콜 비활성화
- **ICMP**: 정찰용 ping 응답 방지
- **LLMNR/NetBIOS**: 레거시 이름 확인 프로토콜 차단

#### 애플리케이션 보안  
- **매크로**: Office 애플리케이션에서 매크로 실행 비활성화
- **AutoRun**: 이동식 미디어의 자동 실행 방지

#### 원격 관리
- **PSRemoting**: PowerShell 원격 세션 비활성화
- **WinRM**: Windows Remote Management 중지
- **Remote Assistance**: 원격 지원 연결 차단

#### 액세스 제어
- **관리자 공유**: C$, ADMIN$ 공유 비활성화
- **게스트 계정**: 게스트 계정 액세스 비활성화
- **USB 저장소**: USB 장치 사용 제한

### Azure 통합
```powershell
# Azure 테넌트에 연결
Connect-AzureGhost -Interactive

# 보안 기본값 활성화
Set-AzureSecurityDefaults -Enable

# 조건부 액세스 구성
Set-AzureConditionalAccess -BlockLegacyAuth -RequireMFA

# 권한 있는 사용자 감사
Set-AzurePrivilegedUsers -AuditOnly
```

### Intune 통합 (v2의 새로운 기능)
```powershell
# Intune에 연결
Connect-IntuneGhost -Interactive

# Intune 정책을 통해 배포
Set-IntuneGhost -Settings @{
    RDP = $true
    SMBv1 = $true
    USBStorage = $true
    Macros = $true
}
```

## ⚠️ 중요한 고려사항

### 테스트 요구사항
- **실험실 환경**: 모든 설정을 격리된 환경에서 먼저 테스트
- **단계별 배포**: 문제를 식별하기 위해 점진적으로 롤아웃
- **롤백 계획**: 필요한 경우 변경 사항을 되돌릴 수 있는지 확인
- **문서화**: 귀하의 환경에서 작동하는 설정을 기록

### 잠재적 영향
- **사용자 생산성**: 일부 설정이 일상적인 워크플로우에 영향을 줄 수 있음
- **레거시 애플리케이션**: 이전 시스템에는 특정 프로토콜이 필요할 수 있음
- **원격 액세스**: 합법적인 원격 관리에 미치는 영향 고려
- **비즈니스 프로세스**: 설정이 중요한 기능을 손상시키지 않는지 확인

### 보안 제한사항
- **심층 방어**: Ghost는 보안의 한 계층이며 완전한 솔루션이 아님
- **지속적 관리**: 보안은 지속적인 모니터링과 업데이트가 필요
- **사용자 교육**: 기술적 제어는 보안 인식과 함께 사용해야 함
- **위협 진화**: 새로운 공격 방법이 현재의 보호를 우회할 수 있음

## 🎯 공격 시나리오 예제

Ghost가 일반적인 공격 벡터를 대상으로 하지만, 구체적인 예방은 적절한 구현과 테스트에 달려 있습니다:

### WannaCry 스타일 공격
- **완화**: `Set-Ghost -SMBv1`이 취약한 프로토콜을 비활성화
- **고려사항**: SMBv1이 필요한 레거시 시스템이 없는지 확인

### RDP 기반 랜섬웨어
- **완화**: `Set-Ghost -RDP`가 원격 데스크톱 액세스를 차단
- **고려사항**: 대체 원격 액세스 방법이 필요할 수 있음

### 문서 기반 악성코드
- **완화**: `Set-Ghost -Macros`가 매크로 실행을 비활성화
- **고려사항**: 합법적인 매크로 지원 문서에 영향을 줄 수 있음

### USB 전송 위협
- **완화**: `Set-Ghost -USBStorage -AutoRun`이 USB 기능을 제한
- **고려사항**: 합법적인 USB 장치 사용에 영향을 줄 수 있음

## 🏢 기업 기능

### 그룹 정책 지원
```powershell
# 그룹 정책 레지스트리를 통한 설정 적용
Set-Ghost -SMBv1 -RDP -AutoRun -GroupPolicy

# GP 새로 고침 후 도메인 전체에 설정 적용
gpupdate /force
```

### Microsoft Intune 통합
```powershell
# Ghost 설정에 대한 Intune 정책 생성
Set-IntuneGhost -Settings $GhostSettings -Interactive

# 정책이 관리 장치에 자동으로 배포됨
```

### 규정 준수 보고
```powershell
# 보안 평가 보고서 생성
Get-Ghost | Export-Csv -Path "SecurityAudit-$(Get-Date -Format 'yyyy-MM-dd').csv"

# Azure 보안 자세 보고서
Get-AzureGhost | Out-File "AzureSecurityReport.txt"
```

## 📚 모범 사례

### 배포 전
1. **현재 상태 문서화**: 변경하기 전에 `Get-Ghost` 실행
2. **철저한 테스트**: 비프로덕션 환경에서 검증
3. **롤백 계획**: 각 설정을 되돌리는 방법 숙지
4. **이해관계자 검토**: 비즈니스 단위가 변경사항을 승인했는지 확인

### 배포 중
1. **단계별 접근**: 먼저 파일럿 그룹에 배포
2. **영향 모니터링**: 사용자 불만이나 시스템 문제 관찰
3. **문제 문서화**: 향후 참고를 위해 모든 문제 기록
4. **변경사항 소통**: 사용자에게 보안 개선사항 알림

### 배포 후
1. **정기 평가**: 설정을 확인하기 위해 정기적으로 `Get-Ghost` 실행
2. **문서 업데이트**: 보안 구성을 최신 상태로 유지
3. **효과성 검토**: 보안 사고 모니터링
4. **지속적 개선**: 위협 환경에 기반하여 설정 조정

## 🔧 문제 해결

### 일반적인 문제
- **권한 오류**: PowerShell 세션이 관리자 권한으로 실행되었는지 확인
- **서비스 종속성**: 일부 서비스에는 종속성이 있을 수 있음
- **애플리케이션 호환성**: 비즈니스 애플리케이션으로 테스트
- **네트워크 연결**: 원격 액세스가 여전히 작동하는지 확인

### 복구 옵션
```powershell
# 필요한 경우 특정 서비스 재활성화
Set-RDP -Enable
Set-SMBv1 -Enable
Set-AutoRun -Enable
Set-Macros -Enable
```

## 👨‍💻 저자 소개

**Jim Tyler** - PowerShell Microsoft MVP
- **YouTube**: [@PowerShellEngineer](https://youtube.com/@PowerShellEngineer) (구독자 10,000+)
- **뉴스레터**: [PowerShell.News](https://powershell.news) - 주간 보안 정보
- **저자**: "PowerShell for Systems Engineers"
- **경험**: PowerShell 자동화 및 Windows 보안 분야 수십 년 경험

## 📄 라이선스 및 면책 조항

### MIT 라이선스
Ghost는 무료 사용, 수정 및 배포를 위해 MIT 라이선스 하에 제공됩니다.

### 보안 면책 조항
- **보증 없음**: Ghost는 어떠한 종류의 보증도 없이 "있는 그대로" 제공됩니다
- **테스트 필요**: 항상 비프로덕션 환경에서 먼저 테스트하십시오
- **전문가 지침**: 프로덕션 배포를 위해서는 보안 전문가와 상의하십시오
- **운영상 영향**: 저자들은 운영 중단에 대해 책임지지 않습니다
- **포괄적 보안**: Ghost는 완전한 보안 전략의 한 구성 요소입니다

### 지원
- **GitHub 이슈**: [버그 신고 또는 기능 요청](https://github.com/jimrtyler/Ghost/issues)
- **문서**: 자세한 도움말을 위해 `Get-Help <function> -Full` 사용
- **커뮤니티**: PowerShell 및 보안 커뮤니티 포럼

---

**🔒 Ghost로 보안 자세를 강화하세요 - 하지만 항상 먼저 테스트하십시오.**

```powershell
# 추측이 아닌 평가로 시작하세요
Get-Ghost
```

**⭐ Ghost가 보안 자세 개선에 도움이 된다면 이 리포지토리에 별표를 표시하세요!**