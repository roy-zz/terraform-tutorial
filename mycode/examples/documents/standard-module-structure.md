
## 표준 모듈 구조

표준 모듈 구조는 별도의 저장소에 배포된 재사용 가능한 모듈에 대해 권장하는 파일 및 디렉토리 레이아웃이다.
"Terraform 도구"는 표준 모듈 구조를 이해하고 해당 구조를 사용하여 문서, 모듈 레지스트리용 색인 모듈 등을 생성하도록 구축되어있다.

표준 모듈 구조는 아래에 설명된 레이아웃을 예상한다. 목록이 길어 보일 수 있지만 루트 모듈을 제외한 모든 항목은 필요한 경우 도입할 수 있는 선택사항이다.
대부분의 모듈은 표준 구조를 따르기 위해 추가 작업을 수행할 필요가 없다.

---

### 루트 모듈(Root Module)

루트 모듈은 표준 모듈 구조에 필요한 **유일한 필수 요소**다.
"Terraform 파일"은 리포지토리의 루트 디렉터리에 있어야 한다. 이것은 모듈의 기본 진입점이 되어야 하며 의견이 있을 것으로 예상된다.  
["Consul 모듈"](https://registry.terraform.io/modules/hashicorp/consul)의 경우 루트 모듈은 완전한 "Consul 클러스터"를 설정한다.
그러나 많은 가정을 하고 있으며 숙련자는 특정 중첩 모듈을 사용하여 원하는 것을 보다 신중하게 제어할 것으로 기대한다.

### README.md

"루트 모듈"과 "모든 중첩 모듈"에는 "README.md" 파일이 있어야 한다. 이 파일에는 모듈에 대한 설명과 용도에 대한 설명이 있어야 한다.
이 모듈을 다른 리소스와 함께 사용하는 방법에 대한 예제를 포함하려면 "examples 디렉토리"에 넣어야 한다.
모듈이 생성할 수 있는 인프라 리소스와 해당 관계를 나타내는 **시각적 다이어그램**을 포함하는 것이 좋다.

도구가 자동으로 생성하기 때문에 "README.md 파일"은 모듈의 `inputs` 또는 `outputs` 문서화할 필요가 없다.
파일에 연결하거나 리포지토리 자체에 포함된 이미지를 포함하는 경우 커밋별 절대 URL을 사용하여 나중에 링크가 리소스의 잘못된 버전을 가리키지 않도록 한다.

### 라이선스(License)

이 모듈을 사용할 수 있는 라이선스다.
모듈을 공개적으로 게시하는 경우 명확한 라이선스가 없으면 많은 조직에서 모듈을 채택하지 않는다.
오픈 소스 라이선스가 아니더라도 **항상 라이선스 파일을 보유하는 것**이 좋다.

### 필수 파일(main.tf, variables.tf, outputs.tf)

내용이 없더라도 최소한의 모듈에 권장되는 파일 목록이다.
`main.tf`은 기본 진입점이 되어야 한다. 간단한 모듈의 경우 여기에서 모든 리소스가 생성될 수 있다.
복잡한 모듈의 경우 리소스 생성이 여러 파일로 분할될 수 있지만 모든 중첩 모듈 호출은 기본 파일에 있어야 한다.
`variables.tf`, `outputs.tf` 파일을 통해 **"변수" 및 "출력"에 대한 선언**을 각각 포함해야 한다.

### 변수 및 출력(Variable & Output)

모든 변수와 출력에는 **목적을 설명**하는 한두 문장의 설명이 있어야 한다. 이는 문서화에 사용된다.
자세한 내용은 ["변수 구성"](https://developer.hashicorp.com/terraform/language/values/variables) 및 ["출력 구성"](https://developer.hashicorp.com/terraform/language/values/outputs) 에 대한 설명서를 참조한다.

### 중첩 모듈(Nested module)

중첩된 모듈은 `modules/` 하위 디렉터리 아래에 있어야 한다. "README.md" 가 포함된 모든 중첩 모듈은 외부 사용자가 사용할 수 있는 것으로 간주된다.
"README.md"가 없으면 내부용으로만 간주되지만 이것은 순전히 Terraform 공식문서의 권고사항이다. "Terraform"은 내부 모듈 사용을 적극적으로 거부하지 않는다.
복잡한 동작을 고급 사용자가 신중하게 선택하고 선택할 수 있는 여러 개의 작은 모듈로 분할하려면 중첩 모듈을 사용해야 한다.
예를 들어 "Consul 모듈"에는 필요한 IAM 정책을 설정하기 위해 모듈과 별도로 클러스터를 생성하기 위한 중첩 모듈이 있다. 이를 통해 사용자는 자신의 IAM 정책 선택 항목을 가져올 수 있다.
  
루트 모듈에 중첩된 모듈에 대한 호출이 포함된 경우, 이를 **별도로 다시 다운로드하지 않고** 동일한 리포지토리 또는 패키지의 일부로 간주하도록 `./modules/consul-clusterTerraform`와 같은 **상대 경로를 사용**해야 한다.
  
리포지토리 또는 패키지에 중첩된 모듈이 여러 개 포함되어 있는 경우, 서로 직접 호출하여 깊이 중첩된 모듈 트리를 만드는 대신 호출자가 이상적으로 [구성할 수](https://developer.hashicorp.com/terraform/language/modules/develop/composition) 있어야 한다.

### 예시(Example)

모듈 사용 예제는 저장소의 루트 하위 디렉토리인 `examples/`에 존재해야 한다. 각 예제에는 예제의 목표와 사용법을 설명하는 "README.md"파일이 있을 수 있다.
하위 모듈에 대한 예제도 루트 `examples/` 디렉토리에 배치해야 한다.   
예제는 사용자가 지정을 위해 다른 리포지토리에 복사되는 경우가 많기 때문에 `module` 블록은 `source`의 상대 경로가 아닌 외부 호출자가 사용할 주소로 설정되어야 한다.
  
---

표준 구조를 따르는 최소한의 권장 모듈은 아래와 같다. 루트 모듈이 유일한 필수 요소이지만 최소한 아래의 구조가 권장된다.

```
$ tree minimal-module/
.
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
```

표준 구조를 따르는 모듈의 완전한 예시는 아래와 같다. 이 예시는 모든 선택적 요소를 포함하므로 가장 복잡한 구조의 모듈이 된다.

```
$ tree complete-module/
.
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
├── ...
├── modules/
│   ├── nestedA/
│   │   ├── README.md
│   │   ├── variables.tf
│   │   ├── main.tf
│   │   ├── outputs.tf
│   ├── nestedB/
│   ├── .../
├── examples/
│   ├── exampleA/
│   │   ├── main.tf
│   ├── exampleB/
│   ├── .../
```

---

### 참고 자료

- [Terraform - Standard Module Structure](https://developer.hashicorp.com/terraform/language/modules/develop/structure)
- [Terraform - Consul Module](https://registry.terraform.io/modules/hashicorp/consul)
- [Terraform - Variables](https://developer.hashicorp.com/terraform/language/values/variables)
- [Terraform - Outputs](https://developer.hashicorp.com/terraform/language/values/outputs)
- [Terraform - Composition](https://developer.hashicorp.com/terraform/language/modules/develop/composition)