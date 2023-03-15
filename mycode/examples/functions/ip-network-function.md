## Terraform IP Network Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 IP 및 네트워크와 관련된 기능을 정리한다.

---

### cidrhost

- 주어진 IP 네트워크 주소 접두사 내에서 주어진 호스트 번호에 대한 전체 호스트 IP 주소를 계산한다.
- `cidrhost(${접두사}, ${호스트번호})` 형식으로 사용한다.

```shell
$ terraform console
> cidrhost("10.1.1.0/20", 10)
"10.1.0.10"
> cidrhost("10.1.1.0/20", 20)
"10.1.0.20"
> cidrhost("10.1.1.0/20", 300)
"10.1.1.44"
> cidrhost("fd00:fd12:3456:7890:00a2::/72", 34)
"fd00:fd12:3456:7890::22"
```

---

### cidrnetmask

- CIDR 표기법으로 주어진 IPv4 주소 접두사를 서브넷 마스크 주소로 변환한다.

```shell
$ terraform console
> cidrnetmask("192.168.0.1/20")
"255.255.240.0"
```

---

### cidrsubnet

- 주어진 IP 네트워크 주소 접두사 내에서 서브넷 주소를 계산한다.
- `cidrsubnet(${prefix}, ${newbits}, ${netnum})` 형식으로 사용하며 각 인자의 의미는 아래와 같다.
  - `prefix`: RFC 4632에 정의된 CIDR 표기법을 따르는 문자열
  - `newbits`: `prefix`에서 확장할 추가 비트 수. 예를 들어, `prefix`가 `/16`으로 끝나고 `newbits`가 4인 경우 `prefix`가 `/20`인 것과 동일하다.
  - `netnum`: 접두사에 추가된 추가 비트를 채우는 데 사용되는 `newbits` 이진수 이하의 이진 정수로 나타낼 수 있는 정수다.

```shell
$ terraform console
> cidrsubnet("172.16.0.0/12", 4, 2)
172.18.0.0/16
> cidrsubnet("10.1.2.0/24", 4, 15)
10.1.2.240/28
> cidrsubnet("fd00:fd12:3456:7890::/56", 16, 162)
fd00:fd12:3456:7800:a200::/72
```

---

### cidrsubnets

- 특정 CIDR 접두사 내에서 일련의 연속 IP 주소 범위를 계산한다.
- `cidrsubnets(${prefix}, ${newbits...})` 형식으로 사용한다.

```shell
$ terraform console
> cidrsubnets("10.10.0.0/16", 4, 4, 4, 4)
tolist([
  "10.10.0.0/20",
  "10.10.16.0/20",
  "10.10.32.0/20",
  "10.10.48.0/20",
])
```

---

### 참고한 자료

- [cidrhost](https://developer.hashicorp.com/terraform/language/functions/cidrhost)
- [cidrnetmask](https://developer.hashicorp.com/terraform/language/functions/cidrnetmask)
- [cidrsubnet](https://developer.hashicorp.com/terraform/language/functions/cidrsubnet)
- [cidrsubnets](https://developer.hashicorp.com/terraform/language/functions/cidrsubnets)