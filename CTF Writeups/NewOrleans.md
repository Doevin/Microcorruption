The main functions to pay attention to in this problem are the **create password** and **check password** functions.

## Create Password
- This function stores the created systempassword in memory location `2400` as `'=!mMI5Q'`.
- After this, **register 15** will point to the location where the user-inputted password is stored.
```
447e <create_password>
447e:  3f40 0024      mov	#0x2400, r15
4482:  ff40 3d00 0000 mov.b	#0x3d, 0x0(r15)
4488:  ff40 2100 0100 mov.b	#0x21, 0x1(r15)
448e:  ff40 6d00 0200 mov.b	#0x6d, 0x2(r15)
4494:  ff40 4d00 0300 mov.b	#0x4d, 0x3(r15)
449a:  ff40 4900 0400 mov.b	#0x49, 0x4(r15)
44a0:  ff40 3500 0500 mov.b	#0x35, 0x5(r15)
44a6:  ff40 5100 0600 mov.b	#0x51, 0x6(r15)
44ac:  cf43 0700      mov.b	#0x0, 0x7(r15)
44b0:  3041           ret
```
## Check Password
- This function:
  1. Resets **register 14**.
  2. Moves the position **register 13** points to the location of the user-inputted password.
- It then loops through all the bits of the actual password, checking them against the user input:
  - The loop continues until:
    - All bits of the actual password have been checked, or
    - A mismatch is found.
  
## Key Takeaways
Since the program stores the password directly in memory, it can be easily found and inputted into the program. The program will take the user inputted password iterating through the characters to ensure matches.
