<style>
    :root {
        --primary-color: #007bff;
        --secondary-color: #333;
        --text-color: #666;
        --light-bg: #f8f9fa;
        --border-color: #e9ecef;
        --hover-color: #0056b3;
    }

    .about-me {
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
        font-family: "Microsoft YaHei", sans-serif;
        background-color: white;
        border-radius: 8px;
        box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }

    .section-title {
        font-size: 24px;
        font-weight: bold;
        margin: 25px 0 15px;
        color: var(--secondary-color);
        border-bottom: 2px solid var(--light-bg);
        padding-bottom: 8px;
    }

    .info-header {
        display: flex;
        align-items: center;
        gap: 20px;
        margin-bottom: 20px;
    }

    .avatar {
        width: 150px;/* 调整为你想要的宽度 */
        height: 200px;/* 调整为你想要的高度 */
        overflow: hidden;
        border: 2px solid var(--primary-color);
    }

    .avatar img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .basic-info {
        flex: 1;
    }

    .name {
        font-size: 28px;
        font-weight: bold;
        color: var(--secondary-color);
        margin-bottom: 5px;
    }

    .affiliation {
        font-size: 16px;
        color: var(--text-color);
    }

    .timestamps {
        display: flex;
        gap: 20px;
        color: var(--text-color);
        margin-bottom: 20px;
    }

    .timestamps i {
        margin-right: 5px;
        color: var(--primary-color);
    }

    .intro-paragraph {
        margin-bottom: 15px;
        line-height: 1.8;
        color: var(--text-color);
        text-align: justify;
    }

    .highlight {
        color: var(--primary-color);
        transition: color 0.3s;
    }

    .highlight:hover {
        color: var(--hover-color);
    }

    .contact-list {
        list-style: none;
        padding: 0;
    }

    .contact-list li {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
    }

    .contact-list i {
        width: 25px;
        color: var(--primary-color);
        text-align: center;
        margin-right: 10px;
    }

    .contact-list a {
        color: var(--primary-color);
        text-decoration: none;
        transition: all 0.3s;
        display: inline-flex;
        align-items: center;
    }

    .contact-list a:hover {
        color: var(--hover-color);
        transform: translateX(3px);
    }

    .qrcode {
        margin-top: 10px;
        margin-left: 35px;
        border: 1px solid var(--border-color);
        padding: 5px;
        display: inline-block;
        border-radius: 4px;
        background-color: white;
    }

    .qrcode img {
        width: 120px;
        height: 120px;
        display: block;
    }

    .site-info {
        margin-top: 30px;
        padding-top: 15px;
        border-top: 1px solid var(--light-bg);
        color: var(--text-color);
        font-size: 14px;
        line-height: 1.6;
    }

    .source-code {
        margin-top: 10px;
    }

    .source-code a {
        color: var(--primary-color);
        text-decoration: none;
    }

    .source-code a:hover {
        text-decoration: underline;
    }
</style>

<div class="about-me">
    <div class="info-header">
        <div class="avatar">
            <img src="https://s1.imagehub.cc/images/2025/07/27/40badc9974f073ad607dbbb0d1ee50e6.jpeg" alt="头像">
        </div>
        <div class="basic-info">
            <div class="name">揭涛英</div>
            <div class="affiliation">深圳职业技术大学</div>
        </div>
    </div>
    

    <h3 class="section-title">自我观照</h3>
    <p class="intro-paragraph">对真理的追求比对真理的占有更重要。</p>
    <p class="intro-paragraph"><strong>我始终相信，技术的价值在于解决问题，而成长的意义在于持续探索。</strong></p>
    <p class="intro-paragraph">闲暇时，我喜欢把学习中的思考、踩过的坑整理成笔记，也爱分享读书时的感悟。这个网站于我而言，更像一个「数字笔记本」，既为自己存档成长轨迹，也期待能给路过的朋友一点启发。</p>

    <h3 class="section-title">联系方式</h3>
    <ul class="contact-list">
        <li><i class="far fa-envelope"></i> 邮箱：<a href="mailto:2026125544@qq.com" class="highlight">2026125544@qq.com</a></li>
        <li><i class="fab fa-github"></i> GitHub：<a href="https://github.com/jie-taoying" class="highlight">jie-taoying</a></li>
        <li><i class="fab fa-weixin"></i> 微信：
            <span class="highlight">扫码添加</span>
            <div class="qrcode">
                <img src="https://s1.imagehub.cc/images/2025/07/26/4e9e072b52e963d4c149f3c3edca3388.jpg" alt="微信二维码">
            </div>
        </li>
    </ul>

    <div class="section-title">网站说明</div>
    <div class="site-info">
        <p>本网站使用MkDocs和Material主题构建，采用Markdown格式编写内容。所有文章和资料仅供学习交流使用，转载请注明出处。</p>
        <div class="source-code">
            <a href="#" class="highlight"><i class="fab fa-github"></i> 查看源代码</a>
        </div>
    </div>
</div>
    