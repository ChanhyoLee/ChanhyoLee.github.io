# grep과 expression

- [grep과 expression](#grep--expression)
  * [grep의 용도](#grep----)
  * [grep의 친척들](#grep-----)
  * [grep이란 이름은 어디서 왔는가?](#grep---------------)
  * [man grep](#man-grep)
    + [SYNOPSIS](#synopsis)
    + [OPTIONS](#options)
  * [Regular Expressions](#regular-expressions)
    + [Metacharacters](#metacharacters)
    + [Character classes](#character-classes)
  * [사용 예시 및 FAQ](#--------faq)

## grep의 용도

`grep`은 파일 내에서 원하는 문자열 패턴을 찾기 위해 사용합니다. 패턴들은 New line으로 구분된 하나 이상의 패턴이며, 패턴에 일치하는 각 line들을 출력합니다. shell 커맨드로 grep이 사용될 때에는 패턴들을 따옴표로 묶는 것이 좋습니다.

## grep의 친척들

`grep`과 유사한 `egrep`, `fgrep`, `rgrep` 등은 각각 `grep -E`, `grep -F`, `grep -r` 과 같은 의미를 가지고 있습니다.

## grep이란 이름은 어디서 왔는가?

`grep`의 이름에 대한 이해를 하기 위해서는 먼저 `ed` (ee-dee) editor에 대한 이해가 필요합니다. 당시 `ed` 에디터에는 지금의 `vim`에서와 비슷하게, `/print/` 와 같은 Regular Expression을 통해 해당 문자열의 검색이 가능했었습니다. 그러던 중, 용량이 큰 텍스트 파일 (`The Federalist Papers`-미연방헌법을 지지하는 논문들)에 대한 작업이 필요했고, 초창기 시스템은 자원이 굉장히 한정적이었기에 (메모리 < 64Kbytes) 양이 많은 문서에 대해서 한 파일에 작성해서 메모리에 올린 뒤 작업하는 것이 불가능했습니다.

이 과정에서 여러 파일들 속의 문자열을 찾기 위한 요구가 생겼고, 이에 따라 `grep`이 만들어졌습니다. `ed` 에서는 `g/print/d` 와 같이 입력했을 때 문서 전체 (Global)에서 `print` 라는 문자열을 모두 삭제 (Delete)하라는 방식으로 명령어를 사용했었습니다. `grep`은 `g/re/p`의 형태로, Globally search for a Regular Expression and Print matching lines 라는 의미가 있습니다. 

참조 : https://www.youtube.com/watch?v=NTfOnGZUZDk

## man grep

### SYNOPSIS

grep의 시놉시스는 아래와 같습니다.

```
SYNOPSIS
       grep [OPTION...] PATTERNS [FILE...]
       grep [OPTION...] -e PATTERNS ... [FILE...]
       grep [OPTION...] -f PATTERN_FILE ... [FILE...]
```

기본적으로 명령어, 옵션, 찾고자하는 패턴, 탐색할 대상 파일 형식으로 사용합니다.

FILE이 비어 있거나 `-` 인 경우 표준 입력으로 입력을 받습니다.

### OPTIONS

| 영역                         | 옵션      | 인자                  | 설명                                                         |
| ---------------------------- | --------- | --------------------- | ------------------------------------------------------------ |
| Generic Program Information  | `-V`      | -                     | `grep`의 버전을 출력                                         |
| Pattern Syntax               | `-E`      | -                     | 주어진 패턴을 Extended Regular Expressions로 해석            |
|                              | `-F`      | -                     | 주어진 패턴을 Fixed Strings으로 해석                         |
|                              | `-G`      | -                     | 주어진 패턴을 Basic Regular Expressions로 해석               |
|                              | `-P`      | -                     | 주어진 패턴을 Perl-compatible Regular Expressions로 해석     |
| Matching Control             | `-e`      | `PATTERNS`            | 주어진 패턴을 사용. 여러 번 입력하는 경우 여러 패턴에 대해 탐색 가능. "-"로 시작하는 패턴의 경우 옵션으로 오인식되는 경우를 방지 가능 |
|                              | `-f`      | `FILE`                | `FILE`에 포함된 패턴을 사용. `-e` 옵션과 같이 사용하여 주어진 모든 패턴에 대해 탐색할 수 있음 |
|                              | `-i`      | -                     | Ignore Case. 디폴트로는 Case Sensitive함                     |
|                              | `-v`      | -                     | Invert Match. 해당 문자열이 포함된 줄을 출력하지 않음        |
|                              | `-w`      | -                     | 주어진 패턴과 정확히 일치하는 단어가 포함된 줄을 출력        |
|                              | `-x`      | -                     | 주어진 패턴과 정확히 줄 전체가 일치할 때 출력                |
| General Output Control       | `-c`      | -                     | 일반 출력 대신 count를 출력. `-v`와 같이 사용하는 경우 일치하지 않는 줄의 수를 출력 |
|                              | `--color` | `never, auto, always` | 일치한 패턴을 터미널에 출력할 때 컬러를 지정하는 옵션으로, `GREP_COLORS` 환경변수에 지정된 색으로 출력 |
|                              | `-l`      | -                     | 해당 패턴을 포함하는 파일의 이름들을 출력                    |
|                              | `-L`      | -                     | 해당 패턴을 포함하지 않는 파일의 이름들을 출력               |
|                              | `-m`      | `COUNT`               | `COUNT` 수만큼의 Matching Line이 탐색되면 종료               |
|                              | `-o`      | -                     | 일치하는 부분만 출력                                         |
| Output Line Prefix Control   | `-n`      |                       | 파일에서의 Line number를 같이 출력                           |
| Context Line Control         | `-A`      | `NUM`                 | 탐색 문자열이 포함된 해당 줄로부터 `NUM` 아랫줄 포함하여 출력 |
|                              | `-B`      | `NUM`                 | 탐색 문자열이 포함된 해당 줄로부터 `NUM` 윗줄 포함하여 출력  |
|                              | `-C`      | `NUM`                 | 탐색 문자열이 포함된 해당 줄로부터 `NUM` 위/아래줄 포함하여 출력 |
| File and Directory Selection | `-d`      | `read, skip, recurse` | 입력 파일이 디렉토리인 경우, `ACTION`에 지정된 항목에 따라 동작이 변화. `read`는 일반 파일일 경우 탐색, `skip`은 디렉토리 skip, `recurse`는 디렉토리 하위 모든 파일들을 탐색. |

## Regular Expressions

### Metacharacters

`BRE` 는 Basic Regular Expressions, `ERE`는 Extended Regular Expressions를 말합니다.

| Metacharacter               | Description                                                  |
| :-------------------------- | :----------------------------------------------------------- |
| `.`                         | 1개 Character와 매칭. 단 대괄호 내에서는 문자  `.`으로 해석. (ex. `[a.c]`는 `a`, `.`, `c` 셋 중 하나로 봄 ) |
| `[ ]`                       | 대괄호 표현식. `-`를 통해 범위를 지정할 수 있음 (`[a-cx-z]`는 `a`,`b`,`c`,`x`,`y`,`z` 중 하나로 봄. 단, `[-ab]`, `[ab-]` 와 같이 맨 앞, 뒤에 위치한 경우에는 문자 `-`로 해석하며, `[]abc]`와 같이 대활고 표현식을 맨 앞에 두어 포함시킬 수도 있음 |
| `[^ ]`                      | 대괄호 속에 포함된 문자열이 포함되지 않는 문자열을 매칭. (ex. `[^abc]`는 `a`,`b`,`c` 이외의 문자들에 대해 매칭됨) |
| `^`                         | 문자열 내 시작 위치 매칭. Line Based 도구에서는 모든 줄의 시작 위치와 일치 |
| `$`                         | 문자열의 끝 위치 또는 문자열 끝 개행 직전의 위치를 매칭. Line Based 도구에서는 모든 줄의 끝 위치와 일치 |
| BRE: `\( \)` ERE: `( )`     | 블록 또는 캡처 그룹이라고 불리는 하위 표현식을 정의 (ex. `abc{2}` $\rightarrow$  `abcc`, `(abc){2}` $\rightarrow$ `abcabc`) |
| `*`                         | 앞의 요소와 0회 이상 일치하는지 비교. (ex. `ab*c`는 `ac`,  `abc`, `abbbc` 등을 만족. `[xyz]*`는 ` `, `x`, `y`, `z`, `zx`, `zyx`, `xyzzy` 등을 만족.) |
| BRE: `\+` ERE: `+`          | 앞의 요소가 1번 이상 일치하는지 비교. (ex. `ab*c`는 `abc`, `abbbc` 등과는 일치하지만, `ac`와는 일치하지 않음) |
| BRE: `\?` ERE: `?`          | 앞의 요소가 1번 또는 0번 일치하는지 비교. (ex. `ab?c`는 `ac`, `abc`와 일치하고, `(ab)?`는 ` `또는 `ab`와 일치) |
| BRE: `\|` ERE: `|`          | 앞의 요소 혹은 뒤의 요소와 일치하는지 비교. (ex. `abc|def`는 `abc` 또는 `def` 문자열과 일치) |
| BRE: `\{m,n\}` ERE: `{m,n}` | 앞의 요소가 최소 m번 최대 n번 일치하는지 비교.               |
| BRE: `\{m\}` ERE: `{m}`     | 앞의 요소가 정확히 m번 반복되는지 비교.                      |
| BRE: `\{m,\}` ERE: `{m,}`   | 앞의 요소가 최소 m번 반복되는지 비교.                        |
| BRE: `\{,n\}` ERE: `{,n}`   | 앞의 요소가 최대 n번 반복되는지 비교.                        |

### Character classes

| POSIX class  | similar to             | meaning                                  |
| :----------- | :--------------------- | :--------------------------------------- |
| `[:upper:]`  | `[A-Z]`                | 대문자 알파벳                            |
| `[:lower:]`  | `[a-z]`                | 소문자 알파벳                            |
| `[:alpha:]`  | `[[:upper:][:lower:]]` | 대문자, 소문자를 포함한 모든 알파벳 문자 |
| `[:alnum:]`  | `[[:alpha:][:digit:]]` | 숫자, 알파벳 문자                        |
| `[:digit:]`  | `[0-9]`                | 숫자                                     |
| `[:xdigit:]` | `[0-9A-Fa-f]`          | 16진수 숫자                              |
| `[:punct:]`  | `[.,!?:…]`             | 문장부호                                 |
| `[:blank:]`  | `[ \t]`                | 스페이스와 탭 문자만                     |
| `[:space:]`  | `[ \t\n\r\f\v]`        | Blank (whitespace) 문자                  |
| `[:cntrl:]`  |                        | Control 문자                             |
| `[:graph:]`  | `[^ \t\n\r\f\v]`       | 출력된 문자                              |
| `[:print:]`  | `[^\t\n\r\f\v]`        | 출력된 문자와 스페이스                   |

## 사용 예시 및 FAQ

-   특정 패턴을 가진 파일의 이름을 추출하는 방법?
    -   `grep -l 'main' test-*.c`

-   디렉토리 Recursive 탐색
    -   `grep -r 'hello' /home/gigi`
    -   `grep -r --include='*.c' 'hello' /home/gigi`
-   Leading `-`로 인한 버그를 방지하는 방법
    -   `grep -e '--cut here--' ./*`
-   일부분만 일치하는 단어가 아닌, 전체 단어가 일치하는 경우만 탐색
    -   `grep -w 'hello' test*.log`
    -   `grep 'hello\>' test*.log`
        -   이 경우에는 `hello`로 끝나는 단어가 있다면 포함됩니다 (ex. `Othello`)
-   주변 컨텍스트를 출력하고 싶을 때
    -   `grep -C 2 'hello' test*.log`
-   파일 이름 출력 강제
    -   `grep 'eli' /etc/passwd /dev/null`
    -   `grep -H 'eli' /etc/passwd`
-   `ps` 명령어 출력에 대해 Regular Expression을 적용하는 이유?
    -   `ps -ef | grep '[c]ron'`
    -   대괄호 없이 작성하면 `grep cron` 명령어 또한 같이 잡히게 되는데, 대괄호를 추가하면 무시할 수 있습니다.
-   `grep` 명령어가 "Binary file matches"를 출력하는 이유?
    -   binary file의 각 줄을 출력하더라도 유용한 정보일 확률이 낮습니다. 이를 제거하기 위해서는 `-I` 옵션을 주어야 합니다.
-   `grep -lv`가 일치하지 않는 파일 이름을 출력하지 않는 이유?
    -   해당 명령어는 특정 패턴와 맞지 않는 하나 이상의 줄을 포함하는 모든 파일의 이름을 출력하라는 의미입니다.
    -   원래의 목적을 위해서는 `-L` 옵션을 주어야 합니다.
-   `OR` 의 경우에는 `|`가 있는데, `AND`는?
    -   `AND`는 `grep`을 두 번 하여 얻을 수 있습니다.
    -   `grep 'paul' /etc/motd | grep 'franc,ois'`
-   입력 줄에 대해 비어있는 패턴이 매칭되는 이유?
    -   `grep` 명령어는 패턴과 일치하는 문자열이 포함된 줄을 탐색합니다. 모든 줄에는 빈 문자열이 포함되어 있으므로, empty 패턴을 사용하면 `grep`이 각 줄에서 일치하는 문자열을 찾게 됩니다. 
    -   `^`, `$`등 과 같은 패턴들이 이런 경우에 포함되며, empty line을 위해서는 `'^$'`를 사용하고, blank line을 위해서는 `'^[[:blank:]]*$'`을 사용합니다.
-   표준 입출력 파일에 대해 탐색?
    -   `-`를 사용합니다.
    -   `cat /etc/passwd | grep 'alain' - /etc/motd`
-   Back Referencing이 실패하는 이유?
    -   `echo 'ba' | grep -E '(a)\1|b\1'	`
    -   첫 번째 alternate `'(a)\1'`이 일치하지 않으므로, 출력이 제공되지 않습니다.
    -   `'aa'`가 없기 때문에 두 번째 Alternate `'\1'`은 다시 참조할 항목이 없으므로 일치할 수 없습니다.
-   `grep`, `fgrep`, `egrep`이란?
    -   `grep`은 리눅스에서 수행되던 line editing 방법에서 왔으며, `ed`가 global/regular expression/print 와 같이 수행하던 것에서 유래되었습니다.
    -   `fgrep`은 `Fixed grep`, `egrep`은 `Extended grep`을 의미합니다.
