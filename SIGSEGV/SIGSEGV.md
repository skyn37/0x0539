# Challenge 5: SIGSEGV (40 Points)


- **Difficulty:** Easy
- **Points:** 40

You are provided with a `nc` command:

```bash
nc challenges.0x0539.net 7071
```

Upon execution, the program prompts you with the question, "Tell me, what's your name?"

## Instructions

1. Investigate the concept of SIGSEGV (signal segmentation violation) in Unix systems, particularly its association with errors when a program attempts to access an invalid memory location.

2. SIGSEGV is often triggered by programming errors resulting in invalid memory access, such as buffer overflow. Craft a large enough input to trigger a segmentation fault.

3. Example of a triggering input:

   ```plaintext
   sssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssss
   sssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssss
   sssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssss
   sssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssss
   s
   ```

4. The triggering input should cause a segfault error, and the flag will be revealed in the output:

   ```
   FLAG{I_h0p3_RBP_w4s_AAAAAAAA}
   ```

