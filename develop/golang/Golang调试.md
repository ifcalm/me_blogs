### golang debug 的一些技巧应用

- golang tools 是直接集成在语言工具里，支持内存分析，cpu分析，阻塞锁分析等
- delve，gdb 作为最常用的 debug 工具，让你能够更深入的进入程序调试


### Golang tools

golang 从语言原生层面就集成了大量的实用工具，安装好 golang 之后，执行 `go tool` 就能看到内置支持的所有工具了

```
$ go tool
addr2line
api
asm
buildid
cgo
compile
cover
dist
doc
fix
go_bootstrap
link
nm
objdump
oldlink
pack
pprof
test2json
trace
vet
```

