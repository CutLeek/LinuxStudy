# Linux常用命令

## 一、cd

cd即change directory，改变工作目录，常用的命令如下

### 1、进入/opt目录

```
cd /opt
```

### 2、进入当前用户的家目录

```
cd
```

### 3、进入到当前目录的上级目录

```
cd ..
```

### 4、返回进入此目录之前的目录

```
cd -
```

## 二、ls

ls就是list的缩写，常用于列出目录下的文件，下面是ls命令涉及到的所有的参数

```shell
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'never', 'auto',
                               or 'always' (the default); more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                         like -l, but do not list owner
      --group-directories-first
                             group directories before files;
                               can be augmented with a --sort option, but any
                               use of --sort=none (-U) disables grouping
  -G, --no-group             in a long listing, don't print group names
  -h, --human-readable       with -l, print sizes in human readable format
                               (e.g., 1K 234M 2G)
      --si                   likewise, but use powers of 1000 not 1024
  -H, --dereference-command-line
                             follow symbolic links listed on the command line
      --dereference-command-line-symlink-to-dir
                             follow each command line symbolic link
                               that points to a directory
      --hide=PATTERN         do not list implied entries matching shell PATTERN
                               (overridden by -a or -A)
      --indicator-style=WORD  append indicator with style WORD to entry names:
                               none (default), slash (-p),
                               file-type (--file-type), classify (-F)
  -i, --inode                print the index number of each file
  -I, --ignore=PATTERN       do not list implied entries matching shell PATTERN
  -k, --kibibytes            default to 1024-byte blocks for disk usage
  -l                         use a long listing format
  -L, --dereference          when showing file information for a symbolic
                               link, show information for the file the link
                               references rather than for the link itself
  -m                         fill width with a comma separated list of entries
  -n, --numeric-uid-gid      like -l, but list numeric user and group IDs
  -N, --literal              print raw entry names (don't treat e.g. control
                               characters specially)
  -o                         like -l, but do not list group information
  -p, --indicator-style=slash
                             append / indicator to directories
  -q, --hide-control-chars   print ? instead of nongraphic characters
      --show-control-chars   show nongraphic characters as-is (the default,
                               unless program is 'ls' and output is a terminal)
  -Q, --quote-name           enclose entry names in double quotes
      --quoting-style=WORD   use quoting style WORD for entry names:
                               literal, locale, shell, shell-always, c, escape
  -r, --reverse              reverse order while sorting
  -R, --recursive            list subdirectories recursively
  -s, --size                 print the allocated size of each file, in blocks
  -S                         sort by file size
      --sort=WORD            sort by WORD instead of name: none (-U), size (-S),
                               time (-t), version (-v), extension (-X)
      --time=WORD            with -l, show time as WORD instead of default
                               modification time: atime or access or use (-u)
                               ctime or status (-c); also use specified time
                               as sort key if --sort=time
      --time-style=STYLE     with -l, show times using style STYLE:
                               full-iso, long-iso, iso, locale, or +FORMAT;
                               FORMAT is interpreted like in 'date'; if FORMAT
                               is FORMAT1<newline>FORMAT2, then FORMAT1 applies
                               to non-recent files and FORMAT2 to recent files;
                               if STYLE is prefixed with 'posix-', STYLE
                               takes effect only outside the POSIX locale
  -t                         sort by modification time, newest first
  -T, --tabsize=COLS         assume tab stops at each COLS instead of 8
  -u                         with -lt: sort by, and show, access time;
                               with -l: show access time and sort by name;
                               otherwise: sort by access time
  -U                         do not sort; list entries in directory order
  -v                         natural sort of (version) numbers within text
  -w, --width=COLS           assume screen width instead of current value
  -x                         list entries by lines instead of by columns
  -X                         sort alphabetically by entry extension
  -1                         list one file per line

SELinux options:

  --lcontext                 Display security context.   Enable -l. Lines
                             will probably be too wide for most displays.
  -Z, --context              Display security context so it fits on most
                             displays.  Displays only mode, user, group,
                             security context and file name.
  --scontext                 Display only security context and file name.
      --help     display this help and exit
      --version  output version information and exit

```

