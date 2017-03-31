---
layout: post
title: Example content
---

Using iTextSharp, you could easily automate tasks like sorting pdfs based on number of pages or paper size.
The following snippets show the folder parser to do so.

{% highlight csharp %}
using iTextSharp.text;
using iTextSharp.text.pdf;
using System;
using System.IO;
 
foreach (string file in Directory.EnumerateFiles(path, "*.pdf", SearchOption.TopDirectoryOnly))
{
    PdfReader reader = new PdfReader(file);
    int numberOfPages = reader.NumberOfPages;
 
    string newDir = path + String.Format(@"\{0} Pages", numberOfPages);
    Directory.CreateDirectory(newDir);
 
    File.Copy(file, Path.Combine(newDir, Path.GetFileName(file)));
    File.Delete(file);
}

foreach (string file in Directory.EnumerateFiles(path, "*.pdf", SearchOption.TopDirectoryOnly))
{
    PdfReader reader = new PdfReader(file);
    Rectangle size = reader.GetCropBox(1);
    //Rectangle mediabox = reader.GetPageSize(1);
    //Rectangle cropbox = reader.GetCropBox(1);
 
    string newDir = path + String.Format(@"\Size {0} x {1}", size.Height, size.Width);
    Directory.CreateDirectory(newDir);
 
    File.Copy(file, Path.Combine(newDir, Path.GetFileName(file)));
    File.Delete(file);
}
{% endhighlight %}