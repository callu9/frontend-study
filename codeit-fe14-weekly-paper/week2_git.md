
# 1. Branch Merge 방법들과 각 방법의 특징

Git에서 브랜치를 병합(Merge)하는 방법은 크게 Fast-forward Merge와 Three-way Merge 두 가지가 있습니다. 여기에 추가로 병합 대신 Rebase를 활용하는 방법도 있습니다. 

### (1) Fast-forward Merge
- 설명: 브랜치를 병합할 때, 두 브랜치 사이에 추가된 분기가 없으면, Git은 단순히 브랜치의 끝을 앞으로 옮기기만 합니다. 즉, 새로운 커밋 없이 병합이 이루어집니다.
- 특징:
  - 이력(log)이 깔끔하게 한 줄로 보입니다.
  - 병합 커밋(Merge Commit)이 생성되지 않습니다.
- 예시: “새로운 파일을 수정했는데, 수정된 파일이 다른 사람의 작업과 충돌하지 않으면 그냥 간단히 끝내는 느낌이에요.”

### (2) Three-way Merge
- 설명: 두 브랜치에서 각각의 작업이 이루어졌을 때, Git이 두 브랜치의 공통 조상(Common Ancestor)을 기준으로 병합 커밋을 생성합니다.
- 특징:
  - 병합 커밋이 생성됩니다. (병합된 시점을 명확히 알 수 있음)
  - 충돌이 있을 경우, 이를 해결해야 합니다.
- 예시: “두 사람이 각자 편집한 내용을 합치면서, 서로 겹치는 부분이 있다면 조율을 하는 과정이라고 보면 돼요.”

### (3) Rebase
- 설명: 한 브랜치의 커밋을 다른 브랜치 위에 덮어씌우는 방식입니다. 기존의 커밋 이력을 새로 정리합니다.
- 특징:
  - 이력이 깔끔하게 한 줄로 정리됩니다.
  - 원래 있던 커밋 이력이 변경되므로 협업 중에는 신중하게 사용해야 합니다.
- 예시: “작업을 다시 정리해서 깔끔하게 제출하는 것과 비슷해요.”


# 2. Git Flow 브랜치 전략

Git Flow는 소프트웨어 개발 프로젝트를 효율적으로 관리하기 위한 브랜치 전략입니다. 다양한 작업을 분리하고 체계적으로 관리할 수 있도록 도와줍니다.

### (1) 주요 브랜치

1. Main (또는 Master) 브랜치
- 항상 배포 가능한 코드만 포함합니다.
- 제품의 안정적인 버전이 여기에 저장됩니다.

2. Develop 브랜치
- 새로운 기능 개발이 이루어지는 브랜치입니다.
- 개발 중인 코드를 통합하고 테스트합니다.

### (2) 보조 브랜치

1. Feature 브랜치
    - 새로운 기능을 개발할 때 사용합니다.
    - Develop 브랜치에서 생성하고, 작업 완료 후 Develop에 병합합니다.
    - 예시: “앱에 새로운 버튼을 추가하는 기능을 작업할 때”

2. Release 브랜치
    - 배포 준비를 위한 브랜치입니다.
    - Develop에서 생성하고, 최종 점검 및 버그 수정을 진행한 뒤 Main에 병합합니다.
    - 예시: “앱의 새 버전을 출시하기 전에 최종 검토하는 과정”

3. Hotfix 브랜치
    - Main에서 발견된 긴급 버그를 수정할 때 사용합니다.
    - Main에서 생성하고, 수정 완료 후 Main과 Develop에 병합합니다.
    - 예시: “사용자가 앱을 쓰다가 중요한 에러를 발견했을 때”


### 요약
Git Flow는 프로젝트를 안정적으로 유지하면서 동시에 여러 사람이 협업할 수 있도록 설계된 방식이에요. 
마치 “각각의 작업을 방별로 나눠서 정리한 후에 필요할 때 적절한 방에서 가져다 쓰는 것”과 같다고 생각하면 됩니다!

