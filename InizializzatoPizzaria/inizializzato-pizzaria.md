# Inizializzato Pizzaria Challenge

## Challenge Description

**Connect:** `nc challenges.0x0539.net 3001`

**File:** challenge (this is included with the task)

Upon connecting to the server, you will be presented with the following options:

```
[1] Create a Pizza
[2] Become Admin
[3] Create Pizza + Become Admin
[4] Print the Key (admin only)
[5] Exit
```

To obtain the flag, follow these steps:

1. Choose option [3].
2. Set the pizza ID to 1, and the other ingredients can be arbitrary.
3. When prompted for the password, enter 1.
4. You will receive a prompt indicating that you are now an admin.
5. Choose option [4] to reveal the flag: `FLAG{_r3m3mb3r_t0_initi4liz3_buff3rs}`.


## Challenge Explanation

This challenge involves understanding and exploiting undefined behavior in the program. Upon decompiling the provided file using Ghidra, you'll notice that two functions are called sequentially, both containing uninitialized variables with the same name. This undefined behavior allows you to manipulate the program's state and gain admin privileges.


## Additional Information

If you are interested in the technical details, you can explore a simplified code example bellow and also check : https://stackoverflow.com/questions/19549528

```
#include <stdio.h>
#include <string.h>

void func1() {
    char secret[1];
    secret[0] = 'a';
    
    printf("func1 secret: %s\n", secret);
    printf("func1 secret address: %p\n", (void *)secret);
}

int func2() {
    char secret[1];
    // here we are using secret but this is undefined behavior
    printf("func2 secret: %s\n", secret); 
    printf("func2 secret address: %p\n", (void *)secret);
}

int main() {
    func1();
    func2();
    
    return 0;
}

```