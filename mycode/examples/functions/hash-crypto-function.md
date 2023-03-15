## Terraform Hash & Crypto Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 해시 및 암호화와 관련된 기능을 정리한다.

---

### base64sha256, base64sha512

- `base64sha256`: 주어진 문자열의 SHA256 해시를 계산하고 Base64 인코딩한다. 
  `base64encode(sha256("foo"))`는 16진수 표현을 반환하기 때문에 `sha256("foo")`와 동일하지 않다.
- `base64sha512`: 주어진 문자열의 SHA512 해시를 계산하고 Base64 인코딩한다.
  `base64encode(sha512("foo"))`는 16진수 표현을 반환하기 때문에 `sha512("foo")`와 동일하지 않다.

```shell
$ terraform console
> base64sha256("foo bar")
"+8Gp+Fjqnhd5FpZL2Iw9N7kaHoRBJ2XimVB3fyZcS3U="
> sha256("foo bar")
"fbc1a9f858ea9e177916964bd88c3d37b91a1e84412765e29950777f265c4b75"

> base64sha512("foo bar")
"ZQGShiIqzkGPdCVWNm+bnaWq9nl1J9Lwy6W/5rL47SR0ZUKg8r4dqNY8JHf2iLYI61NiiZOvpiTzeLA/EAkM5w=="
> sha512("foo bar")
"65019286222ace418f742556366f9b9da5aaf6797527d2f0cba5bfe6b2f8ed24746542a0f2be1da8d63c2477f688b608eb53628993afa624f378b03f10090ce7"
```

---

### bcrypt

- Blowfish 암호를 사용하여 주어진 문자열의 해시를 계산하고 일반적으로 많은 Unix 시스템의 섀도우 암호 파일에서 예상되는 Modular Crypt 형식의 문자열을 반환한다.
- `bcrypt(${문자열}, ${cost}`형식으로 사용하며 `cost`는 선택적으로 사용되며 기본값은 10이다.

```shell
$ terraform console
> bcrypt("foo bar")
"$2a$10$lzS3chMktWwsIRRBq2nDQ.GPKHwPcREqFkOCSUU8q2PTpm0rias3O"
> bcrypt("foo bar", 10)
"$2a$10$6enUhwRSN4fnMI3clDBxd.Khj9uAf4w9BMosmjQX2TQhJUQzmARDu"
```

---

### filebase64sha256, filebase64sha512

- `filebase64sha256`: 리터럴 문자열이 아닌 주어진 파일의 내용을 SHA256 해싱하여 계산하고 Base64 인코딩한다.
- `filebase64sha512`: 리터럴 문자열이 아닌 주어진 파일의 내용을 SHA512 해싱하여 계산하고 Base64 인코딩한다.

```shell
$ terraform console
> filebase64sha256("${abspath(path.module)}/foobar.txt")
"c+e+3GkTQ1YJlx/d6SpMTKLYPEnVnRGN4C1q4BlsJMI="
> filebase64sha512("${abspath(path.module)}/foobar.txt")
"kt8bIt00WUv05R0wr11HeUhPtmzFLqxQecjP9cevYXMB7Ap/mUIqoj9gk+Skv7IPnpqMQ0jx9J3nf9Omv+q9uw=="
```

---

### filemd5, filesha1, filesha256, filesha512

- `filemd5`: 리터럴 문자열이 아닌 주어진 파일의 내용을 해시하는 `md5`의 변형이다.
- `filesha1`: 리터럴 문자열이 아닌 주어진 파일의 내용을 해시하는 `sha1`의 변형이다.
- `filesha256`: 리터럴 문자열이 아닌 주어진 파일의 내용을 해시하는 `sha256`의 변형이다.
- `filesha512`: 리터럴 문자열이 아닌 주어진 파일의 내용을 해시하는 `sha512`의 변형이다.

