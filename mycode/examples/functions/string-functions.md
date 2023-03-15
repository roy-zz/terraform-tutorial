## Terraform String Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 문자와 관련된 기능을 정리한다.

---

### chomp

- 입력된 문자열 끝에 있는 개행 문자를 제거한다.
- 문자열 중간에 있는 개행 문자는 제거하지 않으므로 주의해야 한다.

```shell
$ terraform console
> chomp("helloworld\n")
"helloworld"
> chomp("helloworld\r\n")
"helloworld"
> chomp("hello\nworkd\n")
<<EOT
hello
workd
EOT
```

---

### endswith

- "입력된 문자열"이 "입력된 접미사"로 끝나는 경우 `true`를 반환한다.
- `endswith(${문자열}, ${접미사})` 형식으로 사용한다.

```shell
$ terraform console
> endswith("foo bar", "bar")
true
> endswith("foo bar", "foo")
false
```

---

### format

- 사양 문자열에 따라 여러 다른 값의 형식을 지정하여 문자열을 생성한다.
- "C언어"의 `printf`와 유사한 기능을 한다.
- `format(${spec}, ${values...})` 형식으로 사용한다.

```shell
$ terraform console
> format("foo %s!", "bar")
"foo bar!"
```

- `%#v`는 모든 유형의 값을 허용하고 "jsonencode"와 유사한 JSON 인코딩을 사용한다.

```shell
$ terraform console
> format("%#v", "foo")
"\"foo\""
> format("%#v", true)
"true"
> format("%#v", {a=1})
"{\"a\":1}"
```

