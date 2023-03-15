## Terraform File System Functions 정리

**Terraform** `v1.4.x` 버전을 기준으로 파일 시스템과 관련된 기능을 정리한다.

---

### abspath

- 파일 시스템 경로를 포함하는 문자열을 가져와 절대 경로로 변환한다.
- 즉, 경로가 절대 경로가 아닌 경우 현재 작업 디렉토리와 결합된다.

```shell
$ terraform console
> path.root
"."
> abspath(path.root)
"/Users/???/???/???/project/roy/terraform-tutorial"
```

---

### dirname

- 파일 시스템 경로를 포함하는 문자열에서 마지막 부분을 제거하고 디렉토리의 경로만 추출한다.

```shell
$ terraform console
> dirname("foo/poo/bar/var.txt")
"foo/poo/bar"
```

---

### pathexpand

- `~` 세그먼트로 시작할 수 있는 파일 시스템 경로를 사용하고, 만약 그렇다면 해당 세그먼트를 현재 사용자 홈 디렉토리 경로로 변경한다.

```shell
$ terraform console
> pathexpand("~/foo/bar")
"/Users/???/foo/bar"
> pathexpand("/foo/bar")
"/foo/bar"
```

---

### basename

- 파일 시스템 경로를 포함하는 문자열에서 마지막 부분을 제외한 모든 항목을 제거한다.
  즉, 파일명을 포함하는 정상적인 경로인 경우 파일명을 제외한 모든 경로를 제거한다.

```shell
$ terraform console
> basename("foo/bar/var.txt")
"var.txt"
> basename("foo/bar")
"bar"
```

---

### file

- 주어진 경로에서 파일의 내용을 읽고 문자열로 변환한다.

```shell
$ terraform console
> file("${abspath(path.root)}/foobar.txt")
"Foo Poo Bar Var"
```

---

### fileexists

- 주어진 경로에 파일이 존재하는지 여부를 반환한다.

```shell
$ terraform console
> fileexists("${abspath(path.root)}/foobar.txt")
true
> fileexists("${abspath(path.root)}/foobar.txt") ? file("${abspath(path.root)}/foobar.txt") : "파일없음"
"Foo Poo Bar Var"
> fileexists("${abspath(path.root)}/foobars.txt")
false
> fileexists("${abspath(path.root)}/foobars.txt") ? file("${abspath(path.root)}/foobars.txt") : "파일없음"
"파일없음"
```

---

### fileset

- 지정된 경로에 위치하는 파일 중에서 파일의 이름이 지정된 패턴에 일치하는 파일 목록을 반환한다.

```shell
$ terraform console
> fileset("${abspath(path.module)}", "*.md")
toset([
  "collection-functions.md",
  "encoding-functions.md",
  "file-system-functions.md",
  "numeric-functions.md",
  "string-functions.md",
])
> fileset("${abspath(path.module)}", "file-system-*.md")
toset([
  "file-system-functions.md",
])
```

---

### filebase64

- 주어진 경로에서 파일의 내용을 읽고 Base64 인코딩된 문자열을 반환한다.

```shell
$ terraform console
> filebase64("${abspath(path.module)}/foobar.txt")
"Rm9vIFBvbyBCYXIgVmFy"
```

---

### templatefile

- 지정된 경로에서 파일을 읽고 제공된 템플릿 변수 집합을 사용하여 해당 콘텐츠를 템플릿으로 렌더링한다.
- `templatefile(${경로}, ${변수})` 형식으로 사용한다.

---

### 참고한 자료

- [abspath](https://developer.hashicorp.com/terraform/language/functions/abspath)
- [dirname](https://developer.hashicorp.com/terraform/language/functions/dirname)
- [pathexpand](https://developer.hashicorp.com/terraform/language/functions/pathexpand)
- [basename](https://developer.hashicorp.com/terraform/language/functions/basename)
- [file](https://developer.hashicorp.com/terraform/language/functions/file)
- [fileexists](https://developer.hashicorp.com/terraform/language/functions/fileexists)
- [fileset](https://developer.hashicorp.com/terraform/language/functions/fileset)
- [filebase64](https://developer.hashicorp.com/terraform/language/functions/filebase64)
- [templatefile](https://developer.hashicorp.com/terraform/language/functions/templatefile)