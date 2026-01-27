# Architecture & Flow
> This diagram shows the full rendering pipeline from browser request to Trilium share page,
> with the Share CSS treated as an independent styling architecture.
```mermaid
flowchart LR
    subgraph ShareCSS[Custom Share CSS]
        direction TB
        CustomCSS[User-injected Custom CSS]
        RootVars["1. :root Variables\n(Typora Light Theme)"]
        GlobalReset["2. Global Reset\n(scrollbar / font / links)"]
        Layout["3. Layout System\n(flex row-reverse + full-width)"]
        AutoHide["4. Auto-hide Sidebar\n(10px → 280px on hover)"]
        Typo["5. Typography\n(Typora-style headings)"]
        CodeTable["6. Code & Tables\n(highlight / zebra)"]
        Responsive["7. Responsive Rules\n(mobile folding)"]

        CustomCSS --> RootVars --> GlobalReset --> Layout
        Layout --> AutoHide
        Layout --> Typo --> CodeTable
        Layout --> Responsive
    end

    subgraph Trilium[Trilium Server]
        direction TB
        Server[Trilium Server]
        NoteDB[(Notes Database)]
        ShareLogic[Share Page Generation]
        DefaultCSS[Built-in Default Share CSS]

        Server --> NoteDB
        Server --> ShareLogic
        ShareLogic --> DefaultCSS
    end

    subgraph Browser[Browser]
        direction TB
        UA[User Agent]
        RenderEngine[Rendering Engine]
        UA --> RenderEngine
    end   
    
    ShareCSS -->|"Applies final styles"| Trilium
    Trilium --> |"Invode Custom Css"| ShareCSS
    Trilium -->|"Serves HTML + <style>"| Browser
    Browser -->|"Requests shared note"| Trilium

    classDef css      fill:#e8f5ff,stroke:#1e88e5,stroke-width:4px,color:#0d47a1
    classDef trilium  fill:#fff8e1,stroke:#f57c00,stroke-width:4px,color:#e65100
    classDef browser  fill:#e8f5e9,stroke:#2e7d32,stroke-width:4px,color:#1b5e20

    class ShareCSS css
    class Trilium trilium
    class Browser browser
```

# Features
## English Description
| Category      | Feature                         | Description                                                                                       |
| ------------- | ------------------------------- | ------------------------------------------------------------------------------------------------- |
| Theme         | Typora-style Light Theme        | Recreates Typora classic light appearance with white background, dark gray text, and blue accents |
| Layout        | Width Unlock + Centered Reading | Removes Trilium width limits and keeps content centered at ~80% viewport width                    |
| Navigation    | Auto-hide Sidebar               | Sidebar collapses to a thin strip and expands smoothly on hover                                   |
| Layout        | Two-column Structure            | Left menu + right content area using Flexbox                                                      |
| Typography    | Typora-like Headings            | Gradient H1, accent-bar H2/H3 with clear hierarchy                                                |
| Code          | Light Code Blocks               | Soft gray code blocks with Typora-style syntax colors                                             |
| Tables        | Enhanced Tables                 | Zebra striping, hover highlight, horizontal scrolling                                             |
| Scrollbar     | Light Scrollbars                | Wide, light-themed scrollbars optimized for readability                                           |
| Compatibility | Style Override Friendly         | Key `!important` rules removed to preserve note-level custom styles                               |
| Responsive    | Mobile Adaptation               | Switches to vertical layout on small screens                                                      |


## Chinese Description
| 功能分类 | 功能点           | 说明                                |
| ---- | ------------- | --------------------------------- |
| 主题风格 | Typora 经典浅色风格 | 复刻 Typora 默认浅色主题：白色背景、深灰文字、蓝色强调   |
| 布局策略 | 宽度限制解除 + 居中阅读 | 解除 Trilium 默认宽度限制，正文约 80% 视口宽度并居中 |
| 目录导航 | 侧边栏自动隐藏       | 目录栏默认极窄显示，鼠标悬停自动展开                |
| 页面结构 | 双栏布局          | 左侧目录（Menu）+ 右侧正文（Main）            |
| 排版体系 | Typora 风格标题   | H1 渐变标题，H2/H3 左侧强调条，层级清晰          |
| 代码展示 | 浅色代码块         | 浅灰代码背景，Typora 风格配色，降低视觉负担         |
| 表格增强 | 表格斑马纹与悬停      | 支持斑马纹、行悬停高亮、横向滚动                  |
| 细节优化 | 浅色滚动条         | 加宽滚动条，适配浅色主题与触控操作                 |
| 兼容性  | 保留原笔记样式       | 刻意移除部分 `!important`，允许原笔记样式覆盖     |
| 响应式  | 移动端适配         | 小屏幕自动切换为纵向布局                      |


# install
1. copy a css file code
2. trilium create CSS note, and paste code
3. share root note add a New Relation: shareCss, isInheritable, value = CSS note
4. sync and new share css work!
<img width="1924" height="970" alt="image" src="https://github.com/user-attachments/assets/e3b399c4-28ce-478e-b146-87c3f22fb006" />

# demo

## ATHidesideTypora

### default share view
<img width="2539" height="1238" alt="cat_2026-01-27_131957" src="https://github.com/user-attachments/assets/27c0d9f3-a0c6-4c7f-bb91-049d45ff019a" />

### mouse move and auto expand sidebar view
<img width="2497" height="1260" alt="cat_2026-01-27_131950" src="https://github.com/user-attachments/assets/0a87f895-6c4a-4582-b942-177ad01281ce" />

### change chrome window width view
<img width="1533" height="1254" alt="cat_2026-01-27_132017" src="https://github.com/user-attachments/assets/4a70f340-59ea-44aa-b141-fd25f22559f0" />

## ATHidesideBlack

### default share view
<img width="2550" height="1260" alt="cat_2026-01-27_132736" src="https://github.com/user-attachments/assets/0d1c4012-c4be-4e74-9cd3-08f7b6c535ef" />

### mouse move and auto expand sidebar view
<img width="2531" height="1237" alt="cat_2026-01-27_132748" src="https://github.com/user-attachments/assets/fe618e4d-0f95-4430-b4ef-4d9d54298214" />

### change chrome window width view
<img width="1413" height="1109" alt="cat_2026-01-27_132755" src="https://github.com/user-attachments/assets/2d26cf38-e677-412f-a546-393831d21c8d" />
