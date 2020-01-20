# HV19.10 Guess what

**Task:** The flag is right, of course.

**Flag:** `HV19{Sh3ll_0bfuscat10n_1s_fut1l3}`

The binary was so broken it had to be updated twice. At least time for the full flag points was extended to 48h and the binary's resistance to be solved was futile.

# Research

```bash
$ file guess3
guess3: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=5e1e9f74990e4f8f96d380d2b5264a3567a9d046, stripped
```

Essentially, there are two useful Linux tools:

* `strace` - trace system calls and signals, thus calls to the kernel
* `ltrace` - traces library calls, calls within userspace

If we run `ltrace`, we see which parameters the binary expects - and a suspicious string starting with `HV19`:

``` 
$ ltrace ./guess2
...
sprintf("exec './guess2' "$@"", "exec '%s' "$@"", "./guess2") 
...
strchr("\001H\001V\0011\0019\001{\001S\001h\0013\001l\001l\001_\0010\001b\001f\001u\001s"..., '\001') = "\001H\001V\0011\0019\001{\001S\001h\0013\001l\001l\001_\0010\001b\001f\001u\001s"...
```

Filtering for `strchr` in the third version of the challenge reveals the flag:

```bash
$ ltrace -e strchr ./guess3
...
guess3->strchr("ersa:d:i:n:p:t:u:N:", 'p')					= "p:t:u:N:"
Your input: The answer to life...
guess3->strchr(" \t\n", ' ')								= " \t\n"
guess3->strchr("HV19{Sh3ll_0bfuscat10n_1s_fut1l3"..., '$')	= nil
guess3->strchr("success"", '$')								= nil
...
```
