# radius2-examples
Examples of using the radius2 CLI tool to solve reversing challenges

install `radare2` from git then run `cargo install radius2` to get `radius2`

## ais3_crackme 
```bash
$ radius2 -p ais3_crackme -s flag 184 -X wrong

  flag : "ais3{I_tak3_g00d_n0t3s}"

```
Explanation:
- `-p ais3_crackme` sets the path to the target binary
- `-s flag 184` creates a 184 **bit** symbolic value named "flag"
- `-X wrong` sets the execution to avoid code XREFs to strings containing "wrong"
## defcamp_r100 
```bash
$ radius2 -p r100 -s stdin 96 -X Incorrect

  stdin : "Code_Talkers"

```
Explanation:
- `-s stdin 96` creates a 96 **bit** symbolic value named "stdin"
  - the name `stdin` is special as it also writes this value to standard input

## defcamp_r200 
```bash
$ radius2 -p r200 -s stdin 48 -X Incorrect -m 0x4007c1

  stdin : "rotors"

```
Explanation:
- `-m 0x4007c1` sets a mergepoint at the address 0x4007c1 which prevents state explosion by combining all states which reach this point

## google2016_unbreakable_1
```bash
$ radius2 -zp unbreakable -s flag 408 -c flag 'CTF{' -B 'Thank you'   

  flag : "CTF{0The1Quick2Brown3Fox4Jumped5Over6The7Lazy8Fox9}"

```
Explanation: 
- `-c flag 'CTF{'` constrains the first four bytes of `flag` to be "CTF{"
- `-B 'Thank you'` sets a breakpoint at the XREF of "Thank you"
- `-z` configures the engine to check sat infrequently, for faster execution

## securityfest_fairlight
```bash
$ radius2 -p fairlight -s flag 112 -A. flag -X failure   

  flag : "4ngrman4gem3nt"

```
Explanation:
- `-A. flag` sets `argv[0]` to be "." and `argv[1]` to the symbolic value named "flag"

## foobarctf23 
```bash
$ radius2 -p chall -s stdin 192 -b 0x1729   

  stdin : "GLUG{bE_W@tER_my_FriEnD}"

```
Explanation:
- `-b 0x1729` sets a breakpoint at address 0x1729

## greyctf23_crackme2
```bash
$ radius2 -p crackme -s flag 320 -X Wrong -B Correct

  flag : "grey{d1d_y0u_s0lv3_by_emul4t1ng?_1e4b8a}"

```
Explanation:
- `radius2` now defaults to putting the first symbolic value in `argv[1]` without `-A. flag`

## ollvm
```bash
$ for hash in `tail -n1 README`; do radius2 -p ollvm -s hex 128 -C1 "Output: $hash"; done  

  hex : "6d6972726F725f6d"


  hex : "6972726f725F6F6e"

...
$ rax2 -s 6d6972726F725f6d6972726f725F6F6e5f7468655F77616c6c5f77686f735f...
mirror_mirror_on_the_wall_whos_the_ugliest_handler_of_them_all?!
```
Explanation:
- `-C1 "Output: $hash"` constrains the contents of fd 1 (`stdout`) to the provided string after execution finishes 
  - `-c` and `-C` can constrain symbol, register, and file contents

## escrackme
```bash
$ radius2 -p apk://escrackme.apk -s flag 32 -S v7 flag 32 -a 0x1bd1cc -b 0x1bd21e   

  flag : 0xcafebabe

```
Explanation:
- `-p` accepts any URI that works with `r2`, including `apk://`, `frida://`, `gdb://`, ... 
- `-S v7 flag 32` directly sets register `v7` to the 32 bit symbolic value `flag`
- `-a 0x1bd1cc` sets execution to begin at address 0x1bd1cc

## dicectf_2023
```bash
$ radius2 -zp babymix -s stdin 176 -X Incorrect   

  stdin : "m1x_it_4ll_t0geth3r!1!"

```

## foobarctf_2022
```bash
$ r2 -qc '/ad mov dword [rbp - 4], 0~[0]' babyrev > avoids 
$ radius2 -zp babyrev -s flag 336 -X INCORRECT -x `cat avoids`

  flag : "GLUG{C01nc1d3nc3_c4n_b3_fr3aky_T6LSERDYB6}"

```