ls常用的命令和参数其实并不多，接下来列举几个常用的命令

### 1、列出当前目录下的文件

```
ls
```

### 2、列出指定目录下的文件

```
ls /opt      此处以/opt目录举例，可以是任意目录
```

### 3、列出目录下的所有文件，包含隐藏文件

```
ls -a
```

### 4、列出文件大小、权限、所有者等详细信息

```
ls -l
```

### 5、列出的时候，以文件修改时间排序

```
ls -t
```

### 6、排序时以相反次序排列，通常与-t选项一起使用

```
ls -r
```

## 三、pwd

pwd，即print work directory，常被用来查看当前的工作目录，用法也非常的简单，只需要在终端输入pwd即可

```
pwd
```

## 四、mkdir

我们通常使用mkdir命令来创建一个目录，创建目录的时候会去校验这个目录是不是已经存在了，如果已经存在则不会再去创建，所有的参数如下

```shell
  -m, --mode=MODE   set file mode (as in chmod), not a=rwx - umask
  -p, --parents     no error if existing, make parent directories as needed
  -v, --verbose     print a message for each created directory
  -Z                   set SELinux security context of each created directory
                         to the default type
      --context[=CTX]  like -Z, or if CTX is specified then set the SELinux
                         or SMACK security context to CTX
      --help     display this help and exit
      --version  output version information and exit


```

### 1、创建一个目录

```
mkdir /soft
```

### 2、创建目录的时候，如果父目录不存在，那么一并创建出来

```
mkdir -p /soft/tmp

注：如果/soft目录不存在的话，直接执行mkdir /soft/tmp是无法创建的，需要先创建/soft目录，而使用-p参数之后，会将/soft目录和/soft/tmp目录一并创建出来
```

### 3、创建的时候显示过程，用的比较少，在一次性创建很多目录的时候会用到

```
mkdir -v /soft
```

### 4、创建目录的时候指定目录权限为777

```
mkdir -m 777 /soft
```

### 5、一次创建一个项目会用到的所有目录

```
mkdir -vp /soft/{lib,bin,doc/{doc,tmp}}
```

## 五、rm

rm命令就是Linux用来删除文件或目录的命令，因为删除的数据无法进行恢复，所以在使用此命令的时候，需要小心谨慎一些，下面是rm的所有参数

```shell
  -f, --force           ignore nonexistent files and arguments, never prompt
  -i                    prompt before every removal
  -I                    prompt once before removing more than three files, or
                          when removing recursively; less intrusive than -i,
                          while still giving protection against most mistakes
      --interactive[=WHEN]  prompt according to WHEN: never, once (-I), or
                          always (-i); without WHEN, prompt always
      --one-file-system  when removing a hierarchy recursively, skip any
                          directory that is on a file system different from
                          that of the corresponding command line argument
      --no-preserve-root  do not treat '/' specially
      --preserve-root   do not remove '/' (default)
  -r, -R, --recursive   remove directories and their contents recursively
  -d, --dir             remove empty directories
  -v, --verbose         explain what is being done
      --help     display this help and exit
      --version  output version information and exit
```

### 1、删除一个文件

```
rm a.txt
注：此命令执行后会询问是否确定要删除，确认后会执行真正的删除操作
```

### 2、递归删除，删除一个目录及目录下的所有内容

```
rm -r /soft
```

### 3、强制并递归删除一个目录

```
rm -rf /soft
```

## 六、mv

mv就是move的意思，通常用来移动一个文件或目录，或者是给文件改名，以下为mv的所有参数

