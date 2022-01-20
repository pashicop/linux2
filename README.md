# 1
`cd` встроенная в shell команда. Реализует базовый функционал shell с возможностью изменять внутреннее состояние оболочки ($PWD). Могла бы быть внешней, например, /bin/cd. Но внешняя программа не может влиять на внутреннее состояние оболочки. 
```
vagrant@vagrant:~$ type cd
cd is a shell builtin
```
# 2
`grep -c <some_string> <some_file>`
# 3
С помощью `pstree -p` находим systemd(1), то етсь процесс systemd имеет PID 1
# 4
Перенаправляем stderr с /dev/pts/0 на /dev/pts/1  
`ls -zzzzz 2>/dev/pts/1`
# 5
Можно создать файл 1.txt с набором слов, затем передать на вход (stdin) grep файл 1.txt с поиском по слову test (таких строк две), затем вывод (stdout) в файл 2.txt 
```
vagrant@vagrant:~$ cat 1.txt
23
test2342
34234
52
542
45
245
2452
test
1
vagrant@vagrant:~$ cat 2.txt
123
vagrant@vagrant:~$ grep test 0<1.txt 1>2.txt
vagrant@vagrant:~$ cat 2.txt
test2342
test
```
# 6
Да. Через vagrant ssh заходим в PTS, а локально через virtualbox - получаем доступ к tty. Пользуясь примером из 4 задания получаем вывод stderr из pts/0 в tty1
```
vagrant@vagrant:~$ tty
/dev/pts/0
vagrant@vagrant:~$ ls -zzzzz 2>/dev/tty1
```
![image](https://user-images.githubusercontent.com/97126500/150359451-e076c2bf-84f0-4706-bee7-6dac2cc5973d.png)

# 7
`bash 5>&1` перенаправит поток 5 на stdout 
```
vagrant@vagrant:~$ vagrant@vagrant:~$ echo netology > /proc/$$/fd/5
netology
```
отправляем вывод echo в поток 5. Так как он перенаправляется в stdout, то увидим вывод netology

vagrant@vagrant:~$ echo netology > /proc/$$/fd/6
bash: /proc/2455/fd/6: No such file or directory
vagrant@vagrant:~$
# 8
Используем промежуточный поток №5. Тогда при stdout останется в pts, а stderr передаётся через pipe `|`
```
vagrant@vagrant:~$ cat cat2.txt

vagrant@vagrant:~$ ll 5>&1 1>&2 2>&5|cat >cat2.txt
total 44
drwxr-xr-x 5 vagrant vagrant 4096 Jan 20 20:15 ./
drwxr-xr-x 3 root    root    4096 Dec 19 19:42 ../
drwxrwxr-x 2 vagrant vagrant 4096 Jan 20 19:55 1/
-rw-r--r-- 1 vagrant vagrant  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 vagrant vagrant 3771 Feb 25  2020 .bashrc
drwx------ 2 vagrant vagrant 4096 Dec 19 19:42 .cache/
-rw-rw-r-- 1 vagrant vagrant    0 Jan 20 20:16 cat2.txt
-rw-rw-r-- 1 vagrant vagrant   54 Jan 20 20:15 cat.txt
-rw-r--r-- 1 vagrant vagrant  807 Feb 25  2020 .profile
drwx------ 2 vagrant root    4096 Jan 20 19:42 .ssh/
-rw-r--r-- 1 vagrant vagrant    0 Dec 19 19:42 .sudo_as_admin_successful
-rw-r--r-- 1 vagrant vagrant    6 Dec 19 19:42 .vbox_version
-rw-r--r-- 1 root    root     180 Dec 19 19:44 .wget-hsts
vagrant@vagrant:~$ cat cat2.txt
vagrant@vagrant:~$ ll egdfgd 5>&1 1>&2 2>&5|cat >cat2.txt
vagrant@vagrant:~$ cat cat2.txt
ls: cannot access 'egdfgd': No such file or directory
vagrant@vagrant:~$
```
# 9
```
vagrant@vagrant:~$ cat /proc/$$/environ
SHELL=/bin/bashPWD=/home/vagrantLOGNAME=vagrantXDG_SESSION_TYPE=ttyMOTD_SHOWN=pamHOME=/home/vagrantLANG=en_US.UTF-8LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:SSH_CONNECTION=10.0.2.2 62314 10.0.2.15 22LESSCLOSE=/usr/bin/lesspipe %s %sXDG_SESSION_CLASS=userTERM=xterm-256colorLESSOPEN=| /usr/bin/lesspipe %sUSER=vagrantSHLVL=3XDG_SESSION_ID=7XDG_RUNTIME_DIR=/run/user/1000SSH_CLIENT=10.0.2.2 62314 22XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktopPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/binDBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/busSSH_TTY=/dev/pts/0OLDPWD=/proc/2500/fd_=/usr/bin/bashvagrant@vagrant:~$
```
Можно увидеть командой export
```
export
declare -x DBUS_SESSION_BUS_ADDRESS="unix:path=/run/user/1000/bus"
declare -x HOME="/home/vagrant"
declare -x LANG="en_US.UTF-8"
declare -x LESSCLOSE="/usr/bin/lesspipe %s %s"
declare -x LESSOPEN="| /usr/bin/lesspipe %s"
declare -x LOGNAME="vagrant"
declare -x LS_COLORS="rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:"
declare -x MOTD_SHOWN="pam"
declare -x OLDPWD="/home/vagrant/t1"
declare -x PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin"
declare -x PWD="/home/vagrant"
declare -x SHELL="/bin/bash"
declare -x SHLVL="4"
declare -x SSH_CLIENT="10.0.2.2 62314 22"
declare -x SSH_CONNECTION="10.0.2.2 62314 10.0.2.15 22"
declare -x SSH_TTY="/dev/pts/0"
declare -x TERM="xterm-256color"
declare -x USER="vagrant"
declare -x XDG_DATA_DIRS="/usr/local/share:/usr/share:/var/lib/snapd/desktop"
declare -x XDG_RUNTIME_DIR="/run/user/1000"
declare -x XDG_SESSION_CLASS="user"
declare -x XDG_SESSION_ID="7"
declare -x XDG_SESSION_TYPE="tty"
vagrant@vagrant:~$
```
# 10
```
/proc/[pid]/cmdline
Доступный только для чтения файл содержит полную командную строку для процесса, если этот процесс не является зомби. В последнем случае в этом файле ничего нет: то есть чтение этого файла вернет 0 символов. Аргументы командной строки отображаются в этом файле как набор строк, разделенных нулевыми байтами ('\0'), с дополнительным нулевым байтом после последней строки.
```
```
/proc/[pid]/exe
Файл представляет собой символическую ссылку, содержащую фактический путь к выполняемой команде.
```
# 11
Версия SSE 4.2
```
vagrant@vagrant:~$ grep sse /proc/cpuinfo
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni ssse3 cx16 pcid sse4_1 sse4_2 hypervisor lahf_lm invpcid_single pti fsgsbase invpcid md_clear flush_l1d arch_capabilities
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni ssse3 cx16 pcid sse4_1 sse4_2 hypervisor lahf_lm invpcid_single pti fsgsbase invpcid md_clear flush_l1d arch_capabilities
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni ssse3 cx16 pcid sse4_1 sse4_2 hypervisor lahf_lm invpcid_single pti fsgsbase invpcid md_clear flush_l1d arch_capabilities
```
# 12
```
vagrant@vagrant:~$ ssh localhost 'tty'
vagrant@localhost's password:
not a tty
vagrant@vagrant:~$ ssh -t localhost 'tty'
vagrant@localhost's password:
/dev/pts/3
Connection to localhost closed.
vagrant@vagrant:~$
```
Это происходит потому что автоматически не выдаётся pts при запуске команды через `ssh`. Ключ `-t` заставляет принудительно выдать pts
```
-t      Force pseudo-terminal allocation.  This can be used to execute arbitrary screen-based programs on a remote machine, which can be very use‐
             ful, e.g. when implementing menu services.  Multiple -t options force tty allocation, even if ssh has no local tty.
```
# 13
reptyr -T PID
# 14
В отличие от `echo` команда `tee` читает данные stdin и передаёт в stdout или файл. `echo` только выводит на экран текст, а перенаправлением в файл отвечает shell. Поэтому `sudo echo string > /root/new_file` не сможет перенаправить stdout в файл, так как shell запущен не из под `sudo`. А `echo string | sudo tee /root/new_file` принимает строчку от `echo` на вход и в привилегированном режиме `tee` может перенаправить выход в /root/new_file 
