# Rock Paper Scissors (120 Points)

In this challenge, obtaining the flag is straightforward—defeat the bot in a game of rock-paper-scissors. A hint suggests the bot might not play fair...

## Tools

- Python 2
- Netcat (`nc`)
- `checksec`
- `file`
- Ghidra
- GDB

## Instructions

Connect to the server using:

```bash
nc challenges.0x0539.net 7072
```

<!-- and use the provided `rps` binary. -->

Upon connection, the bot prompts for a name length and a name. It initializes a rock-paper-scissors game where, as the player, no matter what valid option you choose, the bot will counter with an option that beats yours:

- You: Rock
- Bot: Paper
- You: Paper
- Bot: Scissors
 
... and so on.

If you attempt options not listed, the program terminates with an unknown command message.

Upon experimenting with the name and name length inputs, you may trigger a segmentation fault (segfault) without the typical "You lose! >:) It's impossible for you to beat me!" message.

Exploiting this segfault becomes the potential exploit path, but how?

Additional information about the program can be obtained from the `rps` binary:

```bash
file rps
checksec rps
```

This reveals that the binary is a 32-bit dynamically linked Linux executable with partial RELRO, a found Canary, NX enabled, and no PIE.

Using Ghidra to disassemble, locate the `win` function and note its address.

Run the program in GDB with the input causing the crash. GDB will display the crash address. If using all `RR`, the address might be `0x52525252`.

Now, determine which `R` is leaked. Craft a long string of `R`s, `S`s, and `P`s with a unique sequence, run the program in GDB with the string, note the address, convert it to ASCII, and align it with your string to find the sequence.

Example:

```bash
RRPPRRSPRRPP
    RRSP
```

After identifying the malicious parts of the string, substitute them for the `win` function address in reverse order (due to LSB):

```bash
RRRRPRRRSRRPPSR\x51\x86\x04\x08PRSS
```

Generate a payload file:

```bash
python2 -c 'print "2222222222222222222222222222222222222222_RRRRPRRRSRRPPSR" + "\x51\x86\x04\x08" + "PRSS"' > pwn
```

Use Netcat to trigger the `win` function:

```bash
nc challenges.0x0539.net 7072 < pwn
```

This should exploit the program and reveal the flag:

```
FLAG{w3ll_th4ts_4_wr4p_f0lks}
```