```shell
      --backup[=CONTROL]       make a backup of each existing destination file
  -b                           like --backup but does not accept an argument
  -f, --force                  do not prompt before overwriting
  -i, --interactive            prompt before overwrite
  -n, --no-clobber             do not overwrite an existing file
If you specify more than one of -i, -f, -n, only the final one takes effect.
      --strip-trailing-slashes  remove any trailing slashes from each SOURCE
                                 argument
  -S, --suffix=SUFFIX          override the usual backup suffix
  -t, --target-directory=DIRECTORY  move all SOURCE arguments into DIRECTORY
  -T, --no-target-directory    treat DEST as a normal file
  -u, --update                 move only when the SOURCE file is newer
                                 than the destination file or when the
                                 destination file is missing
  -v, --verbose                explain what is being done
  -Z, --context                set SELinux security context of destination
                                 file to default type
      --help     display this help and exit
      --version  output version information and exit

```

### 1、给一个文件改名，例如将a.txt改为b.txt

```
mv a.txt b.txt
```

### 2、移动一个文件，例如将a.txt移动到/tmp下

```
mv a.txt /tmp
```

### 3、将a.txt改为b.txt，如果b.txt已存在，强制覆盖，并在覆盖之前备份b.txt

```
mv -bf a.txt b.txt
```

## 七、cp

cp，就是copy的意思，通常用来复制文件，所有参数如下

```shell
  -a, --archive                same as -dR --preserve=all
      --attributes-only        don't copy the file data, just the attributes
      --backup[=CONTROL]       make a backup of each existing destination file
  -b                           like --backup but does not accept an argument
      --copy-contents          copy contents of special files when recursive
  -d                           same as --no-dereference --preserve=links
  -f, --force                  if an existing destination file cannot be
                                 opened, remove it and try again (this option
                                 is ignored when the -n option is also used)
  -i, --interactive            prompt before overwrite (overrides a previous -n
                                  option)
  -H                           follow command-line symbolic links in SOURCE
  -l, --link                   hard link files instead of copying
  -L, --dereference            always follow symbolic links in SOURCE
  -n, --no-clobber             do not overwrite an existing file (overrides
                                 a previous -i option)
  -P, --no-dereference         never follow symbolic links in SOURCE
  -p                           same as --preserve=mode,ownership,timestamps
      --preserve[=ATTR_LIST]   preserve the specified attributes (default:
                                 mode,ownership,timestamps), if possible
                                 additional attributes: context, links, xattr,
                                 all
  -c                           deprecated, same as --preserve=context
      --no-preserve=ATTR_LIST  don't preserve the specified attributes
      --parents                use full source file name under DIRECTORY
  -R, -r, --recursive          copy directories recursively
      --reflink[=WHEN]         control clone/CoW copies. See below
      --remove-destination     remove each existing destination file before
                                 attempting to open it (contrast with --force)
      --sparse=WHEN            control creation of sparse files. See below
      --strip-trailing-slashes  remove any trailing slashes from each SOURCE
                                 argument
  -s, --symbolic-link          make symbolic links instead of copying
  -S, --suffix=SUFFIX          override the usual backup suffix
  -t, --target-directory=DIRECTORY  copy all SOURCE arguments into DIRECTORY
  -T, --no-target-directory    treat DEST as a normal file
  -u, --update                 copy only when the SOURCE file is newer
                                 than the destination file or when the
                                 destination file is missing
  -v, --verbose                explain what is being done
  -x, --one-file-system        stay on this file system
  -Z                           set SELinux security context of destination
                                 file to default type
      --context[=CTX]          like -Z, or if CTX is specified then set the
                                 SELinux or SMACK security context to CTX
      --help     display this help and exit
      --version  output version information and exit
```

### 1、复制a.txt到/tmp下

```
cp a.txt /tmp
注：若文件已存在，会询问是否覆盖
```

### 2、复制a.txt到/tmp下并改名为b.txt

```
cp a.txt /tmp/b.txt
```

### 3、复制整个a目录到b目录下

