## Terraform Date & Time Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 날짜 및 시간과 관련된 기능을 정리한다.

---

### formatdate

- 타임스탬프를 다른 시간 형식으로 변환한다.
- 형식에 대한 자세한 사양은 [공식문서](https://developer.hashicorp.com/terraform/language/functions/formatdate)를 참고한다.

```shell
$ terraform console
> formatdate("YYYY-MM-DD HH:mm:ss", "2023-01-01T00:00:00Z")
"2023-01-01 12:00:00"
> formatdate("MMM DD, YYYY", "2023-01-01T00:00:00Z")
"Jan 01, 2023"
```

---

### timeadd

- 타임스탬프에 원하는 기간을 추가한다.
- 원하는 기간을 빼고 싶은 경우 음수 형태를 사용한다.

```shell
$ terraform console
> timeadd("2023-01-01T00:30:00Z", "10m")
"2023-01-01T00:40:00Z"
> timeadd("2023-01-01T00:30:00Z", "-10m")
"2023-01-01T00:20:00Z"
```

---

### timecmp

- 두 개의 타임스탬프를 비교하고 결과를 반환한다.
- `timecmp(timestamp_a, timestamp_b)` 형식으로 사용한다.
- `timestamp_a`보다 `timestamp_b`가 이전이라면 `-1`을 반환한다.
- `timestamp_a`와 `timestamp_b`가 동일한 시간이라면 `0`을 반환한다.
- `tempstamp_a`보다 `timestamp_b`가 이후라면 `1`을 반환한다.

```shell
$ terraform console
> timecmp("2022-01-01T00:00:00Z", "2023-01-01T00:00:00Z")
-1
> timecmp("2023-01-01T00:00:00Z", "2023-01-01T00:00:00Z")
0
> timecmp("2023-01-01T00:00:00Z", "2022-01-01T00:00:00Z")
1
> timecmp("2023-01-01T00:00:00Z", "2023-01-01T01:00:00+01:00")
0
```

---

### timestamp

- RFC 3339 형식의 UTC 타임스탬프 문자열을 반환한다.

```shell
$ terraform console
> timestamp()
"2023-03-15T09:25:04Z"
```

---

### 참고한 자료

- [formatdate](https://developer.hashicorp.com/terraform/language/functions/formatdate)
- [timeadd](https://developer.hashicorp.com/terraform/language/functions/timeadd)
- [timecmp](https://developer.hashicorp.com/terraform/language/functions/timecmp)
- [timestamp](https://developer.hashicorp.com/terraform/language/functions/timestamp)