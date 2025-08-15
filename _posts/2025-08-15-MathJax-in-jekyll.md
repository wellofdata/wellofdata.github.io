---
title: "Jekyll 블로그에 MathJax 기능 넣어서 수식 입력 및 렌더링 정상화하기 "
categories:
  - logs
tags:
  - jekyll
  - MathJax
---

## 발단

얼마 전 글을 적다보니, Vscode 내에서는 수식 렌더링이 정상적으로 되는데 막상 빌드된 웹사이트에서는 수식 렌더링이 정상적으로 되지 않는 현상을 발견했다.

## 문제 원인

Jekyll이 기본적으로 사용하는 마크다운 프로세서 (md -> html converter)는 Kramdown이다. Kramdown 공식 문서를 보면 이미 기본적으로 MathJax를 지원함. (Katex 등 다른 수식 렌더링 방법을 사용하려면 플러그인이 필요함) 근데 왜 안 되는걸까? 하고 봤더니 공식 문서가 말하길,

> Note that kramdown does not ship with the MathJax library and that therefore the **“default” template does not include a link** to it! The MathJax documentation describes how to add a link to MathJax to your page.

## 해결방법

[MathJax 공식 사이트](https://www.mathjax.org) 를 참조해보니 해결책은 심플하다.

``` Javascript
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@4/tex-mml-chtml.js"></script>
```

그냥 이걸 html 파일에 포함해주면 끝. 다만 우리는 jekyll이라는 SSG를 사용 중이므로 몇 가지 과정이 더 있다.

### 1. '_config.yml' 수정

_config.yml에 글로벌 하게 변수를 선언하거나, 아니면 페이지 레이아웃 쪽에서 변수를 선언하거나, 어쨌든 수식 입력 관련 옵션을 활성화 할 수 있도록 변수를 하나 선언해준다.

**예시**

``` yaml
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: false
      show_date: true
      comments: false
      share: false
      related: true
      mathjax: true ## <- math jax 기능 추가>
  # _pages
  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      author_profile: true
```

이 경우 나중에 Liquid 문법 상으로 page.mathjax를 call 하면 저장되어있던 true 값이 반환된다.

### 2. '/_layouts' 디렉토리에 `head.html` 수정하기

`head.html` 파일 말미에 다음을 추가해 준다.

``` Javascript
 {% if page.mathjax %}
   <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@4/tex-mml-chtml.js"></script>
 {% endif %}
```

### 3. (optional) MathJax configuration

원한다면 MathJax Object를 불러오기 전에 설정을 좀 만져줄 수 있다. 실제로 현재 인터넷에 'jekyll mathjax'를 키워드로 검색하면 나오는 블로그들을 보면 커스텀 설정 부분도 포함하고 있는 경우가 많다.

출처: [MKKIM's blog: Jekyll Github 블로그에 MathJax로 수학식 표시하기](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/)

``` Js
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    TeX: {
      equationNumbers: {
        autoNumber: "AMS"
      }
    },
    tex2jax: {
    inlineMath: [ ['$', '$'] ],
    displayMath: [ ['$$', '$$'] ],
    processEscapes: true,
  }
});
MathJax.Hub.Register.MessageHook("Math Processing Error",function (message) {
	  alert("Math Processing Error: "+message[1]);
	});
MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message) {
	  alert("Math Processing Error: "+message[1]);
	});
</script>
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```



## 주의할 점

### 1. `cdn.mathjax.org` 지원 종료

CDN 은 알다시피 다운로드 등의 트래픽이 아주 높을 것이 예상되는 컨텐츠를 각 지역의 캐시 서버(엣지서버) 를 만들어 다운로드 속도를 높이고 대역폭 비용을 아끼게 해주는 서비스다. 근데 이제 주소가 바뀌었고, 상술한 [해결방법](#해결방법) 탭의 cdn 주소를 사용해야 한다.

### 2. 실제 문서 작성시 delimiter

**기본** MathJax 에서는 display math style 로 `\[...\]` 와 `$$...$$`를 모두 사용하고, inline math로는 `\(...\)` **만**을 지원한다. 따라서 달러 한 개짜리 delimiter를 사용하고 싶다면 앞에서 말한 설정을 건드려주어야 한다. (MathJax.Hub.Config 부분) 

또한, jekyll 특성상 포스팅 입장에서는 Kramdown -> Mathjax 이렇게 두 번 텍스트가 처리되기 때문에 마크다운 파일을 작성할때 **백슬래시를 두 개 사용하거나** 혹은 **달러 사인 두 개를 display/inline 구분없이 사용**하여야 제대로 처리가 된다. 
백슬래시를 두 개 사용하는 이유는 Kramdown이 하나의 백슬래시를 지워버린 채로 html을 생성해서 나중에 mathjax가 결과물인 html 파일을 봐도 `\(...\)` 가 아니라 `(...)`를 보게 되기 때문에 수식처리가 제대로 되지 않기 때문이다. 
두번째 방법 같은 경우 나도 좀 신기했던 건데, 마크다운 파일내에서 줄바꿈을 하지 않은 채로 달러사인 두 개를 사용하면 알아서 수식입력을 인라인으로 처리해준다. 정확한 원리는 jekyll이랑 Kramdown 사이에 무슨 일이 일어나는지 좀 더 들여봐야 할 것 같은데 아직 잘 모르겠다. 아마 jekyll 이 먼저 '이 블록은 수식 블록입니다' 하는 태그를 붙여주는 것 같다.

## 확인되지 않은 부분

1. 아직 환경(align, equation) 같은게 제대로 작동하는지는 확인을 안 해 봤다. 숫자 매기는 것도 제대로 매겨지나? 흠...
2. 비슷한 맥락으로 MathJax에서 기존의 Latex 패키지를 사용할 수 있는지도 잘 모르겠다. tikz라던가 뭐 그런...