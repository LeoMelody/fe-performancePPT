---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
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
# 浅谈性能优化

<div style="height: 50px"></div>

<h3>王一衡</h3>

---
transition: fade-out
---

# 内容简介

- 🛠 **性能优化方法**
  - 💻 网络及缓存
  - 🪚 拆分的意义
  - 🐸 编译时与运行时
- 🖥 **工具及指标**
  - 🔆监控
  - 🎄常见的性能指标
- 🪡 **一些与性能密切相关的事情**
  - 🪄好的代码组织会带来更好的性能
    - 🪙CompositionAPI的一些个人理解

<br>
<br>

---
transition: slide-up
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# 性能优化方法

<br>

在这部分中我将用一些简单的demo配合我们系统实际的例子来说明在实际的性能优化过程中会产生的一些情况。

<br>

- 💻 网络及缓存
- 🪚 拆分的意义
- 🐸 编译时与运行时

---
transition: slide-left
---

# 网络及缓存
<style>
.container {
  display: flex;
}

.info {
  margin-left: 40px;
  color: red;
  align-self: center;
}

.info .question {
  color: orange;
}
</style>

在浏览器输入URL发生了什么？ (<span style="font-size: 12px">这里不对这个问题进行展开，只做一些抽象的描述，感兴趣的同学可以自行查阅</span>)
<br>
一个高度抽象的模型：
<div class="container">
  <img src="/img.png" width="400" height="50" v-click="1"/> 

  <div class="info">
    <div v-click="3">红色线条都存在耗时</div>
    <div v-click="4">服务器本身也存在耗时</div>
    <div v-click="5" class="question">对于重复的资源，如何减少这些耗时？</div>
    <div v-click="6">使用缓存！</div>
  </div>
</div>



<div v-click="2">
浏览器输入URL，通过DNS解析到对应的ip地址，访问获取到资源，浏览器解析并渲染页面
</div>


---
transition: fade-out
---

# 网络及缓存 —— 缓存的一些基本概念
<style>
.container {
  display: flex;
}

.right {
  flex: 1;
  margin-left: 40px;
  font-size: 12px !important;
}
</style>
<div class="container">
  <div class="left">
    <h4>缓存的好处：空间换时间，节省链路耗时</h4>
    <h4>缓存的风险：时效性和准确性</h4>
    <h4>HTTP缓存分类： 强缓存和协商缓存</h4>
<br>
<br>
    <img src="/img_1.png" width="400" height="50" v-click="1"/> 
  </div>
  <div class="right"  v-click="2">
    <div>强缓存重点在于资源的时效性，比如一些很难进行更新迭代的脚本和样式。例如之前有个通用的去除CSS默认样式的文件</div>
<br>
    <div>协商缓存重点在于资源的准确行，他关注点在于资源有没有更新</div>
<br>
    <div>关于Etag:一种Hash算法，不同的服务器etag的生成规则不一定一样</div>
<br>
    <div>
      <div>
        有MD5的： 
         <img src="/img_2.png" width="400" height="50"/> 
         <img src="/img_3.png" width="400" height="50"/> 
      </div>
<br>
      <div>
        有基于文件时间和文件大小的，如Nginx
       <img src="/img_4.png" width="400" height="50"/> 
      </div>
    </div>
  </div>
</div>


---
class: px-20
---

# 网络及缓存 —— 客户端缓存中的ServiceWorker
<style>
.container {
  display: flex;
}

.right {
  flex: 1;
  margin-left: 40px;
  font-size: 12px !important;
}
</style>


<div class="container">
<div class="left">
ServiceWorker可以理解为是浏览器客户端和资源服务端之间的一个控制层的角色。通过增加这样的一个控制层角色，来达到对资源访问的时候的控制。
<div style="height: 30px"></div>
<img src="/img_5.png" width="400" height="400" />
</div>

<div class="right" style="min-width: 300px" v-click>
<h5>资源请求的拦截代理</h5>
ServiceWorker的核心，通过对特定请求的拦截代理，可以实现缓存，错误处理等操作。
<div style="height: 30px"></div>
<h5>本地存储管理</h5>
CacheAPI(K,V 适合存储req: res)
IndexedDB(NoSQL 存储一些数据)
<div style="height: 30px"></div>
<h5 style="color: #ff4f4f;">资源请求的缓存策略</h5>

<ul>
  <li>Cache Only</li>
  <li>Cache First</li>
  <li>Network First</li>
  <li>Stale While Revalidate</li>
</ul>
<div style="height: 30px"></div>
<h5>预缓存</h5>
在ServiceWorker install阶段预先缓存后续要加载的资源
</div>
</div>

