---
title: "Setting up Visual Studio Code as the LaTeX IDE"
description: This tutorial shows you how to set up the Visual Studio Code as a perfect LaTeX editing environment.
date: 2020-06-19
layout: post
---
Visual Studio Code (VS Code) is a very powerful source-code editor that supports a variety of programming languages. It can work with additional languages by integrating the extensions in the VS code Marketplace. There are quite a mount of tutorials teaching you how to use VS Code as the Integrated Development Environment (IDE) for LaTeX wirting, which is a document preparation system commonly used by the research and publication community. However, some of them become functionless as the VS Code version updates. This post provides you the latest feasible solution.

1. Download and install [VS Code](https://code.visualstudio.com/).
2. Download and install [MiKTeX](https://miktex.org/download) as the TeX/LaTeX typesetting system on Windows. You can also choose [TeX Live](https://www.tug.org/texlive/) as an alternative. Personally, I found MikTex better in space saving and higher successful compiling rate.
3. Install the extension [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) in the VS Code Market.
4. Go to settings in VS Code and open the *settings.json* file. Add the code in the *Appendix* of this post. Please notice my comments and customize the code accordingly. Save the *json* file.
5.  Now, you should be able to compile your LaTeX files by clicking *Ctrl + Alt + B*. After the generation of your .pdf file, you can view it by clicking *Ctrl+Alt+V*. For the forward search function, you can either right-click and select *SyncTeX from cursor*, or use shortcut *Ctrl + Alt + J* to jump to the .pdf content where you stayed in the TeX file.
6. If you want to use the inverse/backforward search function, go to settings-option in the *SumatraPDF* software. You can install this software using the link provided in the comments in *Appendix* code. Add the following text into the *Set inverse search command-line* space. Customize the folder directory with your own.
{% highlight csharp linenos %}
"C:\\Program Files\\Microsoft VS Code\\Code.exe" "C:\\Program Files\\Microsoft VS Code\\resources\\app\\out\\cli.js" -r -g "%f:%l"
{% endhighlight %}
7. Now, you can use the inverse search function by double click on the pdf file!

It is important to keep in mind that the settings can malfunction when the VS Code updates or for different operating systems. So try to search for more guides if this is not working.

### Appendix code for settings.json
{% highlight csharp linenos %}
{
    "latex-workshop.latex.recipes": [
      /* You can uncomment this or add more compiling options if you need.
        //Here, I only compile with the bibliography.
      {
          "name": "pdflatex  ",
          "tools": [
          "pdflatex"
          ]
      },*/

      {
        "name": "pdflatex ➞ bibtex ➞ pdflatex`×2",
        "tools": [
          "pdflatex",
          "bibtex",
          "pdflatex",
          "pdflatex"
        ]
      }
  ],
  // Compling tools including pdflatex & bibtex
  "latex-workshop.latex.tools": [
      {
          "name": "pdflatex",
          "command": "pdflatex",
          "args": [
              "-synctex=1",
              "-interaction=nonstopmode",
              "-file-line-error",
              "%DOC%"
          ]
      },
      {
          "name": "bibtex",
          "command": "bibtex",
          "args": [
              "%DOCFILE%"
          ]
      }
  ],
  /* Note: You can uncomment this paragraph if you want to clean up the temporary files during compiling. 
    // However, if you need to use the forward search and inverse search function, I recommend keep commenting this paragraph.
    // As it seems that the forward/inverse searching requires some of the temporary files (probably *.synctex.gz),
    // deleting it will fail the search function.
  "latex-workshop.latex.clean.fileTypes": [  
    "*.aux",  
    "*.bbl",  
    "*.blg",  
    "*.idx",  
    "*.ind",  
    "*.lof",  
    "*.lot",  
    "*.out",  
    "*.toc",  
    "*.acn",  
    "*.acr",  
    "*.alg",  
    "*.glg",  
    "*.glo",  
    "*.gls",  
    "*.ist",  
    "*.fls",  
    "*.log",  
    "*.fdb_latexmk",  
    "*.nav",  
    "*.snm",  
    "*.synctex.gz"  
  ],*/
  "latex-workshop.showContextMenu":true, 
  "latex-workshop.latex.autoClean.run": "never",
  "latex-workshop.latex.recipe.default": "lastUsed",
  "latex-workshop.latex.autoBuild.run": "never",
  // View the pdf file externally with [SumatraPDF](https://www.sumatrapdfreader.org/download-free-pdf-viewer.html), comment if not needed.
  "latex-workshop.view.pdf.viewer":"external",
  "latex-workshop.view.pdf.external.viewer.command": "C:\\Program Files\\SumatraPDF\\SumatraPDF.exe",
  "latex-workshop.view.pdf.external.viewer.args": [
       "%PDF%"
  ],
  // Forward search settings, make comment if not required.
  "latex-workshop.view.pdf.external.synctex.command": "C:\\Program Files\\SumatraPDF\\SumatraPDF.exe",
  "latex-workshop.view.pdf.external.synctex.args": [
       "-forward-search",
       "%TEX%",
       "%LINE%",
       "-reuse-instance",
       "-inverse-search",
       // Change this to your own customised directory but follow exactly the style.
       "\"C:\\Program Files\\Microsoft VS Code\\Code.exe\" \"C:\\Program Files\\Microsoft VS Code\\resources\\app\\out\\cli.js\" -r -g \"%f:%l\"",
       "%PDF%"
   ]
}
{% endhighlight %}
