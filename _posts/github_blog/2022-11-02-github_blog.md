---
layout: post
title: 코드박스 복사 버튼 만들기 [깃허브 블로그]
date: 2022-11-02
# last_modified_at: 2022-11-02
tags: [github.io, github_blog]
categories: github_blog
toc: true
toc_sticky: true
comments: true
---

<br/>

## 서론
제가 적용한 방법을 올려봅니다.  
깃허브 블로그 복사버튼 만들기가 해외 블로그에만 나오길래 이것저것 적용해보다가 겨우 만들었네요.  
제 블로그의 테마인 Jekyll - Not Pure Poole 기준입니다.

---

## 복사 버튼 만들기

### 1. _includes/codeHeader.html 만들어 아래 코드를 입력합니다.
- 버튼을 만들고 클래스 이름을 지어줍니다.
- Copy code to clipboard 부분이 복사버튼에 들어갈 내용입니다.

{% include codeHeader.html %}
```html
<div class="code-header">
  <button class="copy-code-button">
    Copy code to clipboard
  </button>
</div>
```

### 2. assets/scripts/copycode.js 만들어 아래 코드를 입력합니다.

{% include codeHeader.html %}
```javascript
const codeBlocks = document.querySelectorAll('.code-header + .highlighter-rouge');
const copyCodeButtons = document.querySelectorAll('.copy-code-button');

copyCodeButtons.forEach((copyCodeButton, index) => {
  const code = codeBlocks[index].innerText;

  copyCodeButton.addEventListener('click', () => {
    window.navigator.clipboard.writeText(code);

    const { innerText: originalText } = copyCodeButton;
    copyCodeButton.innerText = 'Copied!';

    copyCodeButton.classList.add('copied');

    setTimeout(() => {
      copyCodeButton.innerText = originalText;
      copyCodeButton.classList.remove('copied');
    }, 2000);
  });
});
```

### 3. 마지막으로 _layout/post.html의 맨 마지막 부분에 아래 코드를 붙여넣으면 됩니다.

{% include codeHeader.html %}
```html
<script src="/assets/scripts/copyCode.js"></script>
```

### 4. codeblock 작성 시 {% raw %} {% include codeHeader.html %} {% endraw %} 을 포함하여 아래와 같이 작성하면 됩니다.

{% include codeHeader.html %}
{% raw %}
````markdown
{% include codeHeader.html %}
```someLanguage
code goes in here!
```
````
{% endraw %}

복사 버튼이 코드블럭 왼쪽 위에 붙어있는 것을 확인 가능합니다.

복사버튼을 코드박스 안으로 넣기, 꾸미기는 아래 단계를 진행하면 됩니다.

---

## 복사버튼 꾸미기 및 코드박스 안으로 위치 이동

### 1. _sass/codeHeader.scss를 만들고 복사해 넣으면 됩니다.

{% include codeHeader.html %}
```css
.copy-code-button {
  /* 복사 버튼 색상  */
  color: #272822;
  /* 복사 버튼 배경 색상 */
  background-color: rgb(212, 214, 140);
  /* 복사버튼 경계 색상 */
  border-color: #272822;
  border: 2px solid;
  border-radius: 3px 3px 0px 0px;

  /* 아래 세 줄은 복사 버튼 오른쪽 정렬 */
  display: block;
  margin-left: auto;
  margin-right: 0;

  /* 바로 위 글씨와 복사 버튼 사이의 간격 */
  margin-top: -30px;
  margin-bottom: -2px;
  padding: 3px 8px;
  font-size: 0.8em;
  
  position: relative;
  /* 아래 값을 적절히 변경하면 위 아래로 복사버튼 이동 */
  bottom: -24px;
}

.copy-code-button:hover {
  cursor: pointer;
  background-color: #e5e548;
}

.copy-code-button:focus {
  background-color: rgb(253, 139, 194);
  outline: 0;
}

.copy-code-button:active {
  background-color: rgb(212, 214, 140);
}

.highlight pre {
  margin: 0;
}

```

### 2. assets/style.scss 에 붙여넣기

{% include codeHeader.html %}
```css
@import "codeHeader";
```

### 3. 다시 코드블럭위에 {% raw %} {% include codeHeader.html %} {% endraw %}을 포함하여 작성해보기

{% include codeHeader.html %}
{% raw %}
````markdown
{% include codeHeader.html %}
```someLanguage
code goes in here!
```
````
{% endraw %}

## ✨ 여기까지 하면 성공 ✨ 각자 스타일에 맞추어 css파일을 조금씩 변경하시면 됩니다.