- 자세한 사양 문자열은 [공식문서](https://developer.hashicorp.com/terraform/language/functions/format)를 참고한다.

---

### formatlist

- 사양 문자열에 따라 여러 다른 값의 형식을 지정하여 문자열 목록을 생성한다.

```shell
$ terraform console
> formatlist("foo, %s!", ["bar1", "bar2", "bar3", "bar4"])
tolist([
  "foo, bar1!",
  "foo, bar2!",
  "foo, bar3!",
  "foo, bar4!",
])
```

---

### indent

- 지정된 여러 줄 문자열에서 첫 번째 줄을 제외한 모든 줄의 시작 부분에 지정된 수의 공백을 추가한다.
- `indent(${공백의 수}, ${문자열})` 형식으로 사용한다.

```shell
$ terraform console
> "  items: ${indent(2, "[\n  foo,\n  bar,\n]\n")}"
<<EOT
  items: [
    foo,
    bar,
  ]
  
EOT
```

---

### join

- 주어진 문자열 목록의 모든 요소를 주어진 구분 기호와 함께 연결하여 문자열을 생성한다.
- `join(${구분자}, ${문자열 목록})` 형식으로 사용한다.

```shell
$ terraform console
> join(", ", ["foo", "bar1", "bar2", "bar3"])
"foo, bar1, bar2, bar3"
```

---

### lower

- 주어진 문자열을 소문자로 변환한다.

```shell
$ terraform console
> lower("foo BAR")
"foo bar"
```

---

### regex

- 문자열에 정규식을 적용하고 일치하는 하위 문자열을 반환한다.
- `regex(${표현식}, ${문자열})` 형식으로 사용한다.

```shell
> regex("[a-z]+", "11111aaabbbccc22222")
"aaabbbccc"
> regex("(\\d\\d\\d\\d)-(\\d\\d)-(\\d\\d)", "2019-02-01")
[
  "2019",
  "02",
  "01",
]
> regex("^(?:(?P<scheme>[^:/?#]+):)?(?://(?P<authority>[^/?#]*))?", "https://terraform.io/docs/")
{
  "authority" = "terraform.io"
  "scheme" = "https"
}
```

- 정규 표현식에 대한 자세한 정보는 [공식문서](https://developer.hashicorp.com/terraform/language/functions/regex)를 참고한다.

---

### regexall

- `regex`와 동일한 정규식을 사용하며 정규식을 문자열에 적용하고 일치하는 모든 목록을 반환한다.
- `regexall(${표현식}, ${문자열})` 형식으로 사용한다.

```shell
$ terraform console
> regexall("[a-z]+", "1111aaaa2222bbbb3333")
tolist([
  "aaaa",
  "bbbb",
])
```

- 주어진 목록과 일치하는 항목이 있는지 검사하는 용도로 사용할 수 있다.

```shell
$ terraform console
> length(regexall("[a-z]+", "1111aaaa2222bbbb3333"))
2
> length(regexall("[a-z]+", "111122223333")) > 0
false
```

---

### replace

- 주어진 문자열에서 대상 문자열을 검색하고 검색된 각 항목을 대체 문자열로 변경한다.
- `replace(${문자열}, ${대상 문자열}, ${대체 문자열})` 형식으로 사용한다.

```shell
$ terraform console
> replace("1+2+3", "+", "*")
"1*2*3"
> replace("foo vara, foo varb, foo varc", "/var*/", "bar")
"foo bara, foo barb, foo barc"
```

---

### split

- 주어진 문자열을 주어진 구분 기호로 나누어진 목록을 생성한다.

```shell
$ terraform console
> split(",", "foo,bar,baz")
tolist([
  "foo",
  "bar",
  "baz",
])
```

---

### startswith

- "입력된 문자열"이 "입력된 접두사"로 시작하는 경우 `true`를 반환한다.
- `startswith(${문자열}, ${접두사})` 형식으로 사용한다.

```shell
$ terraform console
> startswith("foo bar", "foo")
true
> startswith("poo bar", "foo")
false
```

---

### strrev

- 입력된 문자열을 뒤집는다. 문자는 유니코드 문자로 취급된다.

```shell
$ terraform console
> strrev("foo bar")
"rab oof"
```

---

### substr

- 주어진 문자열에서 오프셋부터 주어진 길이만큼의 문자열을 추출한다.
- `substr(${문자열}, ${오프셋}, ${길이})` 형식으로 사용한다.
- 오프셋 인덱스는 음수가 될 수 있으며, 이 경우 문자열의 끝을 기준으로 계산한다.
- 길이는 `-1`이 될 수 있으며, 이 경우 오프셋 이후의 모든 문자열을 반환한다.

```shell
$ terraform console
> substr("foo bar", 3, 6)
" bar"
> substr("foo bar", -4, 6)
" bar"
> substr("foo bar", 3, -1)
" bar"
> substr("foo bar", -4, -1)
" bar"
```

---

### title

- 주어진 문자열의 각 단어의 첫 글자를 대문자로 변환한다.

```shell
$ terraform console
> title("foo bar")
"Foo Bar"
```

---

### trim

- 주어진 문자열의 시작과 끝에서 지정된 문자 집합을 제거한다.
- 주어진 문자열의 중간에 있는 문자 집합은 제거하지 않으므로 사용에 주의해야 한다.
- `trim(${문자열}, ${제거할 문자 집합})` 형식으로 사용한다.

```shell
$ terraform console
> trim("!?foo bar!?", "!?")
"foo bar"
> trim("!?foo!?bar!?", "!?")
"foo!?bar"
```

---

### trimprefix

- 주어진 문자열의 시작 부분에 주어진 접두사가 있다면 제거한다.

```shell
$ terraform console
> trimprefix("foobar", "foo")
"bar"
> trimprefix("poobar", "foo")
"poobar"
> trimprefix("foofoobar", "foo")
"foobar"
```

---

### trimsuffix

- 주어진 문자열의 끝 부분에 주어진 접미사가 있다면 제거한다.

```shell
$ terraform console
> trimsuffix("foobar", "bar")
"foo"
> trimsuffix("foovar", "bar")
"foovar"
> trimsuffix("foobarbar", "bar")
"foobar"
```

---

### trimspace

- 주어진 문자열의 시작과 끝에 있는 공백을 제거한다.
- 문자열 중간의 공백은 제거되지 않으므로 사용에 주의해야 한다.

```shell
$ terraform console
> trimspace(" foo bar ")
"foo bar"
> trimspace("foo bar")
"foo bar"
```

---

### upper

- 주어진 문자열을 대문자로 변환한다.

```shell
$ terraform console
> upper("foo BAR")
"FOO BAR"
```

---

### 참고한 자료

- [chomp](https://developer.hashicorp.com/terraform/language/functions/chomp)
- [endswith](https://developer.hashicorp.com/terraform/language/functions/endswith)
- [format](https://developer.hashicorp.com/terraform/language/functions/format)
- [formatlist](https://developer.hashicorp.com/terraform/language/functions/formatlist)
- [indent](https://developer.hashicorp.com/terraform/language/functions/indent)
- [join](https://developer.hashicorp.com/terraform/language/functions/join)
- [lower](https://developer.hashicorp.com/terraform/language/functions/lower)
- [regex](https://developer.hashicorp.com/terraform/language/functions/regex)
- [regexall](https://developer.hashicorp.com/terraform/language/functions/regexall)
- [replace](https://developer.hashicorp.com/terraform/language/functions/replace)
- [split](https://developer.hashicorp.com/terraform/language/functions/split)
- [startswith](https://developer.hashicorp.com/terraform/language/functions/startwith)
- [strrev](https://developer.hashicorp.com/terraform/language/functions/strrev)
- [substr](https://developer.hashicorp.com/terraform/language/functions/substr)
- [title](https://developer.hashicorp.com/terraform/language/functions/title)
- [trim](https://developer.hashicorp.com/terraform/language/functions/trim)
- [trimprefix](https://developer.hashicorp.com/terraform/language/functions/trimprefix)
- [trimsuffix](https://developer.hashicorp.com/terraform/language/functions/trimsuffix)
- [trimspace](https://developer.hashicorp.com/terraform/language/functions/trimspace)
- [upper](https://developer.hashicorp.com/terraform/language/functions/upper)