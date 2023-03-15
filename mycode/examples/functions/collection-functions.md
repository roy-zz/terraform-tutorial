## Terraform Collection Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 컬렉션과 관련된 기능을 정리한다.

---

### alltrue

- 지정된 컬렉션의 모든 요소가 `true`인 경우 `true`를 반환한다.
- 컬렉션이 비어 있는 경우에도 `true`를 반환한다.

```shell
$ terraform console
> alltrue(["true", true])
true
> alltrue([true, false])
false
> alltrue([])
true
```

---

### anytrue

- 지정된 컬렉션의 요소 중 하나라도 `true`가 있으면 `true`를 반환한다.
- 컬렉션이 비어 있는 경우에는 `false`를 반환한다.

```shell
$ terraform console
> anytrue([true, true])
true
> anytrue([true, false])
true
> anytrue([false, false])
false
> anytrue([])
false
```

---

### chunklist

- 컬렉션을 지정된 청크 사이즈로 분할한 목록을 반환한다.
- `chunklist(${분할대상 목록}, ${청크 사이즈})` 형식으로 사용한다.

```shell
$ terraform console
> chunklist(["f","o","o","b","a","r"], 3)
tolist([
  tolist([
    "f",
    "o",
    "o",
  ]),
  tolist([
    "b",
    "a",
    "r",
  ]),
])
```

---

### coalesce

- 입력받은 복수의 인수 중에서 `null` 또는 빈 문자열이 아닌 첫 번째 값을 반환한다.
- 인수 뒤에 `...`을 붙여서 컬렉션을 인수로 사용할 수 있다.

```shell
$ terraform console
> coalesce("a", "b")
"a"
> coalesce(null, "a")
"a"
> coalesce("", "a")
"a"
> coalesce([null, "a"]...)
"a"
> coalesce(["", "a"]...)
"a"
```

---

### coalescelist

- 입력받은 복수의 컬렉션 인수 중에서 비어있지 않은 첫 번째 인수를 반환한다.
- 인수 뒤에 `...`을 붙여서 컬렉션을 인수로 사용할 수 있다.

```shell
$ terraform console
> coalescelist(["f","o","o"],["b","a","r"])
[
  "f",
  "o",
  "o",
]
> coalescelist([],["b","a","r"])
[
  "b",
  "a",
  "r",
]
> coalescelist([[],["b","a","r"]]...)
[
  "b",
  "a",
  "r",
]
```

---

### compact

- 컬렉션에서 `null`과 빈 문자열을 제거한 새로운 컬렉션을 생성한다.

```shell
$ terraform console
> compact(["f","","o","o"])
tolist([
  "f",
  "o",
  "o",
])
> compact(["f",null,"o","o"])
tolist([
  "f",
  "o",
  "o",
])
```

---

### concat 

- 둘 이상의 컬렉션을 하나의 컬렉션으로 합친다.

```shell
$ terraform console
> concat(["f","o","o"],["b","a","r"],["!","!","!"])
[
  "f",
  "o",
  "o",
  "b",
  "a",
  "r",
  "!",
  "!",
  "!",
]
```

---

### contains

- 주어진 컬렉션에 주어진 단일 값의 포함 여부를 반환한다.

```shell
$ terraform console
> contains(["foo","bar"], "foo")
true
> contains(["poo","bar"], "foo")
false
```

---

### distinct

- 주어진 컬렉션에서 중복 요소를 제거한 컬렉션을 반환한다.

```shell
$ terraform console
> distinct(["foo","foo","bar","bar"])
tolist([
  "foo",
  "bar",
])
> distinct([null,null,"",""])
tolist([
  tostring(null),
  "",
])
```

---

### element

- 주어진 컬렉션에서 인덱스를 기준으로 단일 요소를 검색한다.
- `element(${컬렉션}, ${인덱스})` 형식으로 사용한다.

```shell
$ terraform console
> element(["f","o","o","b","a","r"], 3)
"b"
> element(["f","o","o","b","a","r"], 10)
"a"
> element(["f","o","o","b","a","r"], 20)
"o"
```

---

### flatten

- 컬렉션 목록에서 모든 요소를 하나의 평범한 컬렉션으로 만들어 반환한다.

```shell
$ terraform console
> flatten([["foo"],[],["bar"]])
[
  "foo",
  "bar",
]
> flatten([[["foo"],[]],["bar"]])
[
  "foo",
  "bar",
]
```

---

### index

- 주어진 컬렉션에서 주어진 값이 있는 인덱스를 반환한다.
- 주어진 값이 없는 경우 오류가 발생한다.
- 여러 값이 있는 경우 가장 빠른 인덱스가 반환된다.
- `index(${컬렉션}, ${값})` 형식으로 사용한다.

```shell
$ terraform console
> index(["f","o","o","b","a","r"], "o")
1
> index(["f","o","o","b","a","r"], "v")
╷
│ Error: Error in function call
│ 
│   on <console-input> line 1:
│   (source code not available)
│ 
│ Call to function "index" failed: item not found.
╵
```

---

### keys

