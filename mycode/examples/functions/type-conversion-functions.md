## Terraform Type Conversion Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 유형 변환과 관련된 기능을 정리한다.

---

### can

- 주어진 표현식을 평가하고 표현식이 오류 없이 결과를 생성했는지 여부를 반환한다.
- 인수를 평가할 때 생성된 오류를 잡을 수 있는 특수함수이며 대부분의 상황에서 `can`보다 `try`를 사용하는 것이 권장된다.
- 일반적으로 `can`은 변수 유효성 검사 규칙의 단순 테스트에만 사옹된다.

```shell
$ terraform console
> local.foo
{
  "bar" = "baz"
}
> can(local.foo.bar)
true
> can(local.foo.boop)
false
```

---

### sensitive, nonsensitive

- `sensitive`: 모든 값을 가져와서 Terraform이 민감한 입력 변수와 동일한 의미 및 동작으로 민감한 것으로 취급하도록 표시된 복사본을 반환한다.
- `nonsensitive`: 민감한 값을 가져와 민감한 표시가 제거된 해당 값의 복사본을 반환하므로 민감한 값이 노출된다.

```shell
$ terraform console
> sensitive("foo bar")
(sensitive value)
> sensitive([])
(sensitive value)
> nonsensitive(sensitive("foo bar"))
"foo bar"
```

---

### tobool, tolist, tomap, tonumber, toset, tostring

- `tobool`: 인수를 부울 값으로 변환한다.
- `tolist`: 인수를 목록 값으로 변환한다.
- `tomap`: 인수를 맵 값으로 변환한다.
- `tonumber`: 인수를 숫자 값으로 변환한다.
- `toset`: 인수를 Set 값으로 변환한다.
- `tostring`: 인수를 문자열 값으로 변환한다.

```shell
$ terraform console
> tobool(true)
true
> tobool("true")
true

> tolist(["foo", "bar"])
tolist([
  "foo",
  "bar",
])

> tomap({"foo" = 1, "bar" = 2})
tomap({
  "bar" = 2
  "foo" = 1
})

> tonumber(1)
1
> tonumber("1")
1

> toset(["foo", "bar", "foo"])
toset([
  "bar",
  "foo",
])

> tostring("1")
"1"
> tostring(1)
"1"
> tostring(true)
"true"
```

---

### try

- 모든 인수 표현식을 순서대로 평가하고 오류를 생성하지 않는 첫 번째 표현식의 결과를 반환한다.

```shell
$ terraform console
> local.foo
{
  "bar" = "baz"
}
> try(local.foo.bar, "fallback")
baz
> try(local.foo.boop, "fallback")
fallback
```

---

### type

- 주어진 값의 유형을 반환한다.
- `terraform console`에서만 사용할 수 있는 특수 기능이다.

```shell
$ terraform console
> type("string")
string
> type(1)
number
> type([1, 2])
tuple([
    number,
    number,
])
```

---

### 참고한 자료

- [can](https://developer.hashicorp.com/terraform/language/functions/can)
- [nonsensitive](https://developer.hashicorp.com/terraform/language/functions/nensensitive)
- [sensitive](https://developer.hashicorp.com/terraform/language/functions/sensitive)
- [tobool](https://developer.hashicorp.com/terraform/language/functions/tobool)
- [tolist](https://developer.hashicorp.com/terraform/language/functions/tolist)
- [tomap](https://developer.hashicorp.com/terraform/language/functions/tomap)
- [tonumber](https://developer.hashicorp.com/terraform/language/functions/tonumber)
- [toset](https://developer.hashicorp.com/terraform/language/functions/toset)
- [tostring](https://developer.hashicorp.com/terraform/language/functions/tostring)
- [try](https://developer.hashicorp.com/terraform/language/functions/try)
- [type](https://developer.hashicorp.com/terraform/language/functions/type)