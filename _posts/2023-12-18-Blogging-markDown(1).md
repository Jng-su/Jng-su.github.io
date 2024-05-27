---
# 👨‍💻 (project) 📌 (fixed) 📖 (What to Learn) 🧷 (Anything) 🌱 (Link)
title: 📌 MarkDown 기본 사용법
author: JEONGSUJONG
date: 2023-12-18 00:00:00 +0800
toc: true
pin: true
published: false
tags: [HelloWorld]
---

> ## 😎 Header Size

---

# `#`글씨를 크게

{: data-toc-skip='' .mt-4 .mb-0 }

## `##` 글씨를 약간 크게

{: data-toc-skip='' .mt-4 .mb-0 }

### `###` 글씨를 약간 약간 크게

{: data-toc-skip='' .mt-4 .mb-0 }

#### `####` 글씨를 약간 약간 약간 크게

> ## 📌 Unordered list

---

- Chapter
  - Section
    - Paragraph

> ## ✍ UnderLine

---

`<U></U>` : <U>JEONGSUJONG</U>

> ## ✅ Check Box

---

`- [ ] ` : 공백을 **총 3개** 넣어줘야 한다.

- [ ] 오늘 할 일
  - [x] 운동 가기
  - [ ] 공부 하기
  - [ ] 영화 시청

> ## 💬 Description list

---

Sun
: the star around which the earth orbits

Moon
: the natural satellite of the earth, visible by reflected light from the sun

> ## 🚨 Prompts

---

> An example showing the `tip` type prompt.
{: .prompt-tip }

> An example showing the `info` type prompt.
{: .prompt-info }

> An example showing the `warning` type prompt.
{: .prompt-warning }

> An example showing the `danger` type prompt.
{: .prompt-danger }

> ## 📦 Code Box

---

``: 한 줄 작성 시 **한 번만** 사용`System.out.println("Hello World");`

````: 여러 줄 작성 시 **총 3개** 사용

```java
class Solution {
    public String solution(String s) {
        String[] numbers = s.split(" ");
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        for (String numStr : numbers) {
            int num = Integer.parseInt(numStr);
            min = Math.min(min, num);
            max = Math.max(max, num);
        }

        return min + " " + max;
    }
}

````

> ## 🤷 Block Quote

---

`>` 으로 시작하는 텍스트

> 인용문
>
> > 인용문의 인용문
> >
> > > 인용문의 인용문의 인용문
> > >
> > > > 인용문의 인용문의 인용문의 인용문
> > > >
> > > > > 인용문의 인용문의 인용문의 인용문의 인용문

> ## 🏹 External Link

---

`<url>` : 외부 url 링크로 이동
<https://www.youtube.com>

`[name](url)` : 외부 url 링크로 이동 (이름 지정 가능)
[Google](https://www.google.co.kr)

> ## 🎫 Table

---

```md
| Header 1 | Header 2 | Header 3 | Header 4 |
| :------: | :------: | :------: | :------: |
|  item 1  |  item 2  |  item 3  |  item 4  |
```

| Header 1 | Header 2 | Header 3 | Header 4 |
| :------: | :------: | :------: | :------: |
|  item 1  |  item 2  |  item 3  |  item 4  |

| Header 1 | Header 2 | Header 3 | Header 4 |
| :------- | :------- | :------- | :------- |
| item 1   | item 2   | item 3   | item 4   |

> ## 📷 image

---

{: data-toc-skip='' .mt-4 .mb-0 }

### default

```md
![Image_title](img_url)
```

![Default Image](https://github.com/JEONGSUJONG/Readme_main/assets/142254876/719d9ee7-ec4c-48e7-a116-4253557c2ad2)

{: data-toc-skip='' .mt-4 .mb-0 }

### Left align

```md
![Image_title](img_url){: width="000" height="000" .normal}
```

![Left Align](https://github.com/JEONGSUJONG/Readme_main/assets/142254876/719d9ee7-ec4c-48e7-a116-4253557c2ad2){: width="100" height="100" .normal}

{: data-toc-skip='' .mt-4 .mb-0 }

### float to left/right

```md
![float to left](img_url){: width="000" height="000" .w-50 .normal}
![float to right](img_url){: width="000" height="000" .w-50 .right}
```

![float to left](https://github.com/JEONGSUJONG/Readme_main/assets/142254876/719d9ee7-ec4c-48e7-a116-4253557c2ad2){: width="200" height="200" .w-25 .normal}
![float to right](https://github.com/JEONGSUJONG/Readme_main/assets/142254876/719d9ee7-ec4c-48e7-a116-4253557c2ad2){: width="200" height="200" .w-25 .right}

{: data-toc-skip='' .mt-4 .mb-0 }

### Shadow (dark/light mode)

```md
![light mode only](img_url){: .light .w-00 .shadow .rounded-10 width="000" height="000"}
![dark mode only](img_url){: .dark .w-00 .shadow .rounded-10 width="000" height="000"}
```

![light mode only](https://github.com/JEONGSUJONG/Readme_main/assets/142254876/84665228-758a-4815-b59a-f12564d5ec3d){: .light .w-50 .shadow .rounded-10 width="972" height="589"}
![dark mode only](https://github.com/JEONGSUJONG/Readme_main/assets/142254876/84665228-758a-4815-b59a-f12564d5ec3d){: .dark .w-50 .shadow .rounded-10 width="972" height="589"}

> ## 📹 Video

{% include embed/youtube.html id='N1wJG-2g49Y' %}
