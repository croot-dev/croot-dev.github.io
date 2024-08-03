---
layout: post
title: "Github Pages Jekyll 에서 Katex 적용"
created: 2024-01-03
edited: 2024-08-03
category: [기술]
tags: [Mathjax:rgb(250 222 201):rgb(73 41 14),Katex:rgb(253 236 200):rgb(64 44 27),Markdown:rgb(227 226 224):rgb(50 48 44),Jekyll:rgb(253 236 200):rgb(64 44 27)]
---


노션 페이지에 수학공식을 작성 후 마크다운으로 github pages에 노출하니 자동으로 변환이 되지 않았다.


구글링하니 [MathJax](https://www.mathjax.org/) 를 이용한 글이 몇가지 보여서 적용해 보았다.


![0](/assets/img/2024-01-03-Github-Pages-Jekyll-에서-Katex-적용.md/0.png)_Untitled.png_


위와 같이 원치 않는 부분까지 치환이 되는 것이 보이고, 적용이 너무 느려서 대체 라이브러리를 찾아보았다.


[Katex](https://katex.org/) 라는 라이브러리가 많이 사용되어 적용해보았다.


`~/_includes/katex_support.html`


```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css" integrity="sha384-n8MVd4RsNIU0tAv4ct0nTaAbDJwPJzDEaqSD1odI+WdtXRGWt2kTvGFasHpSy3SV" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" integrity="sha384-XjKyOOlGwcjNTAIQHIpgOno0Hl1YQqzUOEleOLALmuqehneUG+vnGctmUb0ZY0l8" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" integrity="sha384-+VBxd3r6XgURycqtZ117nYw44OOcIax56Z4dCRWbxyPt0Koah1uHoK0o4+/RRE05" crossorigin="anonymous"></script>
<script>
    document.addEventListener("DOMContentLoaded", function() {
        renderMathInElement(document.body, {
          delimiters: [
              {left: '$$', right: '$$', display: true},
              {left: '\\[', right: '\\]', display: true}
          ],
          throwOnError : false
        });
    });
</script>
```


`~/_layouts/post.html`


```markdown
---
layout: page
---
{% include katex_support.html %}

// ...생략
```


잘 적용 되었다 끄읏!