- 주어진 맵에서 키를 추출하여 만들어진 목록을 반환한다.

```shell
$ terraform console
> keys({"foo"=1,"bar"=2})
[
  "bar",
  "foo",
]
> keys({"foo"=1,"bar"=2,"bar"=3})
[
  "bar",
  "foo",
]
```

---

### length

- 주어진 컬렉션, 맵, 문자열의 길이를 반환한다.

```shell
$ terraform console
> length(["foo","bar"])
2
> length({"foo"=1,"bar"=2})
2
> length("foo bar")
7
```

---

### list

- Terraform v0.12에서 삭제되어 이후 버전에서는 `tolist`를 대신 사용해야 한다.
- 본 문서는 `v1.4.x` 버전 기준이므로 예시를 생략한다.

---

### lookup

- 주어진 키로 맵에서 단일 요소의 값을 검색한다.
- 키가 존재하지 않으면 주어진 기본값이 대신 반환된다.
- `lookup(${맵}, ${키}, ${기본값})` 형식으로 사용한다.

```shell
$ terraform console
> lookup({"foo"=1, "bar"=2}, "foo", "default value")
1
> lookup({"foo"=1, "bar"=2}, "poo", "default value")
"default value"
```

---

### map

- Terraform v0.12에서 삭제되어 이후 버전에서는 `tomap`을 대신 사용해야 한다.
- 본 문서는 `v1.4.x` 버전 기준이므로 예시를 생략한다.

---

### matchkeys

- 다른 컬렉션에 있는 값의 해당 인덱스와 일치하는 컬렉션에서 요소의 하위 집합을 가져와서 새로운 컬렉션을 구성한다.
- `matchkeys(${값 컬렉션}, ${키 컬렉션}, ${대상 키 셋})`

```shell
$ terraform console
> matchkeys(["value1","value2","value3"],["key1","key2","key3"],["key1","key2"])
tolist([
  "value1",
  "value2",
])
```

- 결과의 순서가 중요하지 않은 경우 `for`를 사용하여 유사한 결과를 얻을 수 있다.

```shell
$ terraform console
> [for i, z in {"key1" = "value1", "key2" = "value2", "key3" = "value3"}: i if z == "value1" || z == "value2"]
[
  "key1",
  "key2",
]
```

---

### merge

- 임의의 갯수의 맵 또는 객체에서 모든 인수를 병합하고 단일 맵 또는 객체를 반환한다.
- 중복되는 인수가 있는 경우 나중에 입력된 인수가 높은 우선순위를 가져간다.

```shell
$ terraform console
> merge({"foo" = 1, "bar" = 2}, {"foo" = 3, "var" = 4})
{
  "bar" = 2
  "foo" = 3
  "var" = 4
}
> merge({"foo" = 1, "bar" = 2}, {"foo" = [1, 2]}, {"var" = 3})
{
  "bar" = 2
  "foo" = [
    1,
    2,
  ]
  "var" = 3
}
```

---

### one

- 컬렉션이 비어 있는 경우는 `null`, 컬렉션의 크기가 1인 경우 첫번째 인수를 반환한다.
- 컬렉션의 크기가 2이상인 경우에는 오류가 발생한다.

```shell
$ terraform console
> one([])
null
> one(["foo"])
"foo"
> one(["foo", "bar"])
╷
│ Error: Invalid function argument
│ 
│   on <console-input> line 1:
│   (source code not available)
│ 
│ Invalid value for "list" parameter: must be a list, set, or tuple value with either zero or one elements.
╵
```

---

### range

- 시작값, 한계값, 단계값을 사용하여 숫자 목록을 생성한다.
- `range(${한계값})`, `range(${시작값}, ${한계값})`, `range(${시작값}, ${한계값}, ${단계값})` 형식으로 사용한다.
- 한계값은 목록에 포함되지 않는다.

```shell
$ terraform console
> range(3)
tolist([
  0,
  1,
  2,
])
> range(1, 3)
tolist([
  1,
  2,
])
> range(1, 10, 3)
tolist([
  1,
  4,
  7,
])
```

---

### reverse

- 주어진 컬렉션을 역순으로 뒤집는다.

```shell
$ terraform console
> reverse(["foo", "bar"])
[
  "bar",
  "foo",
]
```

---

### setintersection

- 복수의 셋에서 모든 Set이 공통적으로 가지고 있는 요소를 가지고 새로운 Set을 생성한다.

```shell
$ terraform console
> setintersection(["a", 1, 2], ["b", 1, 3], ["c", 1, 4])
toset([
  "1",
])
> setintersection(["a", 1, 2], ["b", 1, 2], ["c", 1, 2])
toset([
  "1",
  "2",
])
```

---

### setproduct

- "데카르트 곱"을 계산하여 주어진 모든 Set에서 가능한 모든 조합을 출력한다.

```shell
$ terraform console
> setproduct(["a", "b"], [1, 2])
tolist([
  [
    "a",
    1,
  ],
  [
    "a",
    2,
  ],
  [
    "b",
    1,
  ],
  [
    "b",
    2,
  ],
])
```

---

### setsubtract

