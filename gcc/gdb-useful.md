# 有用的gdb调试

http://stackoverflow.com/questions/1471226/most-tricky-useful-commands-for-gdb-debugger

```text
backtrace full: Complete backtrace with local variables
up, down, frame: Move through frames
watch: Suspend the process when a certain condition is met
set print pretty on: Prints out prettily formatted C source code
set logging on: Log debugging session to show to others for support
set print array on: Pretty array printing
finish: Continue till end of function
enable and disable: Enable/disable breakpoints
tbreak: Break once, and then remove the breakpoint
where: Line number currently being executed
info locals: View all local variables
info args: View all function arguments
list: view source
rbreak: break on function matching regular expression
```