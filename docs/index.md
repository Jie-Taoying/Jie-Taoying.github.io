---
hide:
  #- navigation # 显示右
  #- toc #显示左
  - footer
  - feedback
comments: false
---
<!--
____    __    ____  ______   ______   ____    __    ____  __  .__   __. 
\   \  /  \  /   / /      | /  __  \  \   \  /  \  /   / |  | |  \ |  | 
 \   \/    \/   / |  ,----'|  |  |  |  \   \/    \/   /  |  | |   \|  | 
  \            /  |  |     |  |  |  |   \            /   |  | |  . `  | 
   \    /\    /   |  `----.|  `--'  |    \    /\    /    |  | |  |\   | 
    \__/  \__/     \______| \______/      \__/  \__/     |__| |__| \__| 
-->
                                   

<center><font class="custom-font ml3">Wcowin for MkDocs博客教程</font></center>
<script src="https://cdn.statically.io/libs/animejs/2.0.2/anime.min.js"></script>
<style>
    .custom-font {
    font-size: 31px; /* 默认字体大小为8px */
    color: #757575;
}
@media (max-width: 768px) { /* 假设768px及以下为移动端 */
    .custom-font {
        font-size: 25px; /* 移动端字体大小为6px */
    }
}
</style>



<div class="grid cards" markdown>

-   :material-notebook-edit-outline:{ .lg .middle } __导航栏__

    ---
    ![image](https://pic3.zhimg.com/80/v2-0786a6086793ccca444226e9ab3561ec_1440w.webp){ class="responsive-image" align=right width="230" height="300" style="border-radius: 25px;" }

    
    - [x] {==简洁美观==} ，功能多元化，小白配置
    - [x] 基于{~~~>Material for MkDocs~~}美化
    - [x] 如遇页面卡顿，请使用{--科学上网--}
    - [x] 𝕙𝕒𝕧𝕖 𝕒 𝕘𝕠𝕠𝕕 𝕥𝕚𝕞𝕖 !  
    === "Mac/PC端"

        请在上方标签选择分类/左侧目录选择文章

    === "移动端"

        请点击左上角图标选择分类和文章
    

</div>
<style>
    @media only screen and (max-width: 768px) {
        .responsive-image {
            display: none;
        }
    }
</style>


***  


<div class="grid cards" markdown>

-   :simple-materialformkdocs:{ .lg .middle } __Mkdocs教程__

    ---

    - [Mkdocs视频教程](https://space.bilibili.com/1407028951/lists/4566631?type=series){target=“_blank”}
    - [部署静态网页至GitHub pages](blog/Mkdocs/mkdocs1.md)
    - [Mkdocs部署配置说明(mkdocs.yml)](blog/Mkdocs/mkdocs2.md)
    - [如何给MKdocs添加友链](blog/websitebeauty/linktech.md)
    - [网站添加Mkdocs博客](blog/Mkdocs/mkdocsblog.md)
    - [Blogger](blog/index.md)


-   :simple-aboutdotme:{ .lg .middle } __关于__

    ---
    - [Mkdocs-Wcowin博客主题社区](https://support.qq.com/products/646913/){target=“_blank”}
    - [留言板](liuyanban.md)[^Knowing-that-loving-you-has-no-ending] 
    - [Blogger](blog/index.md)   
    [:octicons-arrow-right-24: 了解我](about/geren.md)[^see-how-much-I-love-you]

</div>



[^Knowing-that-loving-you-has-no-ending]:太阳总是能温暖向日葵  
[^see-how-much-I-love-you]:All-problems-in-computer-science-can-be-solved-by-another-level-of-indirection



<!-- <script src="//code.tidio.co/6jmawe9m5wy4ahvlhub2riyrnujz7xxi.js" async></script> -->


<style>
body {
  position: relative; /* 确保 body 元素的 position 属性为非静态值 */
}

body::before {
  --size: 35px; /* 调整网格单元大小 */
  --line: color-mix(in hsl, canvasText, transparent 80%); /* 调整线条透明度 */
  content: '';
  height: 100vh;
  width: 100%;
  position: absolute; /* 修改为 absolute 以使其随页面滚动 */
  background: linear-gradient(
        90deg,
        var(--line) 1px,
        transparent 1px var(--size)
      )
      50% 50% / var(--size) var(--size),
    linear-gradient(var(--line) 1px, transparent 1px var(--size)) 50% 50% /
      var(--size) var(--size);
  -webkit-mask: linear-gradient(-20deg, transparent 50%, white);
          mask: linear-gradient(-20deg, transparent 50%, white);
  top: 0;
  transform-style: flat;
  pointer-events: none;
  z-index: -1;
}

@media (max-width: 768px) {
  body::before {
    display: none; /* 在手机端隐藏网格效果 */
  }
}
</style>