```shell
$ terraform console
> filemd5("${abspath(path.module)}/foobar.txt")
"4cf9a5af11b93fe85d43bcf6e85f968a"
> filesha1("${abspath(path.module)}/foobar.txt")
"18b65fde99cdfb2054d816828a79fb3d01884349"
> filesha256("${abspath(path.module)}/foobar.txt")
"73e7bedc6913435609971fdde92a4c4ca2d83c49d59d118de02d6ae0196c24c2"
> filesha512("${abspath(path.module)}/foobar.txt")
"92df1b22dd34594bf4e51d30af5d4779484fb66cc52eac5079c8cff5c7af617301ec0a7f99422aa23f6093e4a4bfb20f9e9a8c4348f1f49de77fd3a6bfeabdbb"
```

---

### md5

- 주어진 문자열의 MD5 해시를 계산하고 16진수로 인코딩한다.

```shell
$ terraform console
> md5("foo bar")
"327b6f07435811239bc47e1544353273"
```

---

### rsadecrypt

- RSA로 암호화된 암호문을 해독하여 일반 텍스트를 반환한다.
- `rsadecrypt(${암호화된 텍스트}, ${암호키})` 형식으로 사용한다.

---

### sha1, sha256, sha512

- `sha1`: 주어진 문자열의 SHA1 해시를 계산하고 16진수로 인코딩한다.
- `sha256`: 주어진 문자열의 SHA256 해시를 계산하고 16진수로 인코딩한다.
- `sha512`: 주어진 문자열의 SHA512 해시를 계산하고 16진수로 인코딩한다.

```shell
$ terraform console
> sha1("foo bar")
"3773dea65156909838fa6c22825cafe090ff8030"
> sha256("foo bar")
"fbc1a9f858ea9e177916964bd88c3d37b91a1e84412765e29950777f265c4b75"
> sha512("foo bar")
"65019286222ace418f742556366f9b9da5aaf6797527d2f0cba5bfe6b2f8ed24746542a0f2be1da8d63c2477f688b608eb53628993afa624f378b03f10090ce7"
```

---

### uuid, uuidv5

- `uuid`: 고유 식별자 문자열을 생성한다. (버전 4, RFC 4122)
- `uuidv5`: 고유 식별자 문자열을 생성한다. (버전 5, RFC 4122)
  `uuidv5(${네임스페이스}, ${이름})`형식으로 사용한다.

```shell
$ terraform console
> uuid()
"bd6b222b-abc6-2ac9-5a97-56b6de304da6"
> uuidv5("dns", "www.foobar.co.kr")
"e2e46518-3da7-52b6-b6cb-a7b742b2182c"
```

---

### 참고한 자료

- [base64sha256](https://developer.hashicorp.com/terraform/language/functions/base64sha256)
- [base64sha512](https://developer.hashicorp.com/terraform/language/functions/base64sha512)
- [bcrypt](https://developer.hashicorp.com/terraform/language/functions/bcrypt)
- [filebase64sha256](https://developer.hashicorp.com/terraform/language/functions/filebase64sha256)
- [filebase64sha512](https://developer.hashicorp.com/terraform/language/functions/filebase64sha512)
- [filemd5](https://developer.hashicorp.com/terraform/language/functions/filemd5)
- [filesha1](https://developer.hashicorp.com/terraform/language/functions/filesha1)
- [filesha256](https://developer.hashicorp.com/terraform/language/functions/filesha256)
- [filesha512](https://developer.hashicorp.com/terraform/language/functions/filesha512)
- [md5](https://developer.hashicorp.com/terraform/language/functions/md5)
- [rsadecrypt](https://developer.hashicorp.com/terraform/language/functions/rsadecrypt)
- [sha1](https://developer.hashicorp.com/terraform/language/functions/sha1)
- [sha256](https://developer.hashicorp.com/terraform/language/functions/sha256)
- [sha512](https://developer.hashicorp.com/terraform/language/functions/sha512)
- [uuid](https://developer.hashicorp.com/terraform/language/functions/uuid)
- [uuidv5](https://developer.hashicorp.com/terraform/language/functions/uuidv5)