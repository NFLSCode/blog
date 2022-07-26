---
title: 'Polyquine —— 同时在多种语言运行的自生成程式'
categories: [notes]
tags: [Cpp,Python]
toc_level: 5
date: 2022-07-19 21:17:33
---

以下源代码遵循 CC BY-NC-ND 4.0 开源协议。

Quine AC记录（初版）：

[C++14](https://hydro.ac/d/zcoj/record/62cd7d02e4874952a0851613) [Python3](https://hydro.ac/d/zcoj/record/62cd7d18e4874952a0851624)

以后打算写一下原理。各版本原理不同，都打算写一下。

感谢 @[GlaceonVGC](/user/538464) 的帮助，没有他该程序不可能实现。

<!--more-->

下面有过去的版本。

ver 8（2warning，gnu++ only）

```python
#import<cstdio>/*
'''*/
main(){auto/*'''
def printf(a,*b):print(a%b,end='')#*/
_="#import<cstdio>/*%c'''*/%cmain(){auto/*'''%cdef printf(a,*b):print(a%%b,end='')#*/%c_=%c%s%c;printf(_,10,10,10,10,34,_,34,10,10,10);%c#/*%c{#*/%c}";printf(_,10,10,10,10,34,_,34,10,10,10);
#/*
{#*/
}
```
```cpp
#import<cstdio>/*
'''*/
main(){auto/*'''
def printf(a,*b):print(a%b,end='')#*/
_="#import<cstdio>/*%c'''*/%cmain(){auto/*'''%cdef printf(a,*b):print(a%%b,end='')#*/%c_=%c%s%c;printf(_,10,10,10,10,34,_,34,10,10,10);%c#/*%c{#*/%c}";printf(_,10,10,10,10,34,_,34,10,10,10);
#/*
{#*/
}
```

ver 7（采用双语共用而非隔离，1282->301bytes，2warning，gnu++ only）

因为要求所有字符必须完全一致，代码结尾有一空行。

因为格式不同，该版本提供双语高亮。
```python
#import/*
'''*/<cstdio>
main(){char*/*#'''
def printf(a,*b):print(a%b)#*/
_="#import/*%c'''*/<cstdio>%cmain(){char*/*#'''%cdef printf(a,*b):print(a%%b)#*/%c_=%c%s%c;printf(_,10,10,10,10,34,_,34,10,10,10,10);%c#/*%c'''*/%c}//'''%c";printf(_,10,10,10,10,34,_,34,10,10,10,10);
#/*
'''*/
}//'''

```
py/cpp
```cpp
#import/*
'''*/<cstdio>
main(){char*/*#'''
def printf(a,*b):print(a%b)#*/
_="#import/*%c'''*/<cstdio>%cmain(){char*/*#'''%cdef printf(a,*b):print(a%%b)#*/%c_=%c%s%c;printf(_,10,10,10,10,34,_,34,10,10,10,10);%c#/*%c'''*/%c}//'''%c";printf(_,10,10,10,10,34,_,34,10,10,10,10);
#/*
'''*/
}//'''

```

ver 6（2warning）：

```python
#if 0
n,q,v,a,b,s=chr(10),"'"*3,'','#define pass int main(){auto p=R"(','",c="#if 0%c%s%c#endif%c#import<cstdio>%c#define pass int main(){auto p=R%c(%s)%c,c=%c%s%c;printf(c,10,p,10,10,10,34,p,34,34,c,34,10);}%cpass";printf(c,10,p,10,10,10,34,p,34,34,c,34,10);}','''%sn,q,v,a,b,s=chr(10),"'"*3,'','%s','%s',%s;print(s%%('#if 0'+n,a,b,q+s+q,n+f'#endif{n}#import<cstdio>{n+a+s%%(v,a,b,q+s+q,v)}){b+n}pass'),end=v)%s''';print(s%('#if 0'+n,a,b,q+s+q,n+f'#endif{n}#import<cstdio>{n+a+s%(v,a,b,q+s+q,v)}){b+n}pass'),end=v)
#endif
#import<cstdio>
#define pass int main(){auto p=R"(n,q,v,a,b,s=chr(10),"'"*3,'','#define pass int main(){auto p=R"(','",c="#if 0%c%s%c#endif%c#import<cstdio>%c#define pass int main(){auto p=R%c(%s)%c,c=%c%s%c;printf(c,10,p,10,10,10,34,p,34,34,c,34,10);}%cpass";printf(c,10,p,10,10,10,34,p,34,34,c,34,10);}','''%sn,q,v,a,b,s=chr(10),"'"*3,'','%s','%s',%s;print(s%%('#if 0'+n,a,b,q+s+q,n+f'#endif{n}#import<cstdio>{n+a+s%%(v,a,b,q+s+q,v)}){b+n}pass'),end=v)%s''';print(s%('#if 0'+n,a,b,q+s+q,n+f'#endif{n}#import<cstdio>{n+a+s%(v,a,b,q+s+q,v)}){b+n}pass'),end=v))",c="#if 0%c%s%c#endif%c#import<cstdio>%c#define pass int main(){auto p=R%c(%s)%c,c=%c%s%c;printf(c,10,p,10,10,10,34,p,34,34,c,34,10);}%cpass";printf(c,10,p,10,10,10,34,p,34,34,c,34,10);}
pass
```


ver 5（1786bytes，初版5000多bytes，1waring）：


```python
#if 0
n,q,c,d,e,s=chr(10),"'"*3,"""#define pass int main(){const char n=10,*s=R"!(""","""",*r=R"*(printf("#if 0%c%s%c#endif%c#include<cstdio>%c#define pass int main(){const char n=10,*s=R%c!(%s)%c%c,*r=R%c*(%s)%c%c;%s}%cpass%c",n,s,n,n,n,34,s,33,34,34,r,42,34,r,n,n);)*";printf("#if 0%c%s%c#endif%c#include<cstdio>%c#define pass int main(){const char n=10,*s=R%c!(%s)%c%c,*r=R%c*(%s)%c%c;%s}%cpass%c",n,s,n,n,n,34,s,33,34,34,r,42,34,r,n,n);}""",'#include<cstdio>','''n,q,c,d,e,s=chr(10),"'"*3,"""%s""","""%s""",'%s',%s%s%s;s=s%%(c,d,e,q,s,q);print('#if 0'+n+s+n+'#endif'+n+e+n+c+s+')'+'!'+d+n+'pass')''';s=s%(c,d,e,q,s,q);print('#if 0'+n+s+n+'#endif'+n+e+n+c+s+')'+'!'+d+n+'pass')
#endif
#include<cstdio>
#define pass int main(){const char n=10,*s=R"!(n,q,c,d,e,s=chr(10),"'"*3,"""#define pass int main(){const char n=10,*s=R"!(""","""",*r=R"*(printf("#if 0%c%s%c#endif%c#include<cstdio>%c#define pass int main(){const char n=10,*s=R%c!(%s)%c%c,*r=R%c*(%s)%c%c;%s}%cpass%c",n,s,n,n,n,34,s,33,34,34,r,42,34,r,n,n);)*";printf("#if 0%c%s%c#endif%c#include<cstdio>%c#define pass int main(){const char n=10,*s=R%c!(%s)%c%c,*r=R%c*(%s)%c%c;%s}%cpass%c",n,s,n,n,n,34,s,33,34,34,r,42,34,r,n,n);}""",'#include<cstdio>','''n,q,c,d,e,s=chr(10),"'"*3,"""%s""","""%s""",'%s',%s%s%s;s=s%%(c,d,e,q,s,q);print('#if 0'+n+s+n+'#endif'+n+e+n+c+s+')'+'!'+d+n+'pass')''';s=s%(c,d,e,q,s,q);print('#if 0'+n+s+n+'#endif'+n+e+n+c+s+')'+'!'+d+n+'pass'))!",*r=R"*(printf("#if 0%c%s%c#endif%c#include<cstdio>%c#define pass int main(){const char n=10,*s=R%c!(%s)%c%c,*r=R%c*(%s)%c%c;%s}%cpass%c",n,s,n,n,n,34,s,33,34,34,r,42,34,r,n,n);)*";printf("#if 0%c%s%c#endif%c#include<cstdio>%c#define pass int main(){const char n=10,*s=R%c!(%s)%c%c,*r=R%c*(%s)%c%c;%s}%cpass%c",n,s,n,n,n,34,s,33,34,34,r,42,34,r,n,n);}
pass

```

第5版代码彻底重构，长度4300->1786


ver 4:
```python
#if 0
2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%s#include <iostream>'%n,"""#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%s","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";)*";std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";"""%e,'#define os }%simport os'%n,'''%s%s2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%%s#include <iostream>'%%n,"""#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%%s","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";)*";std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";"""%%e,'#define os }%%simport os'%%n,%s%s%s%st=s%%(a,n,q,s,q,h,h,n,b,n,c,s%%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l)%sprint(t,end='')%s%s%s%s%s%s%s%s''';t=s%(a,n,q,s,q,h,h,n,b,n,c,s%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l);print(t,end='')
#endif
#include <iostream>
#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%s#include <iostream>'%n,"""#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%s","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";)*";std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";"""%e,'#define os }%simport os'%n,'''%s%s2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%%s#include <iostream>'%%n,"""#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%%s","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";)*";std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";"""%%e,'#define os }%%simport os'%%n,%s%s%s%st=s%%(a,n,q,s,q,h,h,n,b,n,c,s%%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l)%sprint(t,end='')%s%s%s%s%s%s%s%s''';t=s%(a,n,q,s,q,h,h,n,b,n,c,s%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l);print(t,end=''))!","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";)*";std::cout<<(std::string)p[0]+n+p[1]+n+p[2]+n+p[3]+n+p[4]+z+"R"+m+"!("+p[1]+")"+"!"+m+x+",*z=R"+m+"^("+z+")^"+m+",*x=R"+m+"&("+x+")&"+m+",*r=R"+m+"*("+r+")"+"*"+m+";"+r+n+"#define os }"+n+"import os";
#define os }
import os
```


ver 3:
```python
#if 0
2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%s#include <iostream>'%n,"""#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%s","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";"""%e,'#define os }%simport os'%n,'''%s%s2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%%s#include <iostream>'%%n,"""#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%%s","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";"""%%e,'#define os }%%simport os'%%n,%s%s%s%st=s%%(a,n,q,s,q,h,h,n,b,n,c,s%%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l)%sprint(t,end='')%s%s%s%s%s%s%s%s''';t=s%(a,n,q,s,q,h,h,n,b,n,c,s%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l);print(t,end='')
#endif
#include <iostream>
#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%s#include <iostream>'%n,"""#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%s","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";"""%e,'#define os }%simport os'%n,'''%s%s2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%%s#include <iostream>'%%n,"""#define import int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%%s","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";"""%%e,'#define os }%%simport os'%%n,%s%s%s%st=s%%(a,n,q,s,q,h,h,n,b,n,c,s%%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l)%sprint(t,end='')%s%s%s%s%s%s%s%s''';t=s%(a,n,q,s,q,h,h,n,b,n,c,s%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l);print(t,end=''))!","#endif","#include <iostream>","#define import int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <iostream>","#define import int main(){"})&",*r=R"*(std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";std::cout<<p[0]<<n<<p[1]<<n<<p[2]<<n<<p[3]<<n<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";
#define os }
import os
```

ver 2:

```python
#if 0
2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%s#include <bits/stdc++.h>'%n,"""#define import using namespace std;int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%s","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";"""%e,'#define os }%simport os'%n,'''%s%s2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%%s#include <bits/stdc++.h>'%%n,"""#define import using namespace std;int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%%s","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";"""%%e,'#define os }%%simport os'%%n,%s%s%s%st=s%%(a,n,q,s,q,h,h,n,b,n,c,s%%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l)%sprint(t,end='')%s%s%s%s%s%s%s%s''';t=s%(a,n,q,s,q,h,h,n,b,n,c,s%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l);print(t,end='')
#endif
#include <bits/stdc++.h>
#define import using namespace std;int main(){const char n=10,m=34,*p[]{"#if 0",R"!(2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%s#include <bits/stdc++.h>'%n,"""#define import using namespace std;int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%s","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";"""%e,'#define os }%simport os'%n,'''%s%s2//1;n,e=chr(10),'!';q,h,v,a,b,c,d,l,s="'"*3,';','','#if 0','#endif%%s#include <bits/stdc++.h>'%%n,"""#define import using namespace std;int main(){const char n=10,m=34,*p[]{"#if 0",R"!(""",""")%%s","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";"""%%e,'#define os }%%simport os'%%n,%s%s%s%st=s%%(a,n,q,s,q,h,h,n,b,n,c,s%%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l)%sprint(t,end='')%s%s%s%s%s%s%s%s''';t=s%(a,n,q,s,q,h,h,n,b,n,c,s%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l);print(t,end=''))!","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char n=10,m=34,*p[]{"#if 0",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";)*";for(int i=0;i<4;++i)cout<<p[i]<<n;cout<<p[4]<<z<<"R"<<m<<"!("<<p[1]<<")"<<"!"<<m<<x<<",*z=R"<<m<<"^("<<z<<")^"<<m<<",*x=R"<<m<<"&("<<x<<")&"<<m<<",*r=R"<<m<<"*("<<r<<")"<<"*"<<m<<";"<<r<<n<<"#define os }"<<n<<"import os";
#define os }
import os
```

ver 1:
```python
#define ppip
#ifndef ppip
2//1;n,e=chr(10),chr(33);q,h,v,a,b,c,d,l,s=chr(39)*3,chr(59),'','#define ppip%s#ifndef ppip'%n,'#endif%s#include <bits/stdc++.h>'%n,"""#define import using namespace std;int main(){const char* p[]{"#define ppip","#ifndef ppip",R"!(""",""")%s","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char* p[]{"#define ppip","#ifndef ppip",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";)*";cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";"""%e,'#define os }%simport os'%n,'''%s%s2//1;n,e=chr(10),chr(33);q,h,v,a,b,c,d,l,s=chr(39)*3,chr(59),'','#define ppip%%s#ifndef ppip'%%n,'#endif%%s#include <bits/stdc++.h>'%%n,"""#define import using namespace std;int main(){const char* p[]{"#define ppip","#ifndef ppip",R"!(""",""")%%s","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char* p[]{"#define ppip","#ifndef ppip",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";)*";cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";"""%%e,'#define os }%%simport os'%%n,%s%s%s%st=s%%(a,n,q,s,q,h,h,n,b,n,c,s%%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l)%sprint(t,end='')%s%s%s%s%s%s%s%s''';t=s%(a,n,q,s,q,h,h,n,b,n,c,s%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l);print(t,end='')
#endif
#include <bits/stdc++.h>
#define import using namespace std;int main(){const char* p[]{"#define ppip","#ifndef ppip",R"!(2//1;n,e=chr(10),chr(33);q,h,v,a,b,c,d,l,s=chr(39)*3,chr(59),'','#define ppip%s#ifndef ppip'%n,'#endif%s#include <bits/stdc++.h>'%n,"""#define import using namespace std;int main(){const char* p[]{"#define ppip","#ifndef ppip",R"!(""",""")%s","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char* p[]{"#define ppip","#ifndef ppip",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";)*";cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";"""%e,'#define os }%simport os'%n,'''%s%s2//1;n,e=chr(10),chr(33);q,h,v,a,b,c,d,l,s=chr(39)*3,chr(59),'','#define ppip%%s#ifndef ppip'%%n,'#endif%%s#include <bits/stdc++.h>'%%n,"""#define import using namespace std;int main(){const char* p[]{"#define ppip","#ifndef ppip",R"!(""",""")%%s","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char* p[]{"#define ppip","#ifndef ppip",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";)*";cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";"""%%e,'#define os }%%simport os'%%n,%s%s%s%st=s%%(a,n,q,s,q,h,h,n,b,n,c,s%%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l)%sprint(t,end='')%s%s%s%s%s%s%s%s''';t=s%(a,n,q,s,q,h,h,n,b,n,c,s%(v,v,q,s,q,h,h,v,v,v,v,v,v,v,v),d,n,l);print(t,end=''))!","#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"},*z=R"^(const char* p[]{"#define ppip","#ifndef ppip",)^",*x=R"&(,"#endif","#include <bits/stdc++.h>","#define import using namespace std;int main(){"})&",*r=R"*(cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";)*";cout<<p[0]<<endl<<p[1]<<endl<<p[2]<<endl<<p[3]<<endl<<p[4]<<endl<<p[5]<<z<<"R"<<char(34)<<"!("<<p[2]<<")"<<"!"<<char(34)<<x<<",*z=R"<<char(34)<<"^("<<z<<")^"<<char(34)<<",*x=R"<<char(34)<<"&("<<x<<")&"<<char(34)<<",*r=R"<<char(34)<<"*("<<r<<")"<<char(42)<<char(34)<<";"<<r<<char(10)<<"#define os }"<<endl<<"import os";
#define os }
import os
```