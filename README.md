# 工具v10.3.7 - 搜索与下载修复版(description not updated)

您好！这是一个用于从 **咪咕音乐** 和 **网易云音乐** 下载歌曲的Python脚本。

`v10.3.7` 版本是一个重要的技术修复版。它针对两大平台当前最新的网站结构和接口规范，重写了核心的搜索与下载逻辑。

> **⚠️ 请注意：此版本强制要求登录咪咕和网易云音乐账号才能使用全部功能。** 这是一个必要的前提，未登录将导致脚本无法正常搜索或下载。

## v10.3.7 版本核心更新与技术说明

### 1. 咪咕音乐 (Migu Music) - 搜索策略重构

#### 搜索机制 (重要使用须知)

- **严格的歌手名搜索**:
    
    - **使用要求**: 在搜索咪咕音乐时，**输入框中必须填写您要查找的、准确无误的歌手名**。本版本不支持模糊搜索、错别字或搜索歌曲名。
    - **技术原因**: 咪咕官网直接通过歌曲名搜索，返回的结果数量非常有限（通常只有一页）。为了能获取一位歌手的全部歌曲，`v10.3.7` 版本采取了“直达歌手主页”的策略。脚本会将您输入的**全部内容**当作一个精确的歌手名进行查找。如果找不到完全匹配的歌手，搜索就会失败。
- **技术实现流程**:
    
    1. 脚本接收到您输入的 **精确歌手名**。
    2. 自动访问搜索页面，并模拟浏览器点击切换到 **“歌手”** 分类。
    3. 在歌手列表中，通过文本匹配找到与您输入 **完全一致** 的歌手，并模拟点击进入其个人主页。
    4. 在歌手的个人主页上，歌曲列表是完整呈现的。脚本会在这里解析出所有歌曲信息，供您浏览和下载。

### 2. 网易云音乐 (Netease Cloud Music) - API直连与高质量封面

- **稳定的官方API**:
    
    - `v10.3.7` 版本遵循了 `binaryify/NeteaseCloudMusicApi` 等知名开源项目的研究成果，优先使用官方稳定且数据丰富的 `cloudsearch` 接口 (`/weapi/cloudsearch/get/web`)。
    - **优势**: 相比于在随时可能变动的前端页面上“扒数据”，直接与官方API交互可以获得结构化的 `JSON` 数据。这意味着返回的信息更完整（包含ID、时长、多种音质等级等），并且程序运行更稳定。
- **高质量封面获取策略**:
    
    - **技术流程**:
        1. 首先，脚本会检查API返回的数据中是否已包含封面URL。
        2. 如果缺失，脚本会自动根据歌曲ID，在后台打开该歌曲的详情页。
        3. 通过CSS选择器 `.u-cover img.j-img` 精准定位到主封面元素。
        4. 获取封面URL后，会自动将其参数修改为 `?param=500y500`，从而确保您下载到的是 **500x500像素** 的高清封面图。

### 3. 通用核心改进：为何必须登录？

`v10.3.7` 版本将登录设为必要条件，这并非随意限制，而是为了保证功能的可用性和稳定性。

- **技术原因**: 登录后，脚本在发起所有网络请求时，都能带上您的用户凭证（`Cookie`）。这就好比您需要用自己的钥匙才能打开家门，脚本也需要您的“钥匙”才能被平台服务器识别为真实用户，从而：
    1. **获得访问权限**: 能够访问需要登录才能查看的数据接口（特别是咪咕的完整歌手歌曲列表）。
    2. **规避反爬虫机制**: 大大降低因频繁请求而被平台防火墙拦截的风险。
    3. **获取完整信息**: 确保能看到VIP或付费歌曲的正确标识。

## 未来功能规划 (待实现)

您提供的关于从咪咕歌曲详情页获取封面和歌词的想法非常有价值，这将是未来版本更新的重点。

- **目标**: 在成功下载咪咕歌曲的音频后，自动为其补全封面和歌词文件。
- **技术思路 (基于您的HTML分析)**:
    1. **封面**: 访问歌曲详情页，定位到封面图片元素（如 `div.song-info-pic > img`），提取并下载。
    2. **歌词**: 访问歌曲详情页，通过 `div.lyricBlock[data-v-c21b6439]` 和 `span.lyric` 等选择器，抓取页面上渲染出的所有歌词文本行，并组合成 `.lrc` 文件。

> **当前状态**: 请注意，以上针对咪咕封面和歌词的功能 **尚未在本版本中实现**，敬请期待后续更新。

## 使用指南

1. **启动脚本**: 在终端中运行 `python 你的脚本文件名.py`，浏览器会自动打开操作界面。
    
2. **第一步：初始化**:
    
    - 在 **“系统设置”** 标签页，点击 **“初始化v10.3.7系统”**。一个用于自动化操作的浏览器窗口将会弹出，请勿关闭它。
3. **第二步：登录账号 (必须完成，否则无法使用)**:
    
    - 分别点击 **“打开咪咕/网易云登录页”** 按钮。
    - 在弹出的浏览器窗口中 **手动完成登录**（推荐使用App扫码）。
    - 登录后，回到工具界面，点击对应的 **“检查登录状态”** 按钮，直到状态框显示“✅...登录成功”。
4. **第三步：搜索歌曲 (请仔细阅读)**:
    
    - 切换到 **“v10.3.7搜索下载”** 标签页。
    - **⚠️ 重要提示：请严格按以下规则输入关键词**
        - **搜索咪咕音乐**: **必须输入完整、正确的歌手名。** 例如：`周杰伦`。
        - **搜索网易云音乐**: 可以输入 `歌手名` 或 `歌手名 歌曲名`。例如：`周杰伦 青花瓷`。
    - 设置好关键词后，点击 **“开始v10.3.7搜索”**。
5. **第四步：浏览与下载**:
    
    - 在下方的表格中浏览结果，可以使用分页控件查看全部歌曲。
    - **单曲下载**: 输入歌曲序号，点击“下载单曲”。
    - **批量下载**: 在搜索前勾选“搜索后自动下载”。

## 常见问题 (FAQ)

- **为什么咪咕音乐搜索没有结果？** 答：请检查您输入的 **歌手名是否100%准确**。此版本对咪咕的搜索是严格匹配，不支持模糊查询或错别字。例如，搜索“周杰伦”会有结果，但搜索“周董”则会失败。
    
- **为什么一定要登录？不登录不行吗？** 答：**不行。** 目前，无论是咪咕还是网易云，未登录状态下能获取到的数据都非常有限且不稳定。登录是保证本版本所有功能正常运行的 **硬性前提**。