---
transition: slide-up
---
# 网络及缓存 —— 弱网和无网络情况下的一些优化

<img src="/img_15.png">

---
transition: slide-up
---
# 网络及缓存 —— 弱网和无网络情况下的一些优化

<img src="/img_16.png">

---
transition: slide-up
layout: two-cols
---

```ts {all} {maxHeight:'500px'}
function networkFirst ({
  fetchOptions,
  cacheName = 'runtime-cache',
  matchOptions
} = {}) {
  // ...（定义getCachedResponse、fetchAndCatch）

  return async request => {
    let response

    try {
      // 优先发起网络请求，并将请求返回结果缓存到本地
      response = await fetchAndCatch(request)
    } catch (e) {}

    if (response == null) {
      // 网络资源请求失败时，从本地缓存中读取缓存
      response = await getCachedResponse(request)
    }
    
    // 网络请求超时
    setTimeout(() => {
        // ... 网络请求超时处理
    }, timeout)
    

    return response
  }
}

```
::right::
```ts {all} {maxHeight:'500px'}
function staleWhileRevalidate ({
  fetchOptions,
  cacheName = 'runtime-cache',
  matchOptions
} = {}) {
  // ...（定义 getCachedResponse、fetchAndCatch）
  return async request => {
    let response
    // 首先读取本地缓存
    try {
      response = await getCachedResponse(request)
    } catch (e) {}
    // 发起网络请求并更新缓存
    let fetchPromise = fetchAndCatch(request)
    // 如果存在本地缓存，则静默更新缓存即可，无需阻塞函数执行
    if (response) {
      // 静默更新，无需报错
      fetchPromise.catch(e => {})
    } else {
      // 反之则将网络请求到的资源返回
      response = await fetchPromise
    }
    return response
  }
}
```


---
transition: slide-up
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# 拆分的意义

<div v-click="1">
  <ul>
    <li>通过聚合功能而形成的大而全的系统</li>
    <li>通过拆分+集成形成的小而美的应用</li>
  </ul>
</div>

<br>

<div v-click="2">
  <ul>
    <li>单体服务（SPA）</li>
    <li>微服务体系（微前端）</li>
  </ul>
</div>

<br>
<div v-click="3">
  <ul>
    <li>混合包</li>
    <li>webpack SplitChunks</li>
  </ul>
</div>

<br>
<div v-click="4">
  <ul>
    <li>AllIn</li>
    <li>组件化、模块化</li>
  </ul>
</div>

---
layout: two-cols
---
```html
<template>
  <div>
    <button @click="heavyIncrease">click</button>
    <span style="color: #e33131">{{number1}}</span>
    <span style="color: #f5d743">{{number2}}</span>
    <span style="color: #41f85f">{{number3}}</span>
    <span style="color: #700de8">{{number4}}</span>
  </div>
</template>
```

```ts
const heavyIncrease = () => {
  for (let i = 0; i < 10e8; i++) {
    if (i === 10e7) {
      number1.value = 1;
    }
    if (i === 10e7 * 3) {
      number2.value = 2;
    }
    if (i === 10e7 * 6) {
      number3.value = 3;
    }
    if (i === 10e7 * 9) {
      number4.value = 4;
    }
  }
}
```

::right::

<!-- ./components/ShowNumber.vue -->
<ShowNumber />

---
layout: two-cols
---
```ts {all} {maxHeight:'500px'}
let i = 0;
const increase = () => {
  for (; i < 10e8; i++) {
    if (i === 10e7) {
      number1.value = 1;
      break;
    }
    if (i === 10e7 * 3) {
      number2.value = 2;
      break;
    }
    if (i === 10e7 * 6) {
      number3.value = 3;
      break;
    }
    if (i === 10e7 * 9) {
      number4.value = 4;
      break;
    }
  }
  if (i < 10e8) {
    i++;
    setTimeout(increase);
  }
}
const heavyIncrease = () => {
  i = 0;
  increase();
}
```

::right::

<!-- ./components/ShowNumber2.vue -->
<ShowNumber2 />

---
transition: slide-up
---

# 图示
<img src="/img_7.png">
这里其实是很基础的一个宏任务和微任务的概念

但是实际开发中往往不是这样的简单的例子，一般情况下可能是这样的：

Function = HeavyTask1 + HeavyTask2 + HeavyTask3 ...

如果是这样，可以入手的点就多了 **异步** **worker** **schedulerAPI** 等等

---
transition: slide-up
---

# 我们看到的和实际去计算的是不是成正比
这个绿色的部分只是冰山一角
<img src="/img_8.png">



---
transition: slide-up
---

# 编译时与运行时

还是以一个经典面试题入手： babel-loader和babel-polyfill的区别是什么？

