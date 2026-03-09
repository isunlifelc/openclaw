# Skill Name: Daily AI News Demo


## Reference Template (Golden Sample)

```html


<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>设计旅人 - AI 日报</title>
    
    <!-- 引入 Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- 引入 Font Awesome 图标库 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'brand-yellow': '#facc15', 
                        'brand-black': '#050505',
                        'bg-gray': '#f5f7fa',
                    },
                    fontFamily: {
                        // 强制使用苹方作为首选字体
                        'sans': ['"PingFang SC"', '"PingFang TC"', '"Microsoft YaHei"', '"Helvetica Neue"', 'Arial', 'sans-serif'],
                    },
                    boxShadow: {
                        'card': '0 4px 20px -2px rgba(0, 0, 0, 0.05)',
                        'card-hover': '0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.01)',
                    }
                }
            }
        }
    </script>

    <style>
        /* 全局样式优化 */
        html {
            scroll-behavior: smooth;
            -webkit-font-smoothing: antialiased; /* macOS 字体抗锯齿 */
            -moz-osx-font-smoothing: grayscale;
        }
        
        body {
            background-color: #f0f2f5;
            font-family: "PingFang SC", "Microsoft YaHei", sans-serif;
        }

        /* 隐藏移动端滚动条但保留功能，美化桌面端滚动条 */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1; 
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1; 
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8; 
        }

        /* 链接交互动画 */
        .link-hover {
            position: relative;
            text-decoration: none;
            color: #3b82f6;
            transition: color 0.3s ease;
        }
        .link-hover::after {
            content: '';
            position: absolute;
            width: 100%;
            height: 1px;
            bottom: -2px;
            left: 0;
            background-color: #3b82f6;
            transform: scaleX(0);
            transform-origin: bottom right;
            transition: transform 0.3s ease-out;
        }
        .link-hover:hover::after {
            transform: scaleX(1);
            transform-origin: bottom left;
        }

        /* 文本溢出省略 */
        .line-clamp-2 {
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
        }

        /* 简单的入场动画 */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translate3d(0, 10px, 0);
            }
            to {
                opacity: 1;
                transform: none;
            }
        }
        .animate-fade-in-up {
            animation: fadeInUp 0.6s ease-out forwards;
        }
    </style>
</head>
<body class="text-gray-800 font-sans min-h-screen flex flex-col items-center">

    <!-- 主容器 -->
    <div id="app" class="w-full max-w-2xl bg-white shadow-2xl min-h-screen flex flex-col relative overflow-hidden">
        
        <!-- 页头区域：高度改为 120px -->
        <header class="relative w-full h-[120px] overflow-hidden group bg-black">
            <!-- 背景图片：移除蒙层，直接展示，位置调整为 right center 以保证左侧黑色区域留给文字 -->
            <div class="absolute inset-0 w-full h-full">
                <img src="https://img.alicdn.com/imgextra/i2/O1CN01rjre2R1SxPE7V7LNK_!!6000000002313-0-tps-1200-422.jpg" 
                     alt="Header Background" 
                     class="w-full h-full object-cover object-[right_center] transition-transform duration-700 group-hover:scale-105">
            </div>
            
            <!-- 标题内容 -->
            <div class="relative z-10 h-full flex flex-col justify-center px-6">
                <div class="border-brand-yellow pl-4 animate-fade-in-up flex flex-col justify-center h-full py-2">
                    <!-- 主标题：字号 40px，紧凑行高 -->
                    <h1 class="text-[40px] leading-none font-bold text-white tracking-wide mb-2 drop-shadow-md">
                        设计旅人
                    </h1>
                    <!-- 日期：小字 -->
                    <p class="text-gray-400 text-xs font-normal tracking-widest uppercase flex items-center gap-2">
                        <i class="fa-regular fa-calendar-days text-brand-yellow text-[10px]"></i>
                        <span id="current-date">Fetching Date...</span>
                    </p>
                </div>
            </div>
        </header>

        <!-- 资讯列表内容区域 -->
        <main class="flex-1 px-4 py-6 bg-gray-50">
            <!-- 栏目标题 -->
            <div class="flex items-center gap-2 mb-5 ml-1">
                <div class="w-1 h-5 bg-black rounded-full"></div>
                <h2 class="text-lg font-bold text-gray-900 tracking-wide">今日 AI 资讯</h2>
            </div>

            <!-- 动态生成的资讯列表容器 -->
            <div id="news-container" class="space-y-4">
                <!-- Javascript will populate this -->
            </div>
        </main>

        <!-- 底部 -->
        <footer class="w-full bg-black py-8 flex justify-center items-center mt-auto">
            <!-- 底部黑色色块与低调水印 -->
            <div class="text-center">
                <p class="text-[10px] font-bold tracking-[0.3em] text-[#222222] uppercase" style="text-shadow: 0 1px 0 rgba(255,255,255,0.03);">
                    by LClaw AI
                </p>
            </div>
        </footer>

    </div>

    <!-- JavaScript 逻辑 -->
    <script>
        // 1. 设置日期逻辑
        function setDate() {
            const dateElement = document.getElementById('current-date');
            const now = new Date();
            const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
            // 格式化为 YYYY.MM.DD
            const formattedDate = now.toLocaleDateString('zh-CN', options).replace(/\//g, '.');
            
            // 获取星期
            const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
            const weekDay = days[now.getDay()];

            dateElement.innerHTML = `${formattedDate} <span class="mx-1 text-brand-yellow/50">|</span> ${weekDay}`;
        }

        // 2. 模拟 AI 资讯数据 (保持完整数据)
        const newsData = [
            {
                id: 1,
                title: "OpenAI 发布 GPT-5 预览版开发计划",
                summary: "OpenAI 官方博客透露，新一代模型将重点提升逻辑推理与多模态交互能力，预计将于年底对开发者开放内测申请。",
                link: "#"
            },
            {
                id: 2,
                title: "Midjourney V7 引入全新色彩控制引擎",
                summary: "最新版本更新支持更精准的十六进制色彩代码输入，并优化了人像手指细节处理，设计师工作流效率提升显著。",
                link: "#"
            },
            {
                id: 3,
                title: "谷歌 DeepMind 医疗 AI 突破性进展",
                summary: "AlphaFold 新变体在蛋白质结构预测中准确率达到98%，有望加速癌症靶向药物的研发周期，相关论文已登顶 Nature。",
                link: "#"
            },
            {
                id: 4,
                title: "Adobe Firefly 集成至 Photoshop 网页版",
                summary: "无需安装客户端，用户现可直接在浏览器中使用生成式填充功能，并支持商用版权保护，创意设计更加灵活。",
                link: "#"
            },
            {
                id: 5,
                title: "欧盟正式通过全球首部《人工智能法案》",
                summary: "法案对高风险 AI 系统提出了严格的数据透明度要求，违规企业最高将面临全球营业额 7% 的巨额罚款。",
                link: "#"
            }
        ];

        // 3. 渲染资讯列表
        function renderNews() {
            const container = document.getElementById('news-container');
            let htmlContent = '';

            newsData.forEach((item, index) => {
                htmlContent += `
                    <article class="group bg-white rounded-xl p-4 md:p-5 shadow-[0_2px_10px_-4px_rgba(0,0,0,0.05)] border border-gray-100 hover:shadow-card-hover hover:-translate-y-1 transition-all duration-300 cursor-default">
                        <div class="flex justify-between items-start mb-2">
                            <h3 class="text-[17px] font-bold text-gray-800 leading-snug group-hover:text-blue-600 transition-colors">
                                ${item.title}
                            </h3>
                            <span class="text-xs font-mono text-gray-300 ml-3 mt-1 shrink-0">0${index + 1}</span>
                        </div>
                        <p class="text-[13px] text-gray-500 mb-3 leading-relaxed line-clamp-2 text-justify">
                            ${item.summary}
                        </p>
                        <div class="flex justify-between items-center border-t border-dashed border-gray-100 pt-3 mt-1">
                            <span class="text-[10px] text-gray-400 font-medium bg-gray-50 px-2 py-0.5 rounded-sm">AI Daily</span>
                            <a href="${item.link}" class="text-xs font-semibold link-hover flex items-center gap-1 group/link text-blue-500">
                                阅读原文 <i class="fa-solid fa-arrow-right -rotate-45 group-hover/link:rotate-0 transition-transform duration-300 text-[10px] scale-75"></i>
                            </a>
                        </div>
                    </article>
                `;
            });

            container.innerHTML = htmlContent;
        }

        // 初始化执行
        document.addEventListener('DOMContentLoaded', () => {
            setDate();
            renderNews();
        });
    </script>
</body>
</html>



