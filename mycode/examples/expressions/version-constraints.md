## Terraform Version

이전에 [공급자의 버전 관리와 업그레이드](https://imprint.tistory.com/389)에 대해서 알아본 적이 있다.
이번에는 코드로 버전의 제약하는 방법을 구체적으로 알아보도록 한다.

---

### 버전 제약(Version Constraints)

- 테라폼을 사용하여 허용 가능한 버전 범위를 지정할 수 있는 모든 곳에서 버전 제약 조건으로 알려진 특수 형식의 문자열이 필요하다.
- 하나의 버전을 지정하기 위해 여러 조건이 중첩되어 사용될 수 있으므로 사용자는 테라폼이 어떤 방식으로 버전을 지정하는지 이해하고 있어야 한다.
- 아래는 대표적으로 버전 제약 조건이 사용되는 곳이다.
  - [Modules](https://developer.hashicorp.com/terraform/language/modules)
  - [Provider Requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
  - [required_versions 블록](https://developer.hashicorp.com/terraform/language/settings#specifying-a-required-terraform-version)

```terraform
version = ">= 1.2.0, < 2.0.0"
```

- 버전 제약 조건은 쉼포로 구분된 하나 이상의 조건을 포함하는 문자열이며, 각 조건은 연산자와 버전 번호로 구성된다.
- 버전 번호는 `1.2.0`과 같이 마침표로 구분된 일련의 번호여야 하며 선택적으로 베타 릴리즈를 나타내는 접미사가 있어야 한다.
- 연산자의 경우 아래와 같은 표현들이 사용된다.
  - `=` or "연산자가 없는 경우": 정확한 버전 번호 하나만 허용하며 다른 조건과 조합되어 사용될 수 없다.
  - `!=0`: 정확한 버전 번호를 제외한다.
  - `>`, `>=`, `<`, `<=`: 지정된 버전과 비교하여 비교가 참인 버전을 **허용**한다. "초과 또는 이상"은 최신 버전을 요청하고, "미만 또는 이하"는 이전 버전을 요청한다.
  - `~>`: 비관적 제약 조건 연산자라고 부르며 가장 오른쪽 버전 요소만 증분할 수 있다.
    예를 들어, `~> 1.0.4`와 같이 제약 조건이 설정된 경우, `1.0.5`, `1.0.10` 버전은 허용하지만 `1.1.0`은 허용하지 않는다.

#### 버전 제약 동작

- 적용 가능한 모든 제약 조건을 충족하는 버전 번호는 허용되는 것으로 간주된다. 즉, 허용되는 여러 버전이 있을 수 있다.
- 테라폼은 버전 제약 조건을 참조하여 허용 가능한 자체 버전, 필수 공급자 플러그인 및 필수 모듈이 있는지 확인한다.
  플러그인 및 모듈의 경우 해당 제약 조건을 충족하는 버전 목록에서 "설치되어 있는 버전" 중 "최신 버전"을 사용한다.
- 테라폼은 필요한 플러그인 또는 모듈의 허용 가능한 버전이 없는 경우 적용 가능한 제약 조건을 충족하는 최신 버전을 다운로드하려고 시도한다.
- 테라폼이 허용 가능한 버전의 외부 종속성을 얻을 수 없거나 허용 가능한 자체 버전이 없는 경우 `plan`, `apply` 또는 상태 조작 작업을 진행하지 않는다.
- 루트 모듈과 하위 모듈 모두 허용 가능한 버전의 테라폼과 이들이 사용하는 공급자를 제한할 수 있으며, 테라폼은 이러한 제약 조건이 동일하다고 간주하고 모든 조건을 충족할 수 있는 경우에만 진행한다.
- `1.2.0-beta`와 같은 시험판 버전은 `=`연산자로 정확히 지정해야 사용할 수 있다.

#### 모범 사례

- 타사 모듈에 의존하는 경우 업데이트가 편리한 경우에만 업데이트가 수행되도록 특정 버전이 필요하다.
- 조직 내에서 유지 관리되는 모듈의 경우 시맨틱 **버전 관리가 일관되게 사용**되거나 **원하지 않는 업데이트를 방지**하는 잘 정의된 릴리즈 프로세스가 있는 경우 버전 범위를 지정하는 것이 적절할 수 있다.
- 재사용 가능한 모듈은 최소 허용 버전의 테라폼 및 공급자(`>= version`)를 사용하여 비호환을 방지하는 동시에 모듈을 변경하지 않고 테라폼을 업그레이드할 수 있다.
- 루트 모듈은 `~>` 제약 조건을 사용하여 **의존하는 각 공급자의 버전에 대한 하한 및 상한을 모두 설정**해야 한다.

---

### 참고한 자료

- [Version Constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
- [공급자(Provider) 버전 관리](https://imprint.tistory.com/389)
- [Dependency Lock File](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [Lock and Upgrade Provider Versions](https://developer.hashicorp.com/terraform/tutorials/certification-associate-tutorials-003/provider-versioning)
- [What is Terraform Cloud](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-sign-up)
- [Create a Credentials Variable Set](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-create-variable-set)