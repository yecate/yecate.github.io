## 简介
易语言内联汇编器(InlineAssembler)是一个易语言扩展支持库, 用于在易语言中嵌入汇编语言指令, 支持x86/x64, 支持NASM宏, 媲美C++内联汇编.

---

## 特性
- 汇编代码补全提示
- 汇编代码语法高亮
- 汇编代码鼠标悬停提示指令帮助
- 支持易语言模块编译
- 支持汇编代码引用变量/参数/全局变量/常量
```
    mov eax, 参数
    mov eax, 局部变量
    mov eax, 全局变量
    mov eax, [eax + #常量]
```
- 支持汇编代码调用易语言子程序
```
    call 子程序     ; 调用子程序
    mov eax, 子程序 ; 取子程序地址
```
- 支持调用DLL函数
```
    call user32.dll.MessageBoxA     ; 调用MessageBoxA
    mov eax, user32.dll.MessageBoxA ; 取MessageBoxA地址
```
- 支持内联汇编中使用文本字符串 gbk/utf8/unicode
```
    mov eax, "文本"   ; GBK
    mov eax, u8"文本" ; UTF8
    mov eax, L"文本"  ; UNICODE
```
- invoke 调用
```
    invoke 子程序, 1, 2, 3                                  ; 默认 __stdcall 调用约定
    invoke __stdcall 子程序, 1, 2, 3                        ; __stdcall 调用约定
    invoke __cdecl 子程序, 1, 2, 3                          ; __cdecl 调用约定, 自动清理堆栈
    invoke __fastcall 子程序, 1, 2, 3                       ; __fastcall 调用约定
    invoke __thiscall 子程序, _this, 1, 2, 3                ; __thiscall 调用约定
    invoke user32.dll.MessageBoxA, 0, "内容", "标题", 0     ; DLL 函数
```
- 支持 _naked/_cdecl/_removepack 修饰子程序
- 支持 IDE 断点调试, 支持单步跟踪进入子程序
- 支持 x86/x64
```
    bits 32
    mov eax,ecx

    bits 64
    mov rax,rcx
```
- NASM内核
- 集成NASMX
```
    %use nasmx
```
---

