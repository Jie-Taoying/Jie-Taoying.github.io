<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>语录集</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: '#2563eb',
          },
        },
      }
    }
  </script>
  <style type="text/tailwindcss">
    @layer utilities {
      .quote-card {
        @apply bg-white rounded-lg p-4 shadow-sm border border-gray-100 mb-4 transition-all hover:shadow-md;
      }
      .pagination-btn {
        @apply px-3 py-1 border border-gray-300 rounded hover:bg-primary hover:text-white hover:border-primary transition-colors;
      }
      .pagination-btn.active {
        @apply bg-primary text-white border-primary;
      }
    }
  </style>
</head>
<body class="bg-gray-50">
  <div class="container mx-auto px-4 py-8 max-w-5xl">
    <!-- 标题区域 -->
    <header class="mb-8 text-center">
      <h1 class="text-3xl font-bold text-gray-800 mb-2">语录集</h1>
      <p class="text-gray-600">精选名人名言与智慧感悟</p>
    </header>

    <!-- 搜索和筛选 -->
    <div class="bg-white p-4 rounded-lg shadow-sm mb-6">
      <div class="flex flex-col sm:flex-row gap-4">
        <div class="relative flex-grow">
          <input type="text" id="search-input" placeholder="搜索语录内容或出处..." 
                 class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-primary/50 focus:border-primary outline-none">
          <i class="fa fa-search absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
        </div>
        
        <div class="flex gap-2">
          <select id="source-filter" class="border border-gray-300 rounded-lg px-3 py-2 focus:ring-2 focus:ring-primary/50 focus:border-primary outline-none">
            <option value="">所有出处</option>
            <!-- 动态生成出处选项 -->
          </select>
          
          <select id="rows-per-page" class="border border-gray-300 rounded-lg px-3 py-2 focus:ring-2 focus:ring-primary/50 focus:border-primary outline-none">
            <option value="10">10条/页</option>
            <option value="20" selected>20条/页</option>
            <option value="50">50条/页</option>
          </select>
        </div>
      </div>
    </div>

    <!-- 语录内容区域 -->
    <div id="quotes-container">
      <!-- 语录内容将动态生成 -->
    </div>

    <!-- 分页控件 -->
    <div id="pagination" class="mt-8 flex flex-col sm:flex-row justify-between items-center gap-4">
      <div class="text-sm text-gray-600">
        显示 <span id="showing-range">1-20</span> 条，共 <span id="total-count">0</span> 条
      </div>
      
      <div class="flex items-center gap-2">
        <button id="prev-page" class="pagination-btn" disabled>
          <i class="fa fa-angle-left"></i> 上一页
        </button>
        
        <div id="page-numbers" class="flex items-center gap-1">
          <!-- 页码将动态生成 -->
        </div>
        
        <button id="next-page" class="pagination-btn" disabled>
          下一页 <i class="fa fa-angle-right"></i>
        </button>
      </div>
    </div>
  </div>

  <script>
    // 语录数据 - 实际使用时可根据需要扩展
    const quotesData = [
      { id: 1, content: "一个人一辈子，做一件事情对社会大众有贡献，对国家民族，对整个的社会，都是一种贡献，这才算是事业。", source: "南怀瑾" },
      { id: 2, content: "我总是在高速路上抛锚。当我站在路边试图拦车帮忙时，没有人会停下来。但当我开始推自己的车，很多司机会自然而然的下车和我一起推。如果你需要帮助，请先帮助自己。", source: "Chirs Rock" },
      { id: 3, content: "一个人的意志是不能被打倒的，不管什么辛苦，我一旦决定要做的，我会克服万难，我不会逃避，不会退缩，但是我不会死拼。我告诉各位，一个有成就的人，都是经历挫折，备受考验的人，否则他不可能有大成就。", source: "曾仕强" },
      { id: 4, content: "一个人可以掩饰和伪装自己的行为动机，却无法掩饰和伪装自己的生命格调。", source: "《中国文脉》" },
      { id: 5, content: "一切文化都会沉淀为人格。", source: "荣格" },
      { id: 6, content: "纵浪大化中，不喜亦不惧。<br>应尽便须尽，无复独多虑。", source: "陶渊明" },
      { id: 7, content: "一切重要的图书都应该立即重读。", source: "叔本华" },
      { id: 8, content: "只要改变自己看世界的眼光，数学就会在你眼前出现。寻找数字是迷人的、永无止境的过程。", source: "《万物皆数》" },
      { id: 9, content: "人生就像滚雪球，重要的是找到很湿的雪和很长的坡。", source: "巴菲特" },
      { id: 10, content: "世界上只有一种真正的英雄主义，那就是看清生活的真相之后，依然热爱生活。", source: "罗曼罗兰" },
      { id: 11, content: "灵光独耀，迥脱根尘，体露真常，不拘文字，心性无染，本自圆成，但离妄缘，即如如佛。", source: "《南禅七日》" },
      { id: 12, content: "不管是顺境也好、逆境也好，不管自己处在何种境遇，都要抱着积极的心态朝前看，任何时候都要拼命工作，持续努力，这才是最重要的。", source: "稻盛和夫" },
      { id: 13, content: "奋斗可能很困难，但是我觉得最喜乐的事情莫过于经过长期的奋斗以后达到的目标，做学问和学习都少不了这种奋斗，我希望年轻人也能体会这份辛苦，收获这份喜悦。", source: "丘成桐" },
      { id: 14, content: "衣食两般皆俱足，又思娇娥美貌妻；<br>娶得美妻生下子，恨无田地少根基；<br>良田置的多广阔，出门又嫌少马骑；<br>槽头扣了骡和马，恐无官职被人欺；<br>七品县官还嫌小，又想朝中挂紫衣；<br>一品当朝为宰相，还想山河夺帝基；<br>心满意足为天子，又想长生不老期；<br>一旦求得长生药，再跟上帝论高低。<br>不足不足不知足，人生人生奈若何？<br>若要世人心满足，除非南柯一梦兮。", source: "《不足歌》朱载育" },
      { id: 15, content: "莫名其妙的生来，无可奈何的活着，不知所以然的死去。", source: "南怀瑾" },
      { id: 16, content: "无限次地重复某个随机试验，最终的平均结果必然不再是随机的，而是无限接近一个极限值。", source: "《万物皆数》" },
      { id: 17, content: "我手里只要有一本书，就不会觉得浪费时间。", source: "查理·芒格" },
      { id: 18, content: "Most of the important things in the world have been accomplished by people who have kept on trying when there seemed to be no hope at all.", source: "戴尔·卡内基" },
      { id: 19, content: "不去行动会滋生怀疑和恐惧，行动会孕育自信和勇气，如果您想克服恐惧，不要坐在家里思考，走出去让自己忙碌起来。", source: "卡内基" },
      { id: 20, content: "成功的人会从他的错误中获益，并且以不同的方式去努力奋斗。", source: "卡内基" },
      // 可以继续添加更多语录...
    ];

    // 状态管理
    const state = {
      currentPage: 1,
      rowsPerPage: 20,
      searchTerm: '',
      sourceFilter: '',
      filteredQuotes: [...quotesData]
    };

    // DOM元素
    const elements = {
      quotesContainer: document.getElementById('quotes-container'),
      searchInput: document.getElementById('search-input'),
      sourceFilter: document.getElementById('source-filter'),
      rowsPerPage: document.getElementById('rows-per-page'),
      prevPage: document.getElementById('prev-page'),
      nextPage: document.getElementById('next-page'),
      pageNumbers: document.getElementById('page-numbers'),
      showingRange: document.getElementById('showing-range'),
      totalCount: document.getElementById('total-count')
    };

    // 初始化
    function init() {
      renderQuotes();
      renderPagination();
      populateSourceFilter();
      setupEventListeners();
    }

    // 渲染语录列表
    function renderQuotes() {
      // 过滤数据
      filterQuotes();
      
      const totalPages = Math.ceil(state.filteredQuotes.length / state.rowsPerPage);
      const startIndex = (state.currentPage - 1) * state.rowsPerPage;
      const endIndex = Math.min(startIndex + state.rowsPerPage, state.filteredQuotes.length);
      const currentPageQuotes = state.filteredQuotes.slice(startIndex, endIndex);
      
      // 更新统计信息
      elements.totalCount.textContent = state.filteredQuotes.length;
      elements.showingRange.textContent = state.filteredQuotes.length > 0 
        ? `${startIndex + 1}-${endIndex}` 
        : '0-0';
      
      // 清空容器
      elements.quotesContainer.innerHTML = '';
      
      // 无数据状态
      if (currentPageQuotes.length === 0) {
        elements.quotesContainer.innerHTML = `
          <div class="text-center py-12 text-gray-500">
            <i class="fa fa-search text-3xl mb-2"></i>
            <p>没有找到匹配的语录</p>
          </div>
        `;
        return;
      }
      
      // 渲染语录卡片
      currentPageQuotes.forEach((quote, index) => {
        const quoteCard = document.createElement('div');
        quoteCard.className = 'quote-card';
        quoteCard.innerHTML = `
          <div class="flex items-start gap-4">
            <div class="text-primary font-bold mt-1">${startIndex + index + 1}.</div>
            <div class="flex-grow">
              <div class="prose max-w-none text-gray-700 mb-2" style="font-size: 1rem;">
                ${quote.content.replace(/\n/g, '<br>')}
              </div>
              <div class="text-right text-gray-500 text-sm italic">—— ${quote.source}</div>
            </div>
          </div>
        `;
        elements.quotesContainer.appendChild(quoteCard);
      });
      
      // 更新分页按钮状态
      elements.prevPage.disabled = state.currentPage === 1 || totalPages === 0;
      elements.nextPage.disabled = state.currentPage === totalPages || totalPages === 0;
    }

    // 渲染分页
    function renderPagination() {
      const totalPages = Math.ceil(state.filteredQuotes.length / state.rowsPerPage);
      elements.pageNumbers.innerHTML = '';
      
      // 最多显示5个页码
      let startPage = Math.max(1, state.currentPage - 2);
      let endPage = Math.min(totalPages, startPage + 4);
      
      // 调整页码范围
      if (endPage - startPage < 4 && totalPages > 5) {
        startPage = Math.max(1, endPage - 4);
      }
      
      // 添加上一页省略号
      if (startPage > 1) {
        addPageNumber(1);
        if (startPage > 2) {
          addEllipsis();
        }
      }
      
      // 添加页码
      for (let i = startPage; i <= endPage; i++) {
        addPageNumber(i);
      }
      
      // 添加下一页省略号
      if (endPage < totalPages) {
        if (endPage < totalPages - 1) {
          addEllipsis();
        }
        addPageNumber(totalPages);
      }
    }

    // 添加页码按钮
    function addPageNumber(page) {
      const button = document.createElement('button');
      button.className = `pagination-btn ${page === state.currentPage ? 'active' : ''}`;
      button.textContent = page;
      button.addEventListener('click', () => {
        state.currentPage = page;
        renderQuotes();
        renderPagination();
        window.scrollTo({ top: 0, behavior: 'smooth' });
      });
      elements.pageNumbers.appendChild(button);
    }

    // 添加省略号
    function addEllipsis() {
      const ellipsis = document.createElement('span');
      ellipsis.className = 'px-2 text-gray-500';
      ellipsis.textContent = '...';
      elements.pageNumbers.appendChild(ellipsis);
    }

    // 过滤语录
    function filterQuotes() {
      state.filteredQuotes = quotesData.filter(quote => {
        const matchesSearch = state.searchTerm === '' 
          ? true 
          : quote.content.toLowerCase().includes(state.searchTerm.toLowerCase()) || 
            quote.source.toLowerCase().includes(state.searchTerm.toLowerCase());
        
        const matchesSource = state.sourceFilter === '' 
          ? true 
          : quote.source === state.sourceFilter;
        
        return matchesSearch && matchesSource;
      });
    }

    // 填充出处筛选
    function populateSourceFilter() {
      const sources = [...new Set(quotesData.map(quote => quote.source))];
      sources.forEach(source => {
        const option = document.createElement('option');
        option.value = source;
        option.textContent = source;
        elements.sourceFilter.appendChild(option);
      });
    }

    // 设置事件监听
    function setupEventListeners() {
      // 搜索
      elements.searchInput.addEventListener('input', (e) => {
        state.searchTerm = e.target.value;
        state.currentPage = 1;
        renderQuotes();
        renderPagination();
      });
      
      // 出处筛选
      elements.sourceFilter.addEventListener('change', (e) => {
        state.sourceFilter = e.target.value;
        state.currentPage = 1;
        renderQuotes();
        renderPagination();
      });
      
      // 每页显示数量
      elements.rowsPerPage.addEventListener('change', (e) => {
        state.rowsPerPage = parseInt(e.target.value);
        state.currentPage = 1;
        renderQuotes();
        renderPagination();
      });
      
      // 上一页
      elements.prevPage.addEventListener('click', () => {
        if (state.currentPage > 1) {
          state.currentPage--;
          renderQuotes();
          renderPagination();
          window.scrollTo({ top: 0, behavior: 'smooth' });
        }
      });
      
      // 下一页
      elements.nextPage.addEventListener('click', () => {
        const totalPages = Math.ceil(state.filteredQuotes.length / state.rowsPerPage);
        if (state.currentPage < totalPages) {
          state.currentPage++;
          renderQuotes();
          renderPagination();
          window.scrollTo({ top: 0, behavior: 'smooth' });
        }
      });
    }

    // 初始化页面
    document.addEventListener('DOMContentLoaded', init);
  </script>
</body>
</html>
