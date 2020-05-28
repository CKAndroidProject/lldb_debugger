# LLDB Commands
## Great cheat sheets
https://gist.github.com/alanzeino/82713016fd6229ea43a8
##### Brief list of attached Libraries
`image list -b`
##### Sections of all loaded code
`image dump sections`
##### Sections of a Module
`image dump sections myApp`
##### Symbols of a Module
`image dump symtab myApp`
##### Symbols of all loaded code (BAD IDEA)
`image dump symtab`
##### Lookup options:
`help image lookup`
##### Lookup a Debug Symbol
`image lookup -r -n YDClass`
##### Lookup non-debug symbols:
`image lookup -r -s YDClass`
##### Lookup Address:
`image lookup -a 0x1000016a0`

##### Print text
`script print "Hello"`
##### Registers (x86_64)
Argument  | Register | Alias  
--|---|--
Return  | RAX  | $rax
First  | RDI  | $arg1
Second  | RSI  | $arg2
Third  |  RDX |  $arg3
Fourth  | RCX  | $arg4  
Fifth  | R8  |  $arg5
Sixth  | R9  |  $arg6

Usage: `po $arg2`

##### Create NSString
`exp NSString *$myMethod = NSStringFromSelector(_cmd)`
##### Get Selector
`po NSSelectorFromString($meth)`

## Breakpoints
##### Delete all breakpoints
`b delete`
##### List
`b list`
##### Breakpoint on Selector
`breakpoint set --selector URLSession:didReceiveChallenge:completionHandler:`
##### Breakpoint on Selector in Module
`breakpoint set --selector URLSession:didReceiveChallenge:completionHandler: -s playModule`
##### Break on exact ObjC Method
`b "-[MyUser name:]"`
##### Breakpoint on completionHandler
`b -[YDURLSessionDel URLSession:didReceiveChallenge:completionHandler:]`
#####  Regex Breakpoint
`rb '\-\[UIViewController\ '`
`rb '\-\[YDUser(\(\w+\))?\ '`


##### Launch
`lldb attach -p ps x|grep -i -m1 sample|awk '{print $1}'` // 'sample' is the app name
##### Import lldb script
`command source <file_path>/lldb_script.txt`
##### Import Python script
`command script import <file_path>/lldb_python.py`
##### Lookup
```
// This works on a stripped, release app...
(lldb) lookup -X (?i)address -m my_app
```
##### Custom prompt
```
// instead of the vanilla (lldb)
settings set prompt \-\>
```
##### lldb iOS Simulators
Avoid using xCode if you are using the Python Debugger:
```
- Kill xcode
- Run iOS app in the simulator
- run a `ps -ax` to find your PID
- `$ lldb -p <PID>`
```
##### STDOUT
If you use `lldb --wait-for` or `lldb -attach` you are attaching **after** a decision on where to send `stdout` was made.  For example:

```
// NSLog only sent to Console.app when you attach

./objc_playground_2
ps -ax
lldb -p 3668

(lldb) exp @import Foundation
(lldb) exp (void)NSLog(@"hello");
(lldb) c
Process 3668 resuming
< you can see the output to NSLog when you open console.app >
```
But you can control `stdout`.
```
$) lldb
(lldb) target create my_playground
(lldb) process launch
(lldb) exp @import Foundation
(lldb) exp (void)NSLog(@"hello");
2018-12-13 10:14:09.638801+0000 objc_playground_2[2776:61771] hello
```