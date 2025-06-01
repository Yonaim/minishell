# minishell

![screenshot](/screenshot.png)

- "yonazang & meyun's 금쪽이(Gold Baby Shell)"
- GNU의 Bash를 '과제가 요구하는 만큼만' 최대한 비슷하게 구현합니다. (Bash와 완벽하게 같지 않습니다)

## Official Documents
- POSIX Shell & Utilities: Detailed ToC (IEEE)
	-> https://pubs.opengroup.org/onlinepubs/9699919799/utilities/contents.html
- Bash Reference Manual (GNU)
	-> https://www.gnu.org/software/bash/manual/bash.html

## Flow

1. Input
	- Readline
2. Parsing
	- Lexical analysis (=Tokenizing)
	- Syntax analysis (=Make Parse Tree)
3. Word Expansion
	- Parameter and Variable Expansions
	- Word Splitting
	- Pathname Expansions
	- Quote Removal
4. Command Execution
	- Redirection
	- Builtin Commands
	- Simple Command Execution

## 1. Input

### Readline
- Standard library의 readline(3)을 사용하여 손쉽게 구현.
- 개행 문자('\n')가 나올 때까지 입력 받고, 개행 문자를 제외한 문자열(Null-terminated string)을 반환한다.
- 사용 방법
	- brew install readline
	- 컴파일 옵션: -lreadline

## 2. Parsing

### Lexical analysis (=Tokenizing)
- Types of Token
  - WORD
  - OPERATOR
    - BRACE_LEFT((), BRACE_RIGHT()), LOGICAL_AND(&&), LOGICAL_OR(||), 
      PIPELINE(|), RDIR_IN(<), RDIR_OUT(>), RDIR_HEREDOC(<<), RDIR_APPEND(>>)
- split 기준
  - blank
  	: space, tab, (newline의 경우, 우리의 구현에서는 해당x)
  - operator (consist of non blank metacharacters)
  	: "(", ")", "|", "||", "&&", "<", ">", "<<", ">>"
- 참고 사항
	- 토큰 타입들은 이후 Syntax 분석에서 BNF의 symbol로 쓰임.
	- 우리의 쉘에서는 따옴표가 닫히지 않게 입력한 경우 따옴표가 닫힐 때까지 여러 줄(multi-line)을 입력 받지 않도록 할 것이므로, 이곳에서 unclosed quote를 syntax error로 간주하여 처리한다.

### Syntax analysis (=Make Parse Tree)

## 3. Word Expansion

### Parameter and Variable Expansions
- Positional Parameters: $N (N is digit)
- Special Parameters
	- ?: exit status of the most recent pipeline
	- \*: wild card
	- 이 밖에도 더 있지만 우리는 다루지 않음
- Environment Variables: ${VARIABLE_NAME}

### Word Splitting

### Pathname Expansions

### Quote Removal

## 4. Command Execution

### Redirection

### Builtin Commands

### Simple Command Execution
- Error case
	- No such file or directory(/포함 && 존재하지 x)
	- Permission denied(/포함 && 존재 && 권한 x)
	- No such file or directory(/포함 && 존재 && 파일x)
	- command not found(/가 없음 && 존재하지 x)