- 첫번째 Set의 요소 중에서 두번째 Set에 없는 요소만 추출한다.

```shell
$ terraform console
> setsubtract(["a", "b", "c"], ["a", "b"])
toset([
  "c",
])
```

---

### setunion

- 여러 Set의 모든 요소를 포함하는 단일 Set을 생성한다.

```shell
$ terraform console
> setunion(["a", "b"], ["a", "c"], ["a", "d"])
toset([
  "a",
  "b",
  "c",
  "d",
])
```

---

### slice

- 컬렉션 내에서 일부를 추출한다.
- `slice(${컬렉션}, ${시작인덱스}, ${종료인덱스})` 형식으로 사용한다.

```shell
$ terraform console
> slice(["f", "o", "o", "b", "a", "r"], 3, 6)
[
  "b",
  "a",
  "r",
]
```

---

### sort

- 문자열 컬렉션을 오름차순으로 정렬한다.

```shell
$ terraform console
> sort(["c", "a", "b"])
tolist([
  "a",
  "b",
  "c",
])
> sort([10, 100, 1])
tolist([
  "1",
  "10",
  "100",
])
```

---

### sum

- 컬렉션 또는 숫자 집합에서 인수들의 합을 반환한다.

```shell
$ terraform console
> sum([1, 1.1, 3])
5.1
```

---

### transpose

- 맵에서 키와 값을 교체한다.

```shell
$ terraform console
> transpose({"foo" = [1, 2], "bar" = [3, 4]})
tomap({
  "1" = tolist([
    "foo",
  ])
  "2" = tolist([
    "foo",
  ])
  "3" = tolist([
    "bar",
  ])
  "4" = tolist([
    "bar",
  ])
})
```

---

### values

- 맵에서 요소의 값을 목록으로 반환한다.

```shell
$ terraform console
> values({1 = "foo", 2 = "bar", 3 = "var"})
[
  "foo",
  "bar",
  "var",
]
```

---

### zipmap

- 키 목록과 값 목록을 하나의 맵으로 변환한다.

```shell
$ terraform console
> zipmap(["key1", "key2"], ["value1", "value2"])
{
  "key1" = "value1"
  "key2" = "value2"
}
> zipmap(["key1", "key2"], ["value1", "value2", "value3"])
╷
│ Error: Error in function call
│ 
│   on <console-input> line 1:
│   (source code not available)
│ 
│ Call to function "zipmap" failed: number of keys (2) does not match number of values (3).
╵
```

---

### 참고한 자료

- [alltrue](https://developer.hashicorp.com/terraform/language/functions/alltrue)
- [anytrue](https://developer.hashicorp.com/terraform/language/functions/anytrue)
- [chunklist](https://developer.hashicorp.com/terraform/language/functions/chunklist)
- [coalesce](https://developer.hashicorp.com/terraform/language/functions/coalesce)
- [coalescelist](https://developer.hashicorp.com/terraform/language/functions/coalescelist)
- [compact](https://developer.hashicorp.com/terraform/language/functions/compact)
- [concat](https://developer.hashicorp.com/terraform/language/functions/concat)
- [contains](https://developer.hashicorp.com/terraform/language/functions/contains)
- [distinct](https://developer.hashicorp.com/terraform/language/functions/distinct)
- [element](https://developer.hashicorp.com/terraform/language/functions/element)
- [flatten](https://developer.hashicorp.com/terraform/language/functions/flatten)
- [index](https://developer.hashicorp.com/terraform/language/functions/index)
- [keys](https://developer.hashicorp.com/terraform/language/functions/keys)
- [length](https://developer.hashicorp.com/terraform/language/functions/length)
- [list](https://developer.hashicorp.com/terraform/language/functions/list)
- [lookup](https://developer.hashicorp.com/terraform/language/functions/lookup)
- [map](https://developer.hashicorp.com/terraform/language/functions/map)
- [matchkeys](https://developer.hashicorp.com/terraform/language/functions/matchkeys)
- [merge](https://developer.hashicorp.com/terraform/language/functions/merge)
- [one](https://developer.hashicorp.com/terraform/language/functions/one)
- [range](https://developer.hashicorp.com/terraform/language/functions/range)
- [reverse](https://developer.hashicorp.com/terraform/language/functions/reverse)
- [setintersection](https://developer.hashicorp.com/terraform/language/functions/setintersection)
- [setproduct](https://developer.hashicorp.com/terraform/language/functions/setproduct)
- [setsubtract](https://developer.hashicorp.com/terraform/language/functions/setsubtract)
- [setunion](https://developer.hashicorp.com/terraform/language/functions/setunion)
- [slice](https://developer.hashicorp.com/terraform/language/functions/slice)
- [sort](https://developer.hashicorp.com/terraform/language/functions/sort)
- [sum](https://developer.hashicorp.com/terraform/language/functions/sum)
- [transpose](https://developer.hashicorp.com/terraform/language/functions/transpose)
- [values](https://developer.hashicorp.com/terraform/language/functions/values)
- [zipmap](https://developer.hashicorp.com/terraform/language/functions/zipmap)