# Embedding Zenroom

Zenroom is designed to facilitate embedding into other native applications and high-level scripting languages. The stable releases distribute compiled library components for Apple/iOS and Google/Android platforms, as well MS/Windows DLL. Golang bindings and a Jupyter kernel are also in experimental phase.

To call Zenroom from an host program is very simple, since there isn't an API of calls, but a single call to execute scripts and return their results. The call is called `zenroom_exec` and prints results to the "stderr/stdout" terminal output. Its prototype is common to all libraries:

```c
int zenroom_exec(char *script, char *conf, char *keys,
                 char *data, int verbosity);
```
The input buffers are all read-only, here their functions:
- `script`: a long string containing the script to be executed
- `conf`: a short configuration string (for now only `umm` supported as value)
- `keys`: a string often JSON formatted that contains keys (sensitive information)
- `data`: a string (also JSON formatted) that contains data
- `verbosity`: a number from 1 to 3 activating more debugging messages

In addition to this function there is another one that copies results (error messages and printed output) inside memory buffers:
```c
int zenroom_exec_tobuf(char *script, char *conf, char *keys,
                       char *data, int verbosity,
                       char *stdout_buf, size_t stdout_len,
                       char *stderr_buf, size_t stderr_len);
```
In addition to the previously explained arguments, the new ones are:
- `stdout_buf`: pre-allocated buffer by the caller where to copy stdout
- `stdout_len`: maximum length of the pre-allocated stdout buffer
- `stderr_buf`: pre-allocated buffer by the called where to copy stderr
- `stderr_len`: maximum length of the pre-allocated stderr buffer

At last a third call is provided not to execute the script, but to obtain its JSON formatted Abstract Syntax Tree (AST) inside a provided buffer:
```c
int zenroom_parse_ast(char *script, int verbosity,
                      char *stdout_buf, size_t stdout_len,
                      char *stderr_buf, size_t stderr_len);
```

# Language bindings

-This API can be called in similar ways from a variety of languages and wrappers that already facilitate its usage.

## Javascript

[Javascript NPM package](https://www.npmjs.com/package/zenroom)

💾 Installation
```
npm install zenroom
```

🎮 Quick Usage

```javascript
import zenroom from 'zenroom'

const script = `print("Hello World!")`
zenroom.script(script).zenroom_exec()

// prints in the console.log "Hello World!"
```

## Python

[Python language bindings](https://pypi.org/project/zenroom/)

💾 Installation
```
pip install zenroom
```

🎮 Quick Usage

```python
from zenroom import zenroom

script = "print('Hello world!')"
result = zenroom.zenroom_exec(script)
print(result.stdout) # guess what
```

## Golang

[Go language bindings](https://godoc.org/github.com/DECODEproject/Zenroom/bindings/golang/zenroom)

💾 Installation
```
import "github.com/DECODEproject/Zenroom/bindings/golang/zenroom"
```

🎮 Quick Usage

```go
script := []byte(`print("Hello World!")`)
res, _ := zenroom.Exec(script)
fmt.Println(string(res))
```
