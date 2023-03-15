## Terraform Encoding Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 인코딩 및 디코딩과 관련된 기능을 정리한다.

---

### base64encode, base64decode

- `base64encode`: 입력된 문자열에 Base64 인코딩을 적용하여 반환한다.
- `base64decode`: Base64 문자 시퀀스를 포함하는 문자열을 가져와 원래 문자열을 반환한다.
- Terraform의 문자열은 바이트가 아닌 유니코드 문자 시퀀스이기 때문에 결과 바이트도 UTF-8로 해석한다.

```shell
$ terraform console
> base64encode("foo bar")
"Zm9vIGJhcg=="
> base64decode("Zm9vIGJhcg==")
"foo bar"
```

---

### base64gzip

- 문자열을 `gzip`으로 압축한 다음 결과를 Base64 인코딩을 적용하여 반환한다.

```shell
$ terraform console
> base64gzip("foo bar")
"H4sIAAAAAAAA/0rLz1dISiwCAAAA//8BAAD//zQBRr4HAAAA"
```

---

### csvecode

- CSV 형식 데이터가 포함된 문자열을 디코딩하고 해당 데이터를 나타내는 맵 목록을 반환한다.

```shell
$ terraform console
> csvdecode("foo,bar\n1,2\n3,4")
tolist([
  {
    "bar" = "2"
    "foo" = "1"
  },
  {
    "bar" = "4"
    "foo" = "3"
  },
])
```

---

### jsonencode, jsondecode

- `jsonencode`: JSON 구문을 사용하여 주어진 값을 문자열로 인코딩한다.
- `jsondecode`: 주어진 문자열을 JSON으로 해석하여 해당 문자열을 디코딩한 결과를 반환한다.

```shell
$ terraform console
> jsonencode({"foo" = "bar"})
"{\"foo\":\"bar\"}"
> jsondecode("{\"foo\":\"bar\"}")
{
  "foo" = "bar"
}
```

---

### textencodebase64, textdecodebase64

- `textencodebase64`: Terraform 언어 문자열은 항상 유니코드 문자 시퀀스이기 때문에 지정된 문자 인코딩을 사용하여 지정된 문자열의 유니코드 문자열를 인코딩하고 Base64 인코딩 결과를 반환한다.
  `textencodebase64(${문자열}, ${인코딩 이름})` 형식을 사용한다.
- `textdecodebase64`: 이전에 Base64로 인코딩된 문자열을 디코딩한 다음 결과를 지정된 문자 인코딩의 문자로 해석한 결과를 반환한다.
  `textdecodebase64(${문자열}, ${디코딩 이름})` 형식을 사용한다.

```shell
$ terraform console
> textencodebase64("foo bar", "UTF-16")
"/v8AZgBvAG8AIABiAGEAcg=="
> textdecodebase64("/v8AZgBvAG8AIABiAGEAcg==", "UTF-16")
"foo bar"
```

---

### urlencode

- 주어진 문자열에 URL 인코딩을 적용한다.

```shell
$ terraform console
> urlencode("foo bar")
"foo+bar"
> urlencode("한글")
"%ED%95%9C%EA%B8%80"
> "http://roy.com/search?query=${urlencode("foo bar")}"
"http://roy.com/search?query=foo+bar"
```

---

### yamlencode, yamldecode

- `yamlencode`: [YAML 1.2](https://yaml.org/spec/1.2/spec.html) 블록 구문을 사용하여 주어진 값을 문자열로 인코딩한다.
- `yamldecode`: 문자열을 YAML의 하위 집합으로 구문 분석하고 해당 값의 표현을 생성한다.

```shell
$ terraform console
> yamlencode({"foo": "poo", "bar": "var"})
<<EOT
"bar": "var"
"foo": "poo"

EOT

> yamlencode({"foo": [1,2], "bar": "var"})
<<EOT
"bar": "var"
"foo":
- 1
- 2

EOT

> yamldecode("foo: bar")
{
  "foo" = "bar"
}
> yamldecode("{a: &foo [1, 2, 3], b: *foo}")
{
  "a" = [
    1,
    2,
    3,
  ]
  "b" = [
    1,
    2,
    3,
  ]
}
```

---

### 참고한 자료

- [base64decode](https://developer.hashicorp.com/terraform/language/functions/base64decode)
- [base64encode](https://developer.hashicorp.com/terraform/language/functions/base64encode)
- [base64gzip](https://developer.hashicorp.com/terraform/language/functions/base64gzip)
- [csvecode](https://developer.hashicorp.com/terraform/language/functions/csvecode)
- [jsondecode](https://developer.hashicorp.com/terraform/language/functions/jsondecode)
- [jsonencode](https://developer.hashicorp.com/terraform/language/functions/jsonecode)
- [textdecodebase64](https://developer.hashicorp.com/terraform/language/functions/textdecodebase64)
- [textencodebase64](https://developer.hashicorp.com/terraform/language/functions/textencodebase64)
- [urlencode](https://developer.hashicorp.com/terraform/language/functions/urlencode)
- [yamldecode](https://developer.hashicorp.com/terraform/language/functions/yamldecode)
- [yamlencode](https://developer.hashicorp.com/terraform/language/functions/yamlencode)