<div v-click>
  <div>babel-loader 用于做代码的转换，ES6/7/8/9 => ES5</div>
  <div>babel-polyfill 用于做代码的补齐(patch)，例如19年前浏览器中 BlobAPI 没有 arrayBuffer() 这个方法，就需要自己写个补丁方法来支持</div>
```js
(function () {
  File.prototype.arrayBuffer = File.prototype.arrayBuffer || myArrayBuffer;
  Blob.prototype.arrayBuffer = Blob.prototype.arrayBuffer || myArrayBuffer;
  function myArrayBuffer() {
    // this: File or Blob
    return new Promise((resolve) => {
      let fr = new FileReader();
      fr.onload = () => {
        resolve(fr.result);
      };
      fr.readAsArrayBuffer(this);
    })
  }
})();
```
</div>


---
transition: slide-up
---

# 编译时与运行时
第二个问题： babel-loader和babel-polyfill这俩为什么不合到一起？

<div v-click>
babel-loader作用于编译阶段，而babel-polyfill作用于运行阶段
</div>

<br>
<div v-click>
<div>
那具体有什么区别呢？
</div>
用一个独特的角度来解释： 编译阶段用户是不参与的，但是运行阶段用户会参与进来。就像是webpack的compiler-error和我们系统在使用过程中出现的xxx is not defined的区别
</div>

<br>
<div v-click>
<div>
一些思考？
</div>
如果代码可以提前编译是不是就是优于在运行时中执行？是否可以让更多的功能在编译阶段完成？有哪些例子？
</div>


<br>
<div v-click>
<div>
Vue 有一个compiler+runtime 版本的包 和 只有 runtime 的包。
</div>
<div>
Vue 提供了一个 @vue/compiler-sfc 对我们写的sfc做编译处理成可执行的render函数, Vue-loader 的核心依赖之一
</div>
</div>

---
transition: slide-up
layout: iframe

# the web page source
url: https://liga-ai-team.feishu.cn/wiki/wikcnqlSW5bUvk1Dbjv3OHfngIc
---

---
transition: slide-up
layout: two-cols
---

```vue  {all} {maxHeight:'500px'}
<template functional>
  <div :class="bPlus()">
    <span :class="bPlus('block')"></span>
  </div>
</template>

<script>
export default {
  name: "CompilerComp"
}
</script>

<style>
.zk-compiler-comp {
  color: green;
}
</style>

```
```js
chainWebpack: (config) => {
    config.module
        .rule('bPlus')
        .test(/\.vue$/)
        .use('bplus-loader')
        .loader('bplus-loader')
        .end()
  }
```

::right::

<img src="/img_9.png">
<img src="/img_10.png">

---
transition: slide-up
---

# 处理逻辑
```ts
import * as compiler from '@vue/compiler-sfc';
import * as webpack from 'webpack';
import { TransformerFactory } from '@/transforms/transformer';

export default function (this: webpack.LoaderContext<any>, code: string): string {
	const { descriptor } = compiler.parse(code);
	const transformer = TransformerFactory.getTransformer(descriptor);
	return transformer.transform();
}

```

这个项目是基于vite搭建的。而且vite这个是很好去加东西的，最初的eslint，prettier，vitest 这些都没用它的脚手架装，后面再去补充的时候也很easy。
<br>
然后推荐下vitest这个东西，确实够快，不好的地方就是我感觉还是没jest功能那么多，暂时也没它生态好

<img src="/img_17.png">


---
transition: slide-up
layout: two-cols
---

# 监控
### WHY
我们要了解目前系统的运行情况
### WHAT
发现问题 => 定位问题 => 解决问题 => 复盘问题
### HOW
- 了解要监控的内容
- 要有基本的性能指标
- 报警及对应阈值的定义
- 对应的问题处理


感兴趣的可以了解下 Prometheus和Sentry

::right::

<img src="/img_12.png" height="900" width="">

---
transition: slide-up
---
# 性能指标

- First Paint 首次绘制（FP）
- First contentful paint 首次内容绘制 (FCP)
- **Largest contentful paint 最大内容绘制 (LCP)**
- **First input delay 首次输入延迟 (FID)**
- Time to Interactive 可交互时间 (TTI)
- Total blocking time 总阻塞时间 (TBT)
- **Cumulative layout shift 累积布局偏移 (CLS)**


---
transition: slide-up
---
# 一些与性能优化密切相关的事情

> 好的代码组织是否会带来更好的性能？

这个是一个非常主观的问题，但是在我看来这个是毋庸置疑的

- 业务逻辑更清晰
- 功能耦合度低
- 可读性，扩展性更好

<br>

