---
tags:
  - linux
  - 7zip
---
# Steps

1. Install p7zip, a command line port of 7-zip for POSIX compliant systems.

```sh
apt install p7zip
```

2. Run the following command:

```sh
# -p: Password. Note: No space between flag and value.
# -mhe: -m sets the compression method. -mhe turns on header encryption.
7z a -p"PASSWORD" -mhe=on archive.7z path/to/files/
```

# Interesting notes

- 7-zip uses LZMA, a compression algorithm invented by 7-zip's creator - Igor Pavlov.
- The `-m` flag has different functionality depending on output archive type (e.g. `.zip` vs `.7z`). Full docs [here](https://web.mit.edu/outland/arch/i386_rhel4/build/p7zip-current/DOCS/MANUAL/switches/method.htm). 