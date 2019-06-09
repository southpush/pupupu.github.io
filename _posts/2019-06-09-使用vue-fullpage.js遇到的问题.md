---
layout:     post
title:      使用vue-fullpage.js遇到的问题
subtitle:   使用axios获取内容，vue-fullpage.js不渲染
date:       2019-06-09
author:     PUPUPU
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - fullpage.js
    - vue

---
背景：研究网页的动画效果（指怎么用插件），因为在学vue，所以拿vue写。

fullpage.js是一个单页滚动部分网站插件，官网地址：[fullpage.js][1]
很好看，有官方的vue-fullpage.js包，github地址：[vue-fullpage.js github][2]

官方的例子这样的

    <div>
        <full-page ref="fullpage" :options="options" id="fullpage">
        <div class="section">
          First section ...
        </div>
        <div class="section">
          Second section ...
        </div>
      </full-page>
    </div>

实际我在开发中，每个section都不是写死的，有几个section，section里什么内容，都是用axios去服务器里拿的，然后用v-for来显示，大概是下面这样，不过这样是没法正确渲染出来的。

    <div class='section' v-for='section in sections'>
        {{ section.content }}
    </div>

琢磨了很久，问了舍友大佬也没懂我这问题，最后想了下既然没法动态地更改内容，干脆搞个v-if，等获取到数据了再渲染，获取数据时候用spinner转圈圈，就这样，spinner用的vue-bootstrap的。

    <b-spinner label="Spinning" v-if'loading'></b-spinner>
    <div class='section' v-for='section in sections' v-if='!loading'>
        {{ section.content }}
    </div>

js代码，请求数据啥的放vuex里，getData()返回的是promise

    data(){
        return {
            loading: true,
            sections: [],
        }
    },
    created(){
        this.$store.dispatch({
            type: 'getData'
        }).then( res => {
            this.sections = res.data
            this.loading = false
        }).catch( err => {
            // 错误处理
        })
    }

就加了个v-if就没有问题了。


  [1]: https://alvarotrigo.com/fullPage/zh/
  [2]: https://github.com/alvarotrigo/vue-fullpage.js