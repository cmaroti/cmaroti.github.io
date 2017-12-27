---
layout: post
title: LaTeX in GitHub Readmes and Blogs
---

One challenge I have come across in my time at Metis and in writing blog posts is how to type mathematical expressions.

I am familiar with LaTeX from undergrad math and science classes, and I figured there must be a way to use that typesetting system on blogs.

Here is a quick summary of what I've learned from reading other resources on this topic (cited below).

## LaTeX in Jekyll Blogs

To get LaTeX working in Jekyll blogs, we can use MathJax, which is a JavaScript library that displays mathematical notation in web browsers. (Jupyter notebook uses this too.)

To add MathJax, go the the `\_layouts` folder of your repo, and find the `default.html` file. Then, inside the `<head>` tag add the following:

```
<script type=“text/javascript" src=“http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML”></script>
```

Now you can type LaTeX pretty much normally, but with delimiters $$...$$ or \\\\(...\\\\) for inline math, and \\\\[...\\\\] for block math.

### An example
``$$\\frac{1}{1+\\sin(x)}$$`` in markdown becomes

$$\frac{1}{1+\sin(x)}$$
