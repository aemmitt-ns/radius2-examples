# radius2-examples
Examples of using the radius2 CLI tool to solve reversing challenges

## defcamp_r100 

```
$ radius2 -p r100 -s stdin 96 -X Incorrect

  stdin : "Code_Talkers"

```
## defcamp_r200 
```
$ radius2 -p r200 -s stdin 48 -X Incorrect -m 0x4007c1

  stdin : "rotors"

```
This challenge requires state merging to solve in a reasonable time. With this mergepoint it takes ~0.1 seconds. 
## ais3_crackme 

```
$ radius2 -p ais3_crackme -s flag 184 -A. flag -X wrong

  flag : "ais3{I_tak3_g00d_n0t3s}"

```
## google2016_unbreakable_1
```
$ radius2 -zp unbreakable -s flag 408 -A. flag -c flag 'CTF{' -B 'Thank you'   

  flag : "CTF{0The1Quick2Brown3Fox4Jumped5Over6The7Lazy8Fox9}"

```
## securityfest_fairlight
```
$ radius2 -p fairlight -s flag 112 -A. flag -X failure   

  flag : "4ngrman4gem3nt"

```
## foobarctf23 
```
$ radius2 -p chall -s stdin 192 -b 0x1729   

  stdin : "GLUG{bE_W@tER_my_FriEnD}"

```
## greyctf23_crackme2
```
$ radius2 -p crackme -s flag 320 -A. flag -X Wrong -B Correct

  flag : "grey{d1d_y0u_s0lv3_by_emul4t1ng?_1e4b8a}"

```
## ollvm
```
$ for hash in `tail -n1 README`; do radius2 -p ollvm -s hex 128 -A. hex -C1 "Output: $hash"; done  

  hex : "6d6972726F725f6d"


  hex : "6972726f725F6F6e"

...
$ rax2 -s 6d6972726F725f6d6972726f725F6F6e5f7468655F77616c6c5f77686f735f...
mirror_mirror_on_the_wall_whos_the_ugliest_handler_of_them_all?!
```
## escrackme
```
$ radius2 -p apk://escrackme.apk -s flag 32 -S v7 flag 32 -a 0x1bd1cc -b 0x1bd21e   

  flag : 0xcafebabe
  
```
This is an example of using radius2 on dalvik bytecode within an android app
## dicectf_2023
```
$ radius2 -zp babymix -s stdin 176 -X Incorrect   

  stdin : "m1x_it_4ll_t0geth3r!1!"

```
## foobarctf_2022
```
$ r2 -qc '/ad mov dword [rbp - 4], 0~[0]' babyrev > avoids 
$ radius2 -zp babyrev -s flag 336 -A. flag -X INCORRECT -x `cat avoids`

  flag : "GLUG{C01nc1d3nc3_c4n_b3_fr3aky_T6LSERDYB6}"

```