## 联系方式
联系作者: [QQ:869443499](http://wpa.qq.com/msgrd?v=3&uin=869443499&site=125.la&Menu=yes)

交流群号: [Q群:767562242](https://jq.qq.com/?_wv=1027&k=6UMShpJb)

有任何想法和意见都可以进群反馈

---

## InlineAssembler 更新历史
```
v3.25
	1.修正如果/判断代码分支标签跳转错误问题(严重问题)
	2.修正屏蔽/恢复汇编代码BUG
	3.修正显示代码调试信息在有缩略块/打开参数表的情况下错位问题
	4.修正 invoke __cdecl 函数 无参数时 出现 add esp, 0 问题
	5.修正置入代码转汇编时标签大小写问题
	6.增加参数是否不为空的符号识别(cmp 参数_是否不为空, 1)
	7.更改右键菜单为二级菜单
```
---
```
v3.2 20230501
    1.修正在易语言循环体内标签定位错位问题(严重问题)
	2.修正部分字符串定位错误问题
	3.增加鼠标悬停显示汇编代码指令提示
	  确保 指令帮助文件被正确放置在 易语言根目录InlineAssembler\db\x86.txt(机器翻译过来的,可以自行修改)

	4.增加 reladdr 关键字,仅用于获取标签真实地址( mov eax, reladdr(标签1))
	  详见 demo\reladdr.e

	5.增加 %reladdrs 宏, 仅用于存储标签真实地址表( %reladdrs 表名称, 标签1, 标签2,... 标签n)
	  详见 demo\switch.e

	6.增加 // 注释
	7.修正易语言子程序参数有通用型(非参考)参数时,导致调试时查看此通用型变量后面的变量数值错误的问题
	8.优化易语言置入代码过长导致的卡顿
	9.增加快速注释/取消注释

	10.右键菜单功能添加快捷键绑定
	  Ctrl + Ins      快速插入多行汇编
      Ctrl + Atl + L  变量初始化
	  Ctrl + Atl + N  变量初始化(无%define)
	  Ctrl + K        屏蔽选中汇编代码
	  Ctrl + M        取消屏蔽选中汇编代码

	11.增强汇编代码调试功能,支持单步跟踪进入 跳转/CALL

	12.增加 %append_code 宏; 语法: %append_code xxxx; 支持自定义插入代码到子程序头部/尾部,方便快速插入各种壳保护标记
	   详见 demo\插入自定义代码到子程序头尾.e
	   插入的代码(字节集) 定义在 易语言目录\InlineAssembler\append_code.json

	   头部代码
	   push ebp
	   mov ebp,esp
	   ....
	   mov esp,ebp
	   pop ebp
	   ret
	   尾部代码
```
---
```
v3.1 20230329 测试版
	1.修正代码缩略/展开参数表识别错误问题
	2.修正基础命令重定位失败问题
	3.修正条件宏是否存在类似命令 编译代码错位问题

	4.增强模块和置入代码功能,智能识别变量/参数,无需手动 %define,
	  最好变量先初始化(可配合右键菜单 ☆内联汇编:变量初始化(无%define))
	  避免调试输出此类命令先于变量初始化,导致编译和调试变量不统一

	5.集成 nasmx(%use nasmx) 如果当前子程序不需要用到高级宏命令,可以不添加 %use nasmx,提高编译速度
	  确保 nasmx.inc被正确放置在 易语言根目录\InlineAssembler\nasmx.inc
	  详见 demo\nasm-x\nasm-x.e

	6.增加代码提示
	7.增加回车自动加注释分号
```
---
```
v3.0 20230319 测试版
	1.更换底层汇编引擎为NASM
	2.增加宏支持(来自NASM)
	  详见demo\宏.e

	3.增加x64汇编支持(来自NASM)
	  详见demo\x64.e

	4.修改DLL命令,增加DLL命令前缀设置,nasm宏定义支持 . 号,所有为了避免代码歧义
	  假设设置 DLL命令前缀 为 API_,则调用API书写格式为 API_user32.dll.MessageBoxA,留空则为 user32.dll.MessageBoxA
      详见demo\DLL命令前缀设置.e

	5.增加模块编译方式,支持编译模块内联汇编为置入代码
	    编译模块之后不需要再配合本支持库使用
	    编译模块不能使用变量/调用子程序/不能跨易语言代码JMP/CALL等等
	    详见demo\模块\方式二

	6.增加IDE右键菜单
	    内联汇编转置入代码(当前子程序)
	    内联汇编转置入代码(当前程序集内所有子程序)
	    变量顺序初始化
	    快速插入多行汇编
		  '内联汇编关键字{
		  '}

	7.移除禁止生成包装函数设置选项,增加_removepack关键字修饰子程序/程序集为禁止生成包装函数
	8.增加函数名称显示高亮(_naked/_cdecl/_removepack)
	9.增加汇编命令大写显示
	10.支持调试模式鼠标悬停寄存器查看寄存器值

	11.增加模块混淆
	    模块编译方式二,可以选择开启混淆类成员变量名称,混淆未公开函数名称
		可以自定义混淆方式,参考 InlineAssembler\obfuscate.e
		编译成DLL 放入 易语言主目录\InlineAssembler\obfuscate.dll 即可

	12.修复若干BUG
```
---
```
v2.1 20221004
    1.新增支持调用DLL函数(详情:demo\调用DLL(invoke)\exe.e)
	    ' push 0
		' push L"UNICODE文本标题"
		' push L"UNICODE文本内容"
		' push 0
		' call user32.MessageBoxW

		' ; 序号
		' push 8888
		' call ws2_32.#14

	2.新增invoke(小写)关键字,方便调用函数和DLL函数
		' invoke user32.MessageBoxW, 0, L"invoke UNICODE文本内容", L"invoke UNICODE文本标题", 0

	3.invoke 调用方式支持 __stdcall/__cdecl/__fastcall 关键字
	    ' invoke __cdecl @分配内存, 260 ; __cdecl 根据参数数量自动平衡堆栈
	    ' invoke kernel32.GetTickCount  ; 默认 __stdcall 方式

	4.修正部分高亮显示不正常
	5.修正部分常量识别不正确
```
---
```
v2.0 20220816 测试版
    1.新增支持内联汇编字符串 (gbk utf8 unicode)
	 ' push "gbk"
	 ' push L"unicode"
	 ' push u8"utf8"

	2.新增支持内联汇编常量
	3.新增支持naked函数不检查返回值(错误(10022): 子程序“XXX”具有返回值定义，但实际上却没有返回数据或者并不是所有程序分支都返回了数据。)
	4.新增支持汇编代码高亮
	5.修正对易语言5.92版本的支持
	6.修复若干BUG
```
---
```
v1.2 20220710
    1.修正和精易助手支持库不兼容问题(感谢 @不苦小和尚 反馈)
	2.修正子程序代码过长导致出现生成错误代码的问题(感谢 @max 反馈)
	3.新增支持多行汇编
```
---
```
v1.1 20220702
    1.新增支持模块编译
	2.新增增加自定义内联汇编关键字(留空则默认易语言注释文本为内联汇编代码)
	3.新增支持易语言版本 5.80 - 5.93
	4.新增禁止易语言生成函数外包装代码
	5.新增程序集名称开头若为 _naked_/_cdecl_ 则表示此程序集的函数全部为 _naked_/_cdecl_
    6.新增内联汇编中使用易语言基础命令(@分配内存/@重新分配内存/@释放内存/@设置组件属性/@读取组件属性)
	7.修复若干BUG
```
---
```
v1.0 20220620
    第一个版本
```

### 感谢您的支持，如果您觉得好用，请给我打赏一杯咖啡吧！
## 赞助列表
| 用户 | 金额|
| :----: | :----: |
| 木头人 | *** |
| TianYi | *** |
| Care | *** |
| ღ。许 先 生 | *** |
| Ling丶全职软件开发 | *** |
| max | *** |
| 落叶 知秋 | *** |
| 白银 | *** |
| lsp | *** |
| 椽木 | *** |
| *航 | *** |
| 最帅的帅哥 | *** |
| weilai(未来·科技) | *** |
| 稀饭 | *** |
|支付宝*进| *** |
|DEHBY| *** |
|微信*技| *** |
|微信 S*?| *** |
|微信 夜莫离| *** |
|QQ 千屈丶| *** |
|QQ 禅问| *** |
|QQ 515831586(飞飞团队)| *** |
|微信QQ 906352171(f)| *** |
|鱼鱼鱼鱼児、| *** |
|小星星| *** |
|水东流            ❶| *** |
|天涯(微信)| *** |
|Jiang(1362431384) 微信| *** |
|汪汪碎冰冰(349221466) 支付宝| *** |
|鸡鸡你太美| *** |
|夜未央| *** |
|钟情于| *** |
|冫氺(3071311441) | *** |