```
cp -r a/ b/
```

## 八、touch

touch命令通常被用来创建一个文件或者是修改文件的日期，一般我们只用来创建文件，以下为touch的所有参数

```shell
  -a                     change only the access time
  -c, --no-create        do not create any files
  -d, --date=STRING      parse STRING and use it instead of current time
  -f                     (ignored)
  -h, --no-dereference   affect each symbolic link instead of any referenced
                         file (useful only on systems that can change the
                         timestamps of a symlink)
  -m                     change only the modification time
  -r, --reference=FILE   use this file's times instead of current time
  -t STAMP               use [[CC]YY]MMDDhhmm[.ss] instead of current time
      --time=WORD        change the specified time:
                           WORD is access, atime, or use: equivalent to -a
                           WORD is modify or mtime: equivalent to -m
      --help     display this help and exit
      --version  output version information and exit
```

### 1、创建一个a.txt的文件

```
touch a.txt
```

### 2、把a.txt的时间改为和b.txt一致

```shell
touch -r a.txt b.txt
```

## 九、cat

cat我们常用来查看某个文件，默认将文件的所有内容返回到屏幕上，以下为cat的所有参数

```shell
  -A, --show-all           equivalent to -vET
  -b, --number-nonblank    number nonempty output lines, overrides -n
  -e                       equivalent to -vE
  -E, --show-ends          display $ at end of each line
  -n, --number             number all output lines
  -s, --squeeze-blank      suppress repeated empty output lines
  -t                       equivalent to -vT
  -T, --show-tabs          display TAB characters as ^I
  -u                       (ignored)
  -v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB
      --help     display this help and exit
      --version  output version information and exit
```

### 1、查看a.txt文件

```
cat a.txt
```

### 2、查看a.txt并将所有行进行编号

```shell
cat -n a.txt
```

### 3、查看a.txt并将所有非空的行进行编号

```shell
cat -b a.txt
```

## 十、more和less

more和less的功能和cat有些类似，都是去读取文件的内容，more说实话有些鸡肋，也是直接就把所有的文件内容直接就读出来了，而且不支持往回翻页，所以在用的时候常常用less

### 1、查看a.txt文件

```
less a.txt
```

### 2、查看进行的时候分页显示

```shell
ps -ef |less
```

## 十一、head和tail

head和tail命令经常的被用到，含义和本身的英文释义及其相似，head是从头开始显示，tail是从末尾开始显示，默认都是显示10行

### 1、查看a.txt的末尾20行

```shell
tail -20 a.txt
```

## 十二、which

which命令被用来查找命令的的实际位置，会在$PATH中查找，所有参数如下

```shell
  --version, -[vV] Print version and exit successfully.
  --help,          Print this help and exit successfully.
  --skip-dot       Skip directories in PATH that start with a dot.
  --skip-tilde     Skip directories in PATH that start with a tilde.
  --show-dot       Don't expand a dot to current directory in output.
  --show-tilde     Output a tilde for HOME directory for non-root.
  --tty-only       Stop processing options on the right if not on tty.
  --all, -a        Print all matches in PATH, not just the first
  --read-alias, -i Read list of aliases from stdin.
  --skip-alias     Ignore option --read-alias; don't read stdin.
  --read-functions Read shell functions from stdin.
  --skip-functions Ignore option --read-functions; don't read stdin.
```

### 1、查找ls命令的路径

```shell
which ls
```

## 十三、whereis

在Linux中，whereis被用来查找二进制文件，和which不同的是，Linux中所有的二进制文件都会保存在一个数据库文件里，所以检测起来比较的方便，不局限于环境变量中

```shell
whereis ls
whereis nginx
```

## 十四、find

find命令常用来查找文件系统中的文件位置，find没有太多的参数，也可以说是有特别多的参数，使用起来比较复杂，而且应用场景比较多，所以还是很值得花时间和精力去学习find命令的使用的











