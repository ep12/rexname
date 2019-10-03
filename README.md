# rexname
Simple but powerful renaming utility written in python

# requirements
Python 3.6 or newer (fstrings)

# installation
After cloning, link to the rexname executable or change your `$PATH`.
```sh
git clone https://github.com/ep12/rexname.git
sudo ln -s rexname/rexname /usr/bin/rexname
```

# examples
Fstrings are a good alternative for some things:
```sh
$ ls -1
'MyFile 1.txt'
'MyFile 10.txt'
'MyFile 2.txt'
$ # fill with 0 to have three characters per number
$ rexname -F fstr "MyFile (\d+).txt" "MyFile {g1:0>3}.txt"
$ ls -1
'MyFile 001.txt'
'MyFile 002.txt'
'MyFile 010.txt'
$ touch "MyFile 000.md"
$ # undo
$ rexname -F fstr "MyFile (?P<num>\d+)(?P<ext>.+)" "MyFile {int(num)}{ext}"
$ ls -1
'MyFile 0.md'
'MyFile 1.txt'
'MyFile 10.txt'
'MyFile 2.txt'
```
Same thing with regex:
```sh
$ ls -1
'MyFile 1.txt'
'MyFile 10.txt'
'MyFile 2.txt'
$ # fill with 0 to have three characters per number
$ rexname "MyFile (\d).txt" "MyFile 00\1.txt"
$ rexname "MyFile (\d{2}).txt" "MyFile 0\1.txt"
$ ls -1
'MyFile 001.txt'
'MyFile 002.txt'
'MyFile 010.txt'
$ touch "MyFile 000.md"
$ # undo
$ rexname "MyFile (?:0*)(?P<num>\d+)(?P<ext>.+)" "MyFile \g<num>\g<ext>"
$ ls -1
'MyFile 0.md'
'MyFile 1.txt'
'MyFile 10.txt'
'MyFile 2.txt'
```
Something regex cannot do:
```sh
$ ls -1
'MyFile 0.md'
'MyFile 1.txt'
'MyFile 10.txt'
'MyFile 2.txt'
$ # add 42 to the number
$ rexname -F fstr "MyFile (\d+)(.*)" "MyFile {int(g1)+42:0>4}{g2}"
$ ls -1
'MyFile 0042.md'
'MyFile 0043.txt'
'MyFile 0044.txt'
'MyFile 0052.txt'
$ # convert numbers to hexadecimal easily
$ rexname -F fstr "MyFile (\d+)(.*)" "MyFile 0x{hex(int(g1))[2:]:0>4}{g2}"
'MyFile 002a.md'
'MyFile 002b.txt'
'MyFile 002c.txt'
'MyFile 0034.txt'
$ # undo
$ rexname -F fstr "MyFile 0x([\da-f]+)(.*)" "MyFile {int(g1, 16)-42}{g2}"
$ ls -1
'MyFile 0.md'
'MyFile 1.txt'
'MyFile 10.txt'
'MyFile 2.txt'
```
