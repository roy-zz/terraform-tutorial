## Terraform Numeric Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 숫자와 관련된 기능을 정리한다.

### abs

- 주어진 숫자의 절대값을 반환한다.

```shell
$ terraform console
> abs(20)
20
> abs(0)
0
> abs(-12.4)
12.4
```

---

### ceil

- 주어진 값보다 크거나 같은 가장 가까운 정수를 반환한다.

```shell
$ terraform console
> ceil(1)
1
> ceil(1.1)
2
```

---

### floor

- 주어진 값보다 작거나 같은 가장 가까운 정수를 반환한다.

```shell
$ terraform console
> floor(1)
1
> floor(1.1)
1
```

---

### log

- 주어진 밑(base)에서 주어진 숫자(number)의 로그를 반환한다.
- `log(${숫자}, ${밑})` 형식으로 사용한다.

```shell
$ terraform console
# log[10]{50}
> log(50,10)
1.6989700043360185
# log[2]{17}
> log(17,2)
4.08746284125034
```

- `log`와 `ceil`을 함께 사용하여 고유한 값을 표현하기 위해 필요한 최소 이진수를 찾을 수 있다.

```shell
$ terraform console
> ceil(log(17,2))
5
> ceil(log(16,2))
4
> ceil(log(15,2))
4
```

---

### max

- 하나 이상의 숫자 모음 중에서 가장 큰 숫자를 반환한다.
- 컬렉션의 경우 컬렉션 인수(argument) 뒤에 `...`를 붙여 사용할 수 있다.

```shell
$ terraform console
> max(100,200,300)
300
> max([100,200,300]...)
300
```

---

### min

- 하나 이상의 숫자 모음 중에서 가장 작은 숫자를 반환한다.
- 컬렉션의 경우 컬렉션 인수(argument) 뒤에 `...`를 붙여 사용할 수 있다.

```shell
$ terraform console
> min(100,200,300)
100
> min([100,200,300]...)
100
```

---

### parseint

- 주어진 문자열을 지정된 기준의 정수 표현을 기반으로 숫자로 변환한다.
- 지정된 기준의 정수 표현은 2 ~ 62 범위내에 있어야 한다.
- 11 ~ 36진법의 경우 대소문자를 구분하지 않는 라틴 문자를 사용하여 더 높은 단위 값을 나타낸다.
- 37진법 이상의 경우 소문자 라틴 문자를 사용한 다음 대문자 라틴 문자를 사용한다.
- `parseint(${문자열}, ${정수 표현})` 형식으로 사용한다.

```shell
$ terraform console
> parseint("100", 10)
100
> parseint("FF", 16)
255
> parseint("ff", 32)
495
> parseint("1010101010", 2)
682
> parseint("bB", 62)
719
```

---

### pow

- 첫번째 인수를 두번째 인수로 거듭제곱한 값을 반환한다.

```shell
$ terraform console
> pow(10,2)
100
> pow(10,0)
1
```

---

### signum

- 주어진 수의 부호에 따른 값을 반환한다.
- 입력된 인수가 음수인 경우 `-1`, 0인 경우 `0`, 양수인 경우 `1`을 각각 반환한다.

```shell
$ terraform console
> signum(-100)
-1
> signum(0)
0
> signum(100)
1
```

---

### 참고한 자료

- [abs](https://developer.hashicorp.com/terraform/language/functions/abs)
- [ceil](https://developer.hashicorp.com/terraform/language/functions/ceil)
- [floor](https://developer.hashicorp.com/terraform/language/functions/floor)
- [log](https://developer.hashicorp.com/terraform/language/functions/log)
- [max](https://developer.hashicorp.com/terraform/language/functions/max)
- [min](https://developer.hashicorp.com/terraform/language/functions/min)
- [parseint](https://developer.hashicorp.com/terraform/language/functions/parseint)
- [pow](https://developer.hashicorp.com/terraform/language/functions/pow)
- [signum](https://developer.hashicorp.com/terraform/language/functions/signum)