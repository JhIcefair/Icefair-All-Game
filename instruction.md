# 目录

- [使用教程](#use)
- [具体解析](#explain)
    - [numberfinding / 数轴探宝](#numberfinding)
    - [passwordcracking / 密码破解](#passwordcracking)
    - [wateroverflow / 倒水挑战](#wateroverflow)
- [使用说明及其他你应该知道的](#youknow)

<a id="use"></a>
# 使用教程

想要在自己的电脑上使用这些游戏很简单，只需要在Visual Studio上自己新建一个新项目，然后将代码复制粘贴进去即可。

> [!NOTE]
> * 如果你问其他工具是否可以？
> 
> 至少我没用过。理论上，只要能编译运行C语言或C++代码应该都可以，不过Visual Studio应该是比较简单的一个方案，因为他会帮你准备好必要的依赖，而在像VSCode的地方，需要自行处理依赖。

<a id="explain"></a>
# 具体解析

<a id="numberfinding"></a>
## numberfinding / 数轴探宝

原名：numberboom，数字炸弹。

正如原名，该游戏是参考“数字炸弹”做的小游戏。最初的一代版本1:1还原了数字炸弹原游戏（不过是在控制台中），后来的版本增加了一些全新的功能，4.0版本开始改名“numberfinding（数轴探宝）”。

其中，一代、二代、三代、四代均为C语言代码，第五代开始改用C++。

* 简要介绍

类似于扫雷，在1-100的范围内有一个宝藏（炸弹），你的目的是找到宝藏，但要避免触发和宝藏放在一起的炸弹。中途你可能遇到各种机关，其中包括了能不让炸弹爆炸的开关。

* 具体介绍

<details>
    <summary>第一代-点击展开</summary>

* 具体说明

该版本为纯“猜数字”。

* 代码解析

    - 包含参数

    int a：目标随机数

    int b：可变数字

    int k：游戏结束后稳定窗口符

    - 工作逻辑

    1、生成一个“目标随机数”作为“炸弹”并为参数a赋值，范围1-100。

    2、开始while循环，判断输入的值（赋值给参数b）是否与目标值（参数a）相同：

        2.1、若相同则循环结束，输出结果；

        2.2、若不同则进行大小判断。

    - 流程图展示
 
    ```mermaid

    graph TD
        start([开始]) --> Randa[生成最大值100的随机数a]
        Randa --> Scanfa[输入数字赋值给b]
        Scanfa --> Whilea{"b=a"}
        Whilea -- yes --> End([游戏结束])
        Whilea -- no --> Ifa{"b>a"}
        Ifa -- yes --> Printfa[偏大]
        Ifa -- no --> Printfb[偏小]
        Printfa --> Scanfa
        Printfb --> Scanfa
    
    ```

</details>

<details>
    <summary>第二代-点击展开</summary>

* 具体说明

该版本为纯“数字炸弹”游戏复刻。

* 更新说明

该版本优化了代码逻辑，新增范围提示功能。

* 代码解析

    - 包含参数
 
    int a：目标随机数

    int b：可变数字

    int max：最大值

    int min：最小值

    int k：游戏结束后稳定窗口符

    - 工作逻辑
 
    1、生成一个“目标随机数”作为“炸弹”并为参数a赋值，范围1-100。

    2、开始do循环，判断输入的值（赋值给参数b）是否与目标值（参数a）相同：

        2.1、若相同则循环结束，输出结果；
  
        2.2、若不同则进行大小判断并根据结果为参数max和min赋值以记录范围。

    - 流程图展示
 
    ```mermaid

    graph TD
        Start([开始]) --> Randa[生成最大值100的随机数a]
        Randa --> Scanfa[输入数字赋值给b]
        Scanfa --> Ifa{"b>a"}
        Ifa -- yes --> Printfa[偏大]
        Ifa -- no --> Printfb[偏小]
        Printfa --> Ifb{"max>b"}
        Ifb -- yes --> Max[将b值赋值给max]
        Ifb --no --> Printfc[输出范围]
        Max --> Printfc[输出范围]
        Printfb --> Ifc{"min<b"}
        Ifc -- yes --> Min[将B值赋值给min]
        Ifc -- no --> Printfc
        Min --> Printfc[输出范围]
        Printfc --> Scanfb[输入数字赋值给b]
        Scanfb --> Do{"b=a"}
        Do -- yes --> End([游戏结束])
        Do -- no --> Ifa

    ```

</details>

<details>
    <summary>第三代-点击展开</summary>

* 具体说明

该版本玩法与“数字炸弹”类似，但是增加了一些新功能，具体玩法如下：

在1-100的范围内有一个炸弹，与原游戏不同，引爆炸弹的可能是胜利者也可能是失败者。 
在排除炸弹的过程中，你可能会触发一个特殊的机关导致炸弹位置发生变化。 
特殊机关位于炸弹附近，具体在游戏初始化时会随机确定。

* 更新说明

该版本新增随机规则、特殊效果。

其中随机规则即触发炸弹的人是输还是赢是随机的； 
特殊效果即触发后可以改变炸弹位置的机关，一共有两个，发布在炸弹附近。

* 代码解析

    - 包含参数
 
    int a：目标随机数

    int p：旧炸弹位置储存器

    int b：可变数字

    int c0：特殊效果随机变量

    int c1：上效果随机数

    int c2：下效果随机数

    int g：特殊效果触发判定器

    int max：最大值

    int min：最小值

    int s：游戏规则

    int k：游戏结束后稳定窗口符
 
    - 工作逻辑
 
    1、生成随机规则代码、炸弹位置、特殊效果位置。
  
        1.1、关于随机规则代码，是一个1-10范围内的随机数，其中随机到前五则规定引爆者获胜，随机到后五则规定引爆者失败

        1.2、关于炸弹位置，在1-100范围内随机

        1.3、关于特殊效果位置，位于炸弹附近，距离随机但公开，范围1-30。
        具体如下：
            1.3.1、位于前面的机关位置是炸弹位置减范围，即a-c0，如果结果为小于1，要再加100确定最终位置。
            1.3.2、位于后面的机关位置是炸弹位置加范围，即a+c0，如果结果超过100，要再减100确定最终位置。
  
    2、游戏正式开始后，首先进行开发者代码判定，包括三个开发者代码（101、102、103）

        2.1、代码101：反馈特殊效果机关范围值，再次输入后执行continue进入下一次循环；
        2.2、代码102：强制结束游戏，通过break跳出循环，直接输出结果；
        2.3、代码103：直接重开游戏，通过goto跳转到位于开头的ICE标签，实现强制重开游戏。

    3、如果无需执行开发者代码，那么先开始进行特殊效果机关的触发判定。

        3.1、第一种情况，触发位于后面的c1，为判定器参数g赋值1的同时生成新的炸弹，新炸弹会位于小于等于旧位置（即较小数字）的区域内；
        3.2、第二种情况，触发位于前面的c2，为判定器参数g赋值2的同时生成新的炸弹，新炸弹会位于大于等于旧位置（即较大数字）的区域内；
        3.3、判定完触发情况后，重新生成新的特殊效果机关位置并重置范围参数值；
        3.4、此时可能存在一种极端情况——最大值和最小值相等即没有更多的范围了，这时需要重新在1-100的范围内生成新的炸弹包括特殊效果机关并再次重置范围参数值。

    4、由于do while函数特性的缘故，可能存在一种特殊情况——一开始就触发炸弹但是没有得到及时判定因而游戏不会立即结束，所以需要用break跳出循环强制结束。

    5、对于一般情况，进行大小判定并更新范围参数值。

    6、如果输入值与炸弹位置相同，结束游戏，进行结果判定。

    7、如果需要，可通过103代码跳转ICE标签直接重开下一局，无需关闭窗口再次打开。

    - 流程图展示
 
    ```mermaid

    flowchart TD
        Start([开始]) ==> Rands[生成决定游戏规则的最大值为10的随机数s]
        Rands --> Ifs{"s<6"}
        Ifs -- yes --> Printfs1[引爆者胜]
        Ifs -- no --> Printfs2[引爆者败]
        Rands ==> Randa[生成最大值100的随机数a]
        Randa ==> Randc0[生成特殊效果随机变量c0]
        Randc0 --> Randc1["c1=a+c0"]
        Randc0 --> Randc2["c2=a-c0"]
        Randc1 --> Ifc1{"c1的值超出范围"}
        Ifc1 -- yes --> C1["c1=c1-100"]
        Randc2 --> Ifc2{"c2的值超出范围"}
        Ifc2 -- yes --> C2["c2=c2+100"]
        Randc0 ==> Scanfb[输入数字赋值给参数b]
        Scanfb ==> Fzp[将参数a的值记录给参数p]
        Fzp ==> If101{代码101}
        If101 == no ==> If102{代码102}
        If101 -- yes --> Printf101[输出特殊效果范围]
        Printf101 --> Scanf101[输入数字赋值给参数b后进入下一个循环]
        Scanf101 --> Ifmain
        If102 -- yes --> Printf102[强制结束游戏，跳出循环]
        Printf102 --> Fend
        If102 == no ==> If103{代码103}
        If103 == no ==> Ifcc1{触发特殊效果值c1}
        If103 -- yes --> Goto[重开游戏，跳转ICE标记]
        Goto --> Start
        Ifcc1 -- yes --> Randca1[更新炸弹位置在小数字区间内，为参数g赋值1]
        Ifcc1 == no ==> Ifcc2{触发特殊效果值c2}
        Ifcc2 -- yes --> Randca2[更新炸弹位置在大数字区间内，为参数g赋值2]
        Ifcc2 == no ==> Ifg{"g>0"}
        Ifg == no ==> Ifbad{处理一开始接触炸弹不会炸的bug}
        Ifbad -- yes --> Fend
        Ifbad == no ==> Ifbam{"b>a"}
        Ifg -- yse --> Gc[按照同样的逻辑生成新的c1 c2]
        Gc --> Gifg[g值为1则新范围为1-p，g值为2则新范围为p-100]
        Gifg --> Gifm[如果最大值和最小值相等，则重新在1-100范围内生成炸弹]
        Gifm --> Gscanfb[输入数字赋值给参数b]
        Gscanfb --> Ifmain
        Ifbam == yes ==> Printfbxm[偏大]
        Printfbxm ==> IFbamax{"max>b"}
        IFbamax == no ==> Printfnx[输出当前范围]
        IFbamax == yes ==> Max[将参数b值赋值给参数max]
        Max ==> Printfnx
        Ifbam == no ==> Printfnm[偏小]
        Printfnm ==> Ifbamin{"min<b"}
        Ifbamin == yes ==> Min[将参数b值赋值给参数min]
        Min ==> Printfnx
        Ifbamin == no ==> Printfnx
        Printfnx ==> Scanfbw[输入数字赋值给参数b]
        Scanfbw ==> Ifmain{"b=a"}
        Ifmain == yes ==> Fend[游戏结束]
        Ifmain == no ==> Fzp
        Fend ==> Ifend{"s<6"}
        Ifend == yes ==> Printfends1[触发者获胜]
        Ifend == no ==> Printfends2[触发者失败]
        Printfends1 ==> Scanfk[输入数字赋值给参数k]
        Printfends2 ==> Scanfk
        Scanfk ==> Ifk{如果参数k的值为103}
        Ifk == no ==> End([结束])
        Ifk == yes ==> Start

    ```

</details>

<details>
    <summary>第四代-点击展开</summary>

* 具体说明

* 更新说明

* 代码解析

    - 包含参数
 
    - 工作逻辑
 
    - 流程图展示
 
    ```mermaid

    ···

</details>

<details>
    <summary>第五代-点击展开</summary>

* 具体说明

* 更新说明

* 代码解析

    - 包含参数
 
    - 工作逻辑
 
    - 流程图展示
 
    ```mermaid

    ···

</details>

<a id="passwordcracking"></a>
## passwordcracking / 密码破解

参考玩具“超级密码机”制作的游戏。

其中，一代、二代为C语言代码，第三代开始改用C++，除此之外，还有借助deepseek制作的网页版。

* 简要介绍

游戏会根据你设定的数字范围挑选四个数字作为密码，你需要通过多次尝试并根据反馈的结果推断出正确密码，猜出则游戏结束。

游戏包含两种难度：简单、困难。简单难度下，反馈的结果会与位置对应，你能够得知具体每一个位置的对应结果（不是、包含、正确）；与之相对应的困难难度是只会反馈你输入的四位数字包含哪些情况，具体对应什么位置需要你自己猜测分析判断。

* 具体介绍

> [!NOTE]
> 借助deepseek制作的网页版特性同第三代，故不做具体单独介绍。

<details>
    <summary>第一代-点击展开</summary>

* 具体说明

* 代码解析

    - 包含参数
 
    - 工作逻辑
 
    - 流程图展示
 
    ```mermaid

    ···

</details>

<details>
    <summary>第二代-点击展开</summary>

* 具体说明

* 更新说明

* 代码解析

    - 包含参数
 
    - 工作逻辑
 
    - 流程图展示
 
    ```mermaid

    ···

</details>

<details>
    <summary>第三代-点击展开</summary>

* 具体说明

* 更新说明

* 代码解析

    - 包含参数
 
    - 工作逻辑
 
    - 流程图展示
 
    ```mermaid

    ···

</details>

<a id="wateroverflow"></a>
## wateroverflow / 倒水挑战

参考“由多个人依次上前按按钮，一个人一次可以按多次按钮，导致与按钮连接的炸弹爆炸的人失败”这样的小游戏制作的游戏，不过把按按钮改成了倒水。

游戏包含两个C++版本和借助deepseek制作的网页版（包括人工二次微调版）。

* 简要介绍

有一个空的大水杯和一个带有刻度的小水杯，你需要用小水杯往大水杯里倒水，导致水溢出的为失败者。

* 具体介绍

> [!NOTE]
> 借助deepseek制作的网页版（包括人工二次微调版）特性同一代，故不做具体单独介绍。

<details>
    <summary>第一代-点击展开</summary>

* 具体说明

* 代码解析

    - 包含参数
 
    - 工作逻辑
 
    - 流程图展示
 
    ```mermaid

    ···

</details>

<details>
    <summary>第二代-点击展开</summary>

    已开发。

    包含新特性：初始水量。

* 具体说明

* 更新说明

* 代码解析

    - 包含参数
 
    - 工作逻辑
 
    - 流程图展示
 
    ```mermaid

    ···

</details>

<a id="youknow"></a>
# 使用说明及其他你应该知道的

该项目中包含的代码均为**净化之子凌柃Icefair**个人参考对应的小游戏独立设计开发的。

既然已经选择开源，那么我不再设置任何使用限制，你可以自由使用。

如果你想要联系我，可以在B站搜索“**净化之子凌柃**”，如果你想要在Discord、X/Twitter等的地方找到我，请认准“**@jhicefair**”！
