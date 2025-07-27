<style>
    .book-system {
        max-width: 1200px;
        margin: 0 auto;
        padding: 20px;
        font-family: Arial, sans-serif;
    }
    .filter-bar {
        display: flex;
        flex-wrap: wrap;
        gap: 15px;
        margin: 0 0 20px 0;
        align-items: center;
    }
    .filter-item {
        display: flex;
        align-items: center;
        gap: 8px;
    }
    .filter-item select, .search-input {
        padding: 6px 10px;
        border: 1px solid #ddd;
        border-radius: 4px;
        font-size: 14px;
    }
    .search-input {
        flex: 1;
        min-width: 200px;
    }
    .count-box {
        margin-left: auto;
        padding: 6px 12px;
        background: #f5f5f5;
        border-radius: 4px;
        font-weight: 600;
        color: #333;
    }
    .book-list {
        display: flex;
        flex-direction: column;
        gap: 15px;
    }
    .book-item {
        display: flex;
        border: 1px solid #eee;
        border-radius: 8px;
        overflow: hidden;
        transition: box-shadow 0.2s;
        height: 240px;
    }
    .book-item:hover {
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    .book-cover {
        width: 160px;
        height: 100%;
        object-fit: cover;
        object-position: center;
    }
    .book-detail {
        padding: 16px;
        flex: 1;
        display: flex;
        flex-direction: column;
    }
    .book-title {
        margin: 0 0 12px 0;
        font-size: 18px;
        color: #222;
    }
    .book-meta {
        margin: 4px 0;
        font-size: 14px;
        color: #666;
    }
    .book-intro {
        margin-top: 10px;
        font-size: 13px;
        color: #555;
        line-height: 1.5;
        flex: 1;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 4;
        -webkit-box-orient: vertical;
    }
    .page-nav {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 10px;
        margin: 30px 0 0 0;
    }
    .page-btn {
        padding: 5px 12px;
        border: 1px solid #ddd;
        border-radius: 4px;
        background: #fff;
        cursor: pointer;
        font-size: 14px;
    }
    .page-btn:hover:not(:disabled) {
        border-color: #007bff;
        color: #007bff;
    }
    .page-btn.active {
        background: #007bff;
        color: white;
        border-color: #007bff;
    }
    .page-btn:disabled {
        opacity: 0.5;
        cursor: not-allowed;
    }
    .page-jump {
        display: flex;
        align-items: center;
        gap: 8px;
        font-size: 14px;
    }
    .page-jump input {
        width: 50px;
        padding: 4px;
        text-align: center;
        border: 1px solid #ddd;
        border-radius: 4px;
    }
    .loading {
        text-align: center;
        padding: 40px;
        color: #666;
        display: none;
    }
</style>

<div class="book-system">
    <div class="filter-bar">
        <div class="filter-item">
            <label>分类：</label>
            <select id="catFilter" onchange="filterBooks()">
                <option value="all">全部书籍</option>
                <option value="literature">文学</option>
                <option value="science">科学</option>
                <option value="history">历史</option>
                <option value="biography">传记</option>
                <option value="philosophy">哲学</option>
                <option value="art">艺术</option>
                <option value="technology">技术</option>
                <option value="economy">经济</option>
                <option value="education">教育</option>
                <option value="life">生活</option>
            </select>
        </div>
        <div class="filter-item">
            <label>搜索：</label>
            <input type="text" id="searchText" class="search-input" placeholder="书名/作者..." oninput="debouncedFilterBooks()">
        </div>
        <div class="count-box">
            总藏书：<span id="totalBooks">0</span> 本 | 当前显示：<span id="showingBooks">0</span> 本
        </div>
    </div>

    <div class="loading" id="loading">加载中...</div>
    <div class="book-list" id="bookContainer"></div>

    <div class="page-nav">
        <button class="page-btn" id="prevBtn" onclick="changePage(currentPage - 1)" disabled>上一页</button>
        <span id="pageInfo">第 1 页 / 共 0 页</span>
        <button class="page-btn" id="nextBtn" onclick="changePage(currentPage + 1)">下一页</button>
        <div class="page-jump">
            跳至：<input type="number" id="pageNum" min="1" value="1" onkeydown="if(event.key==='Enter') goToPage(this.value)">
        </div>
    </div>
</div>

<script>
    // 书籍数据
    const bookData = [
        {
            id: 1,
            title: "《文化苦旅》",
            author: "余秋雨",
            category: "literature",
            img: "https://s1.imagehub.cc/images/2025/07/27/06dda0d3b8539cdf6cc50f7a9e1561e5.jpeg",
            publisher: "作家出版社",
            pubTime: "1992年3月",
            intro: "以文化遗迹游历为线索，探索中国文化的厚重与魅力。书中通过对国内外多处文化古迹的实地考察，将历史、地理、文学等元素融为一体，展现了中华文明的博大精深和历史沧桑。"
        },
        {
            id: 2,
            title: "《我的几何人生：丘成桐自传》",
            author: "丘成桐、史蒂夫·纳迪斯",
            category: "biography",
            img: "https://s1.imagehub.cc/images/2025/07/27/7df130409d6754ff8659a35f9ace7f04.png",
            publisher: "译林出版社",
            pubTime: "2021年3月",
            intro: "国际知名数学家丘成桐的自传，讲述了他从香港的清贫少年成长为国际顶尖数学家的历程，展现了他在数学领域的探索与成就，以及对中国数学发展的贡献。"
        },
        {
            id: 3,
            title: "《核铀国魂：揭开中国铀矿采冶的神秘面纱》",
            author: "杨勤良",
            category: "history",
            img: "https://s1.imagehub.cc/images/2025/07/27/4cefeeb84d072a8ffa03d98c8bf9ab0b.jpeg",
            publisher: "中国原子能出版社",
            pubTime: "2025年4月"
        },
        {
            id: 4,
            title: "《人类简史》",
            author: "尤瓦尔·赫拉利",
            category: "history",
            img: "https://img.alicdn.com/bao/uploaded/i1/2455255647/O1CN011cCVwk1raNLFLKYYt_!!0-item_pic.jpg",
            publisher: "浙江人民出版社",
            pubTime: "2017年7月"
        },
        {
            id: 5,
            title: "《如何阅读一本书》",
            author: "莫提默·J. 艾德勒、查尔斯·范多伦",
            category: "education",
            img: "https://img.alicdn.com/i4/684387459/O1CN01u6N7zF24yGmQk3ueb_!!684387459.jpg",
            publisher: "商务印书馆",
            pubTime: "2004年1月"
        },
        {
            id: 6,
            title: "《美的历程》",
            author: "李泽厚",
            category: "art",
            img: "https://img.alicdn.com/i1/1961939153/O1CN015q6pU52HU7oYW9OJy_!!1961939153.png",
            publisher: "生活·读书·新知三联书店",
            pubTime: "2009年7月"
        },
        {
            id: 7,
            title: "《活着》",
            author: "余华",
            category: "literature",
            img: "https://www.szlib.org.cn/api/opacservice/getBookCover?metatable=80000045&metaid=342183",
            publisher: "作家出版社",
            pubTime: "2012年8月",
            intro: "讲述了中国农村一个普通人福贵的一生，通过他经历的种种苦难，探讨了生命的意义和价值，展现了中国人在苦难中的坚韧与生命力。"
        },
        {
            id: 8,
            title: "《纲鉴易知录》",
            author: "吴乘权等",
            category: "history", // 历史
            img: "https://img3m2.ddimg.cn/69/8/29139432-2_e_1697972962.jpg",
            publisher: "中华书局",
            pubTime: "2016 年 6 月",
            intro: "一部简明的中国通史，上起盘古开天辟地的神话时代，下迄明王朝灭亡，是了解中国历史的经典入门读物。"
        },
        {
            id: 9,
            title: "《穷查理宝典》",
            author: "彼得·考夫曼",
            category: "economy", // 经济
            img: "https://img.yzcdn.cn/upload_files/2019/12/21/FkIanX7Wh1YAseSOGUr0rdTYrlQh.jpg?imageView2/2/w/750/h/0/q/75/format/jpg",
            publisher: "中信出版社",
            pubTime: "2016年8月",
            intro: "收录了查理·芒格的个人传记与投资哲学，以及过去20年来芒格主要的公开演讲和媒体访谈，是了解查理·芒格投资思想的必读之作。"
        },
        {
            id: 10,
            title: "《论语别裁》",
            author: "南怀瑾",
            category: "philosophy",
            img: "https://s1.imagehub.cc/images/2025/07/27/e8c7935201fea151e4155d093960edf6.png",
            publisher: "复旦大学出版社",
            pubTime: "2016年8月",
            intro: "南怀瑾先生以渊博的学识，独特的视角，对《论语》进行了别开生面的解读，打破了两千年来对《论语》的章句注释传统，为读者展现了《论语》所蕴含的人生智慧和处世之道。"
        }
    ];

    /*  书籍数据格式：
        {
        id: 0, // 书籍唯一标识，每次添加新书籍时递增
        title: "书籍名称", // 书名
        author: "作者", // 作者
        category: "类别", // 书籍类别，如literature、science等
        img: "图片链接", // 封面图片链接
        publisher: "出版社", // 出版社
        pubTime: "出版时间", // 出版年月
        intro: "书籍简介" // 书籍内容简介
        }   
    */

    // 系统变量
    let currentPage = 1;
    const pageSize = 6;
    let filteredData = [...bookData];

    // 初始化
    function init() {
        updateCount();
        renderBookList();
        updatePageInfo();
    }

    // 筛选书籍
    function filterBooks() {
        const cat = document.getElementById("catFilter").value;
        const keyword = document.getElementById("searchText").value.trim().toLowerCase();

        filteredData = bookData.filter(book => {
            const catMatch = cat === "all" || book.category === cat;
            const keywordMatch = keyword === "" 
                ? true 
                : book.title.toLowerCase().includes(keyword) || book.author.toLowerCase().includes(keyword);
            return catMatch && keywordMatch;
        });

        currentPage = 1;
        document.getElementById("pageNum").value = 1;
        renderBookList();
        updateCount();
        updatePageInfo();
    }

    // 防抖函数
    function debounce(fn, delay) {
        let timer;
        return function() {
            clearTimeout(timer);
            timer = setTimeout(() => fn.apply(this, arguments), delay);
        };
    }

    // 创建防抖后的搜索函数
    const debouncedFilterBooks = debounce(filterBooks, 300);

    // 渲染书籍列表
    function renderBookList() {
        const start = (currentPage - 1) * pageSize;
        const end = start + pageSize;
        const currentBooks = filteredData.slice(start, end);

        document.getElementById("loading").style.display = "block";
        document.getElementById("bookContainer").innerHTML = "";

        setTimeout(() => {
            let html = "";
            currentBooks.forEach(book => {
                html += `
                    <div class="book-item">
                        <img src="${book.img}" class="book-cover" alt="${book.title}">
                        <div class="book-detail">
                            <h3 class="book-title">${book.title}</h3>
                            <p class="book-meta">作者：${book.author}</p>
                            ${book.publisher ? `<p class="book-meta">出版社：${book.publisher}</p>` : ""}
                            ${book.pubTime ? `<p class="book-meta">出版时间：${book.pubTime}</p>` : ""}
                            <p class="book-meta">分类：${getCategoryName(book.category)}</p>
                            ${book.intro ? `<p class="book-intro">${book.intro}</p>` : ""}
                        </div>
                    </div>
                `;
            });
            document.getElementById("bookContainer").innerHTML = html;
            document.getElementById("loading").style.display = "none";
        }, 200);
    }

    // 获取分类名称
    function getCategoryName(category) {
        const categoryMap = {
            'literature': '文学',
            'science': '科学',
            'history': '历史',
            'biography': '传记',
            'philosophy': '哲学',
            'art': '艺术',
            'technology': '技术',
            'economy': '经济',
            'education': '教育',
            'life': '生活'
        };
        return categoryMap[category] || category;
    }

    // 分页控制
    function changePage(page) {
        const totalPages = Math.ceil(filteredData.length / pageSize) || 1;
        if (page < 1 || page > totalPages) return;

        currentPage = page;
        document.getElementById("pageNum").value = page;
        renderBookList();
        updatePageInfo();
    }

    function goToPage(page) {
        changePage(parseInt(page));
    }

    // 更新计数
    function updateCount() {
        document.getElementById("totalBooks").textContent = bookData.length;
        document.getElementById("showingBooks").textContent = filteredData.length;
    }

    // 更新分页信息
    function updatePageInfo() {
        const totalPages = Math.ceil(filteredData.length / pageSize) || 1;
        document.getElementById("pageInfo").textContent = `第 ${currentPage} 页 / 共 ${totalPages} 页`;
        document.getElementById("prevBtn").disabled = currentPage === 1;
        document.getElementById("nextBtn").disabled = currentPage === totalPages;
    }

    init();
</script>