<div v-click="1">
<ul>
  <li>
    新增的业务逻辑或者可以删除的业务逻辑都非常好下手，不至于写很多补丁代码。往往我们因为业务逻辑的不清晰会去做成倍的无用判断逻辑
  </li>

  <li>
    核心功能，各个组件的功能会更聚焦，更方便去做功能和性能上的优化操作
  </li>

  <li>
    前人种树后人乘凉 而不是 前人欠债后人还钱
  </li>
</ul>

<br>

<div v-click="2" style="color: #ef784b">我们是否要尽快去熟悉掌握和使用compositionAPI？</div>

</div>

---
transition: slide-up
layout: two-cols
---
<img src="/img_13.png">
::right::
<img src="/img_14.png">

---
transition: slide-up
---
# 代码的复用

### Vue2 中通常使用的是 Mixin和插槽


- Mixin之间的属性可能会互相冲突，也可能和组件内部命名冲突
- Mixin之间如果存在交集，会很难处理
- Mixin在跨组件重用方面不好用
  这里其实体现在这个mixin不好设计，比如是一个searchMixin，那在场景A 和 场景B中对于searchMixin的通用设计可能会差别较多，mixin将粒度控制的特别细又显得很啰嗦

<br>
<hr />
<br>

- 插槽的代码复杂度增加时会伴随很大的代码可读性问题
- 在用到一个功能时，需要用到很多个组件

个人觉得：
其实插槽用的好的话也是很牛的一种组件重用的解决方案，这里我感觉最难的点就是组件的抽象封装，作用域的定义等。非常很容易（基本都是）初期考虑不全面，后期维护困难的东西

---
transition: slide-up
layout: two-cols
---
# 关于Composable

在Vue3 中，我们可以定义很多小的代码片段来帮我完成特定的工作

```ts
import { ref, onMounted, onUnmounted } from 'vue'
export function useMouse() {
    const x = ref(0)
    const y = ref(0)

    function update(event) {
        x.value = event.pageX
        y.value = event.pageY
    }
    onMounted(() => window.addEventListener('mousemove', update))
    onUnmounted(() => window.removeEventListener('mousemove', update))
    return { x, y }
}
```

::right::
```vue
<script setup>
import { useMouse } from '../use/mouse';
const { x, y } = useMouse();
</script>
<template>
  Mouse position is at: {{ x }}, {{ y }}
</template>

```

<!-- ./components/MouseUse.vue -->
<MouseUse />

**在小的代码片段中使用ref等compositionAPI直接就能构造出响应式的数据结果**

**在Vue2中怎么去实现这样的一个小功能？**

---
transition: slide-up
layout: two-cols
---
```ts {all} {maxHeight:'500px'}
export function useTitle(
  newTitle: MaybeRef<string | null | undefined> = null,
  options: UseTitleOptions = {},
) {
  const {
    document = defaultDocument,
    observe = false,
    titleTemplate = '%s',
  } = options
  const title = ref(newTitle ?? document?.title ?? null)

  watch(
    title,
    (t, o) => {
      if (isString(t) && t !== o && document)
        document.title = titleTemplate.replace('%s', t)
    },
    { immediate: true },
  )

  if (observe && document) {
    useMutationObserver(
      document.head?.querySelector('title'),
      () => {
        if (document && document.title !== title.value)
          title.value = titleTemplate.replace('%s', document.title)
      },
      { childList: true },
    )
  }

  return title
}
```

::right::

```ts
const refValue = ref(0)
assert(refValue === ref(refValue)) // true
```
<br>
<br>
<br>

**更灵活的参数输入，不用关心当前输入是ref的还是primitive的**

<br>
<br>

#### 除此之外

- 更好的TS集成
- 全面强大的工具库支持 vueuse
- 更加容易测试的代码


---
transition: slide-up
---
# 其他

- 运天大佬推荐的面向开发人员的PPT工具 https://sli.dev/
- website webserver webAPP等 云部署网站 https://render.com/ https://vercel.com/ (vercel 好像更聚焦与前端应用) 
- Google 官方推出的一个面向Web 开发者的网站 https://web.dev/
- Google ChromeDevtools 文档  https://developer.chrome.com/docs/devtools/
- 在浏览器输入URL发生了什么 我认为非常好且全面的答案 https://zhuanlan.zhihu.com/p/133906695
- 百度出品的PWA实战 https://lavas-project.github.io/pwa-book/
- Guillaume Chau(Vue 核心成员) 整理的Vue组件优化的九个方案 https://slides.com/akryum/vueconfus-2019
- 我总结的关于宏任务和微任务的一些关系 https://nebulous-twilight-f4e.notion.site/ce4b4f49b71f450bb622a473e4301dcf

