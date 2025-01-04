This problem closely follows off of the Hanoi problem with teh key exception that there is no longer a direct check for a certain character in the password.

## Login Function
  - Same function as in the Hanoi challenge problem with the exception of the check for the 17th character to exploit.
  - When at the return the top of the stack points to 43fe which in the live memory dump is listed as 3c44, which in little endian would be 443c, which in this problem is the <__stop_progExec__> function. 
  - The program stores the user password in the 43f0 address so as long as the password is long enough it can manipulate the stack pointer return
```
4500 <login>
4500:  3150 f0ff      add	#0xfff0, sp
4504:  3f40 7c44      mov	#0x447c "Enter the password to continue.", r15
4508:  b012 a645      call	#0x45a6 <puts>
450c:  3f40 9c44      mov	#0x449c "Remember: passwords are between 8 and 16 characters.", r15
4510:  b012 a645      call	#0x45a6 <puts>
4514:  3e40 3000      mov	#0x30, r14
4518:  0f41           mov	sp, r15
451a:  b012 9645      call	#0x4596 <getsn>
451e:  0f41           mov	sp, r15
4520:  b012 5244      call	#0x4452 <test_password_valid>
4524:  0f93           tst	r15
4526:  0524           jz	$+0xc <login+0x32>
4528:  b012 4644      call	#0x4446 <unlock_door>
452c:  3f40 d144      mov	#0x44d1 "Access granted.", r15
4530:  023c           jmp	$+0x6 <login+0x36>
4532:  3f40 e144      mov	#0x44e1 "That password is not correct.", r15
4536:  b012 a645      call	#0x45a6 <puts>
453a:  3150 1000      add	#0x10, sp
453e:  3041           ret
```

### Key Takeaways
- Although the direct overflow for the password has been removed there is still an overflow issue in this problem. Recognizing that the password can overflow into the stack pointer the user can input a password of 16 characters followed by `(E` (2845 in hex but accommodating for endianness) to overflow into the buffer and redirect the program into 4528, a call to start the unlock function. Alternatively you could switch `(E` with `FD` to call the unlock function directly through the return call.

```
4446 <unlock_door>
4446:  3012 7f00      push	#0x7f
444a:  b012 4245      call	#0x4542 <INT>
444e:  2153           incd	sp
4450:  3041           ret
```