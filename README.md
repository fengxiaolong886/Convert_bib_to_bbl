# Convert_bib_to_bbl
记录一次生成author-year格式bbl的惨痛教训！原本前已经搞明白了，结果因为电脑重装，丢失了工程。方法被我忘得一干二净，终于又花费了一天时间搞定author-year格式的bbl。

相关文件我存放git上：[https://github.com/fengxiaolong886/Convert_bib_to_bbl](https://github.com/fengxiaolong886/Convert_bib_to_bbl)
clone地址为：
```
git clone git@github.com:fengxiaolong886/Convert_bib_to_bbl.git
```



**感谢这两篇文章的作者，你们给了我基本的处理这个问题的思路**

[https://tex.stackexchange.com/questions/54480/package-natbib-error-bibliography-not-compatible-with-author-year-citations](https://tex.stackexchange.com/questions/54480/package-natbib-error-bibliography-not-compatible-with-author-year-citations)

[https://blog.csdn.net/jsjwr/article/details/79373900](https://blog.csdn.net/jsjwr/article/details/79373900)

下面记录下详细步骤：

# 1. 获取bib

下载论文的bib并粘贴到original.bib这个文件中。

举个例子，我的bib是这样的：

```

@article{Wang2005TE,

 title={TE Routing Algorithm to Minimize Maximum Link Utilization},

 author={Wang, Xin Hong and Liu, Fu Qiang and Wang, Guang Xing},

 journal={Mini-micro Systems},

 year={2005},

}

```

# 2. 主函数生成

创建一个main.tex的文件，然后写入如下代码：

```
\documentclass[letterpaper, 10pt]{article}
\usepackage[authoryear]{natbib}
\usepackage{lipsum}
\begin{document}
\lipsum[1-4]
\nocite{*}
\bibliographystyle{apalike}
\bibliography{original}
\end{document} 
```

其中我们需要注意的是bibilography里面的文件就是上一步做的original.bib

# 3. 生成bbl

接下来是生成bbl

首先，通过WinEdt打开main.tex，点击PDFLatex运行

![image.png](https://upload-images.jianshu.io/upload_images/15463866-6b49d90613b1858d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


然后，在菜单TeX之下运行BibTeX，这样就可以生成bbl了。

生成的格式如下：

```
\begin{thebibliography}{}

\bibitem[Wang et~al., 2005]{Wang2005TE}
Wang, X.~H., Liu, F.~Q., and Wang, G.~X. (2005).
\newblock Te routing algorithm to minimize maximum link utilization.
\newblock {\em Mini-micro Systems}.

\end{thebibliography}

```

# 4. 使用bbl

在我的论文中，我把上一步生成的item拷贝出来，放到我原来的bbl模板中。写出来的格式是这样的：

```
\begin{thebibliography}{45}
\providecommand{\natexlab}[1]{#1}
\providecommand{\url}[1]{\texttt{#1}}
\expandafter\ifx\csname urlstyle\endcsname\relax
  \providecommand{\doi}[1]{doi: #1}\else
  \providecommand{\doi}{doi: \begingroup \urlstyle{rm}\Url}\fi

\bibitem[Wang et~al., 2005]{Wang2005TE}
Wang, X.~H., Liu, F.~Q., and Wang, G.~X. (2005).
\newblock Te routing algorithm to minimize maximum link utilization.
\newblock {\em Mini-micro Systems}.

\end{thebibliography}

```

在论文中，通过如下格式引用参考文献：

```
\citep{Wang2005TE}
```

# 5. 相关问题
## 一

按照原贴的方法引用的时候出现错误：

```
Bibiography not compatible with author-year cations
```

原来的main文件是这样的：

```
\documentclass[preprint,review,12pt,authoryear]{elsarticle}
\begin{document}
\nocite{*}
\bibliographystyle{plain}
\bibliography{bibfile_name}
\end{document}
```

其生成的格式如下：

```
\begin{thebibliography}{1}

\bibitem{Wang2005TE}
Xin~Hong Wang, Fu~Qiang Liu, and Guang~Xing Wang.
\newblock Te routing algorithm to minimize maximum link utilization.
\newblock {\em Mini-micro Systems}, 2005.

\end{thebibliography}

```
可以看出来item后面的格式不一样，所以会导致引用出错。

## 二
在使用TexStudio进行编译的时候还是会出错，因为这里需要选择BiBTex
![image.png](https://upload-images.jianshu.io/upload_images/15463866-11786936e150c3b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

