---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-left
# use UnoCSS
css: unocss
---

# Vue.js 設計與實現

第一章 權衡的藝術

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Let's GO! <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/wowdacom/Vue_design_and_implementation" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# 大綱

- 📝 **圖形化框架設計介紹** - 命令式和聲明式
- 🧑‍💻 **權衡** - 性能 VS. 可維護姓 VS. 心智負擔
- 🤹 **圖形化框架設計方式** - innerHTML VS. 虛擬 DOM
- 🤹 **圖形化框架設計方式** - 運行時 + 編譯時
- 🎨 **總結** - 權衡的藝術

<br>
<br>

Read more about [Why Slidev?](https://sli.dev/guide/why)

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---

# 圖形化框架設計介紹

- 命令式和聲明式
- 權衡：性能 VS. 可維護姓 VS. 心智負擔
- 設計方式：操作 DOM、innerHTML、虛擬 DOM

---

# 命令式和聲明式

## 命令式

```ts
$("#app")
  .text("hello world")
  .on("click", () => {
    alert("ok");
  });
```

<br>

## 聲明式

```ts
<div @click="() => alert('ok')">Hello World</div>
```

---

# 權衡：性能 VS. 可維護姓 VS. 心智負擔

<h1 class="text-center">結論：聲明式代碼不優於命令式代碼的性能</h1>

---

# 圖形化框架設計方式

- createElement 直接運行
- innerHTML template 的直接編譯
- Vnode Compiler + Render 編譯 + 運行

---

# 虛擬 DOM VS. innerHTML 初始化

<table>
    <thead>
        <tr>
            <th style='text-align: center' colspan="3">純JS，innerHTML、虛擬 DOM 在創建時的性能</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>純JS </td>
            <td>innerHTML 渲染 HTML 字符串</td>
  			    <td>虛擬 DOM 創建 JS 物件 (VNode)</td>
        </tr>
  		<tr>
          <td>DOM 運算</td>
          <td>新建所有 DOM 元素</td>
  			  <td>新建所有 DOM 元素</td>
      </tr>
    </tbody>
</table>

---

# 虛擬 DOM VS. innerHTML 更新時

<table>
    <thead>
        <tr>
            <th  style='text-align: center'  colspan="3">純JS，innerHTML、虛擬 DOM 在創建時的性能</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>純JS 創建 JS Object (VNode)</td>
            <td>innerHTML 渲染 HTML 字符串</td>
  			    <td>虛擬 DOM 創建 JS 物件 (VNode) + Diff</td>
        </tr>
  		<tr>
          <td>DOM 運算</td>
          <td>必要的 DOM 元素</td>
  			  <td>銷毀所有舊的 DOM 元素 新建所有新的 DOM</td>
      </tr>
    </tbody>
</table>

---

# 圖形化框架設計方式：運行時和編譯時

- 渲染器
- 編譯器

---

# 渲染器

```ts
const vNode = {
  tag: "div",
  children: [{ tag: "span", children: "Hello! Vue Design and Implement!" }],
};

function Render(obj, root) {
  const el = document.createElement(obj.tag);
  if (typeof obj.children === "string") {
    const text = document.createTextNode(obj.children);
    el.appendChild(text);
  } else if (obj.children) {
    //數組，遞迴調用 Render，使用 el 作為 root 參考
    obj.children.forEach((child) => Render(child, el));
  }

  //將元素添加到 root
  root.appendChild(el);
}

const app = document.querySelector("#app");
Render(vNode, app);
```

https://jsfiddle.net/pukwqgcn/

# 編譯器(不純的)

```html
<div><span>Hello Vue Design and Implement</span></div>
```

👇 👇 👇 Compiler

```ts
const vNode = {
  tag: "div",
  children: [{ tag: "span", children: "Hello! Vue Design and Implement!" }],
};
```

---

# 編譯器(純)

##### ex: servlet

```html
<div><span>Hello Vue Design and Implement</span></div>
```

👇 👇 👇 Compiler

```ts
const div = document.createElement("div");
const span = document.createElement("span");
span.innerText = "hello world";
div.appendChild(span);
document.body.appendChild(div);
```

---

# Q&A

- 再一個框架裡，什麼是運行時，什麼是編譯時，他們各有什麼特徵，他們對框架有什麼影響？
