对一组文件中的每一个文件执行某个特定命令。

FOR %variable IN (set) DO command [command-parameters]

  %variable  指定一个单一字母可替换的参数。
  (set)      指定一个或一组文件。可以使用通配符。
  command    指定对每个文件执行的命令。
  command-parameters
             为特定命令指定参数或命令行开关。

在批处理程序中使用 FOR 命令时，指定变量请使用 %%variable
而不要用 %variable。变量名称是区分大小写的，所以 %i 不同于 %I.

如果启用命令扩展，则会支持下列 FOR 命令的其他格式:

FOR /D %variable IN (set) DO command [command-parameters]

    如果集中包含通配符，则指定与目录名匹配，而不与文件名匹配。

FOR /R [[drive:]path] %variable IN (set) DO command [command-parameters]

    检查以 [drive:]path 为根的目录树，指向每个目录中的 FOR 语句。
    如果在 /R 后没有指定目录规范，则使用当前目录。如果集仅为一个单点(.)字符，
    则枚举该目录树。

FOR /L %variable IN (start,step,end) DO command [command-parameters]

    该集表示以增量形式从开始到结束的一个数字序列。因此，(1,1,5)将产生序列
    1 2 3 4 5，(5,-1,1)将产生序列(5 4 3 2 1)

FOR /F ["options"] %variable IN (file-set) DO command [command-parameters]
FOR /F ["options"] %variable IN ("string") DO command [command-parameters]
FOR /F ["options"] %variable IN ('command') DO command [command-parameters]

    或者，如果有 usebackq 选项:

FOR /F ["options"] %variable IN (file-set) DO command [command-parameters]
FOR /F ["options"] %variable IN ("string") DO command [command-parameters]
FOR /F ["options"] %variable IN ('command') DO command [command-parameters]

    fileset 为一个或多个文件名。继续到 fileset 中的下一个文件之前，
    每份文件都被打开、读取并经过处理。处理包括读取文件，将其分成一行行的文字，
    然后将每行解析成零或更多的符号。然后用已找到的符号字符串变量值调用 For 循环。
    以默认方式，/F 通过每个文件的每一行中分开的第一个空白符号。跳过空白行。
    你可通过指定可选 "options" 参数替代默认解析操作。这个带引号的字符串包括一个
    或多个指定不同解析选项的关键字。这些关键字为:

        eol=c           - 指一个行注释字符的结尾(就一个)
        skip=n          - 指在文件开始时忽略的行数。
        delims=xxx      - 指分隔符集。这个替换了空格和制表符的
                          默认分隔符集。
        tokens=x,y,m-n  - 指每行的哪一个符号被传递到每个迭代
                          的 for 本身。这会导致额外变量名称的分配。m-n
                          格式为一个范围。通过 nth 符号指定 mth。如果
                          符号字符串中的最后一个字符星号，
                          那么额外的变量将在最后一个符号解析之后
                          分配并接受行的保留文本。
        usebackq        - 指定新语法已在下类情况中使用:
                          在作为命令执行一个后引号的字符串并且一个单
                          引号字符为文字字符串命令并允许在 file-set
                          中使用双引号扩起文件名称。