## ida 
```bash
$ radius2 -p chal -s flag 192 -c flag '[ -~]' -b 0x1610  

  flag : "Fr33_M4dam3-De/M4inten0n"

```
Explanation:
- `-c flag '[ -~]'` constrains each byte of `flag` to be in the range `[ -~]`

## vmwhere2
```bash
$ radius2 -p chal -A. program -s stdin 368 -c stdin '[a-z0-9_{}]' -1m 0x16a1

  stdin : "uiuctf{b4s3_3_1s_b4s3d_just_l1k3_vm_r3v3rs1ng}"

=====================================stdout=====================================
Welcome to VMWhere 2!
Please enter the password:
Correct!

================================================================================
```
Explanation:
- `-1` shows contents of `stdout` for the final state
- `-A. program` sets `argv[1]` to the string `program` which is the filename of the bytecode
  - `radius2` will read files from the host filesystem, but won't write to them

## gatecodegate
```bash
$ radius2 -Np main -s stdin 256 -B Correct -C eax 0

  stdin : "4nD&0r&s|~|l,5hr_4rE_fxxk!ng_g0d"

```
Explanation:
- `-N` signals that the code is not self-modifying which can result in faster execution
- `-C eax 0` constrains the register `eax` to be `0` after execution finishes

## ecw2023_moth
```bash
$ radius2 -1zp moth -s flag 648 -X Nope -m 0x137f 

  flag : "bcaedbabaadbcacdcdbcadbebabdebecaceccadadedabdbebcbcedcacaedababebdbcedcacacedaba"

=====================================stdout=====================================
Well done, flag is ECW{md5(input)}

================================================================================
```
Explanation:
- `-m 0x137f` sets a mergepoint between two loops to prevent state explosion

## serial 
```bash
$ radius2 -p serial -s flag 128 -H 0x400a10 'flag,0x200,rbp,-,=[16]' -B ' :)'

  flag : "EZ9dmq4c8g9G7bAV"
```
Explanation:
- `-H 0x400a10 'flag,0x200,rbp,-,=[16]'` hook address 0x400a10 to write `flag` to `[rbp-0x200]`

## 9447_nobranch
```bash
$ radius2 -p nobranch -s flag 144 -c flag '[ -~]' -C1 HMQhQLi6VqgeOj78AbiaqquK3noeJt
  
  flag : "9447{1lVjQp_h33Ps}"

```

## hitcon2017_sakura
```bash
$ radius2 -MNzp sakura -b SHA256 -s stdin 3200 -c stdin '[0-9]'  

  stdin : "0000000000000000000000000092000041000091017037819200638046830296180081071098370000890920000936091500000810120008216028430000310120000498093103700293410003792062019283701207128017200001900000092000091000000000000000000000000014065000271049000041835792008964127501250130730910053037076072086008600026000094805120005300036000570180860005200051048049053808506900850178632940073615284000310740000003501230"

```
Explanation:
- `-M` is an experimental feature that merges states on every jump target
- `-b SHA256` sets a breakpoint at the address of the PLT entry for `SHA256`

## greyctf_crackme3
```bash
$ radius2 -Mp crackme3 -s stdin 192 -H putchar 87,A0,-,_ 

  stdin : "grey{r_y0u_d1zzy?_9bfad}"

```
Explanation:
- `-H putchar 87,A0,-,_` hooks putchar and asserts the first argument A0 != 'W' so that "Wrong" will not appear in the output 

## picoctf2024

```bash 
$ radius2 -p crackme100 -s stdin 400 -c stdin '[a-z_]' -zX FAIL

  stdin : "lumsopgxaocglvijjikbpwhfvchdmeipbotdjhxmwzilkvguun"

```
Explanation:
- Weirdly even though it is not used the _ in the allowed chars solves the challenge 3x faster

## angstromctf

```bash 
$ radius2 -p autorev_assemble -s stdin 1600 -B SOLVED 

  stdin : "Blockchain big data solutions now with added machine learning. Enjoy! I sincerely hope you actf{wr0t3_4_pr0gr4m_t0_h3lp_y0u_w1th_th1s_df93171eb49e21a3a436e186bc68a5b2d8ed} instead of doing it by hand."

```