---
layout: post
title: 'Jekyll 에서 LateX 사용하기'
author: hyunkyung
date: 2018-11-27 12:10
tags: [Jekyll]
image: 
---

# Jekyll 에서 LateX 사용하기



포스팅을 하다보면 수식을 쓸 일들이 있습니다.

방법을 몰라서 그냥 캡쳐해서 이미지로 사용했었는데, 생각보다 간단하게 적용이 가능했습니다.



### Typora 에서 LateX  사용하기

제가 사용하는 마크다운 편집기인 ``Typora`` 기준으로 설명하겠습니다.

파일 > 환경설정에 들어가면 ``Markdown`` 관련 탭이 있습니다.

![](/files/typora.PNG)

여기서 문법 강조 지원에 있는 수식 관련 항목을 체크해 주시면 됩니다. ~~너무 쉽네요... 머쓱타..드..~~



### Jekyll 에서 Latex 사용하기

위와 같은 방법으로 Typora 에서 수식을 사용했는데, 블로그에 올리니 적용이 안되었습니다.

제가 사용하는 ``Jekyll`` 기준으로 설명하면,<br>블로그 저장소의 상위폴더 중 ``_include``  의 ``head.html`` 파일을 들어가셔서 

```html
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        inlineMath: [['$','$']]
      }
    });
</script>
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```

을 추가해주시면 됩니다. ~~이것도 너무 쉽네요... 머쓱키..토..~~





#### 참고

- [Typora 에서 LateX 사용하기](https://support.typora.io/Math/)

- [Latex Symbol 목록](https://oeis.org/wiki/List_of_LaTeX_mathematical_symbols)



