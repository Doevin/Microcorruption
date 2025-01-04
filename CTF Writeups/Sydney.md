This challenge again focuses primarily on the **check password** function. Instead of having the **create password** function present in the main function of this lock, it includes a **get password** and **check password** function, with the password stored in **register 15** afterwards.

## Check Password
- The **check password** function directly checks the user-inputted password against the stored password.
- It does this by:
  - Using offsets to compare values every two bytes.
  - Checking hex values to determine if the next 4 characters match.
 ```
 448a <check_password>
448a:  bf90 642f 0000 cmp	#0x2f64, 0x0(r15)
4490:  0d20           jnz	$+0x1c <check_password+0x22>
4492:  bf90 2644 0200 cmp	#0x4426, 0x2(r15)
4498:  0920           jnz	$+0x14 <check_password+0x22>
449a:  bf90 726f 0400 cmp	#0x6f72, 0x4(r15)
44a0:  0520           jnz	$+0xc <check_password+0x22>
44a2:  1e43           mov	#0x1, r14
44a4:  bf90 443f 0600 cmp	#0x3f44, 0x6(r15)
44aa:  0124           jz	$+0x4 <check_password+0x24>
44ac:  0e43           clr	r14
44ae:  0f4e           mov	r14, r15
44b0:  3041           ret
```
## Key Takeaways
Initially, I attempted to convert the hex value `0x2f6444266f723f44` into its ASCII equivalent and submitted that as the response. However, this approach was denied.After further research, I realized that the machine reads from memory in **little-endian** format, not big-endian. Considering this, I adjusted the hex value to `0x642f2644726f443f` before conversion.Since Microcorruption allows for hex-encoded input, I could directly submit the hex value.