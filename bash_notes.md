# Bash things I've learned

## April 9, 2021 

- `mktemp` = allows you make temporary filename. useful for throwaway scripts. See [here](https://www.mktemp.org/manual.html#:~:text=mktemp%20is%20provided%20to%20allow,for%20an%20attacker%20to%20win.)
- TTY issues (which is a docker thing) is generated from `docker run -it` which makes docker interactive. apparenntly when it's interactive it doesn't like loops/functions.
