Title: 用pandoc-markdown，像写word一样写pdf
Date: 2014-3-29
Tags: pandoc, markdown
Slug: pandoc_markdown_pdf

这篇文章要实现的效果是你在记事本或者随便其他什么编辑器里面写markdown，然后通过一个简单的指令直接生成一份优雅的pdf文档。

接下来将要涉及以下话题：

- markdown，如果你还不知道的话，可以参考我的这篇：[为什么应该用markdown写作]({filename}../latex/whyusemarkdown.md)
- pandoc，这是一个近乎万能的文档格式转换软件，具体的用法下面详细介绍
- latex，是一种标记语言以及生成pdf方式，如果你没听说过，或者不知道该如何处理中文，可以参考我这篇：[在ubuntu上编译中文latex文档]({filename}../latex/texchineseonlinux.md)

#但是，为什么是pdf

我在[为什么应该用markdown写作]({filename}../latex/whyusemarkdown.md)中提到微软的`.doc`不是一种理想的格式，同时，我也不认为pdf就更优秀，但我仍旧有足够的理由选择pdf：

- 逼格当真高
- 处理得当的话效果非常优雅
- 是出版业，论文圈里的绝对标准
- 数学公式，希腊字母的良好支持

当然，因为latex本身极其繁琐恼人，很少有人真的直接手写latex源文件然后去编译pdf的，事实上如果你像我一样用latex写过一学期的场论作业，就会真切的明白为什么这样干逼格高了。

#那么，能不能不干脏活又效果优雅呢

当然可以！我写这篇文章就是为了这个。

其实很长一段时间里我是能不碰latex-pdf就不碰的，倒是把markdown当成日常标准每天都在用。很难说是谁联系上的谁，但当我看到pandoc的时候我明白了，整个生态链的最后一环找到了，此后便是神器在手。

思路也很简单，先用markdown写文档，再用pandoc转换输出成pdf，兼顾markdown的清新易用和latex的强大支持。

下面就看看我是怎么配置的。

*注：我的生产环境为ubunutu，下面的配置都以此为标准，我肯定windows下可以达成同样的效果，但不保证配置同样简单*

#搞好latex

要编译生成pdf没有latex是不行的。win用户直接用ctex，一个打包就齐活了，linux用户则推荐使用texlive的后台，为了能处理中文可以参考[在ubuntu上编译中文latex文档]({filename}../latex/texchineseonlinux.md)。

其实不管怎么样，保证你电脑上能编译通过中文文档就行。

#安装配置pandoc

##安装

[这里](http://code.google.com/p/pandoc/downloads/list)有下载列表，找一个合适的。

ubuntu下面可以直接sudo安装，如果需要beamer的支持，请参考[这里](http://yanping.me/cn/blog/2012/03/13/pandoc/)。

##配置

其实上面把pandoc装好，latex环境配置完成就可以使用了，但为了支持中文还得做一点小改动。

在完成markdown向pdf转换的过程中，pandoc首先套用内置的`.tex`模板将markdown文档转换成tex源文件，再调用相应的latex引擎编译成pdf。若想搞定对中文的支持，还得从这个模板入手。

我的机器上模板的位置在

    :::bash
    /usr/share/pandoc-1.10.1/data/templates/default.latex

我这里有一份[修改过的模板](https://gist.github.com/yangsan/9686706)，如果不嫌弃的话可以拿去用。为了能正常使用，请确认自己texlive安装正确，并且字体也搞定了。`\setCJKmainfont`那一项上我用了微软雅黑，如果你没装的话就换一个。

##命令

以上的所有配置完成以后就可以使用了，将markdown转换成pdf的命令如下：

    :::bash
    pandoc hello.md --latex-engine xelatex -o hello.pdf

其中，"hello.md"是源文件，"-o hello.pdf"表示目标文件为hello.pdf，"--latex-engine xelatex"表示使用xelatex引擎进行编译。

当然这只是最基本的命令，还可以通过添加参数的形式对编译结果进行定制，具体的参数可以参考[官方指南](http://johnmacfarlane.net/pandoc/README.html)。

下面我罗列两个自己常用的参数：

- -N 大小标题标上序号
- --toc 添加目录

##makefile

当然，每次编译文档都要重新输入这么一长串命令也太傻了，所以我写了一个makefile来做这件事情。

    :::make
    FLAG= -N --toc #如果需要其他的参数，可以添加到这后面

    %.pdf : %.md
        pandoc %(FLAG) --latex-engine xelatex -o $@ $<

    .PHONY: clean

    clean:
        rm -f *.pdf

把上面的内容写到一个叫"Makefile"的文件里（一定要把里面的tab改成真tab，直接拷下来可能是四个空格，会出bug），放在你的markdown源文件目录下面，假设你的源文件名叫"test.md"，那么你只需要

    :::bash
    make test.pdf

make会自动检测当前的目录，找到对应的源文件，并将其编译成pdf。

#pandoc的markdown扩展

markdown是极简主义的产物，对付日常写作还能胜任，但如果你需要数学公式或者文献引用的支持就无能为力了。

pandoc既然支持向pdf的转换，自然要解决这个问题，其方法是对markdown语法做出一些扩展。这当然增加了使用的门槛，但却是为了获得更多特性不得不做出的牺牲，结果在我看来则是完全可以接受的。

pandoc-markdown的语法说明在其[官网](http://johnmacfarlane.net/pandoc/README.html)，有热心网友已经翻译了[一份](http://pages.tzengyuxio.me/pandoc/)出来供参考。

下面列举我常用的扩展，稍作说明。

##数学公式

我经常用到的有两种，一种包含在一对`$`中，如`$\rho$`用在行间，另一种包含在一对`$$`中，如`$$\gamma = \frac{1}{2 \pi}$$`则表示独立的公式行。

上面这些标记直接扔进markdown源文件里，编译之后就变成了优美的样子了。

##插入图片和图片的交叉引用

插入图片的一般格式为

    :::tex
    ![image](./pic/fig1.png)

引用图片时则可以加上label

    :::tex
    见图(\ref{fig2})
    blah blah ...

    ![image \label{fig2}](./pic/fig2.png)

##引用

pandoc对引用的支持也相当不错。我的话先用文献管理软件生成一个BibLaTex的文件，文章中需要引用的地方就用`@`加上BibLaTex文件中的引用id，如

    :::tex
    Blah blah [@john2007].
    @joe1998 said blah blah.

此外命令中也要加上一个参数

    :::bash
    --biblio citation.bib

"citatiion.bib"就是刚生成的BibLaTex文件。

编译完成后相关的参考文献也会被列在文档的最后，所以建议在有引用的情况下在文章的最后加上

    :::markdown
    #Reference