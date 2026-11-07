# 第一次重建演练 | First rebuild drill: study a site you own, then rebuild it

拿一个你有权查看的目标把四步走通，顺序别乱（先 Clean room、先小目标，验完再放大）：

1. **装与体检**：从 Releases 装对应平台的安装包（仍为 alpha），或源码路线 `cargo install --path apps/craft-anything-cli`；跑 `craft-anything-cli doctor`，它会告诉你编程助手与引擎缺什么 —— 先补齐再往下走。
2. **确认助手就位**：构建要靠本机的编程助手。首选 Claude Code：装好、登录好你的 `claude`（Craft Anything 跑的是你自己那份、不会经手你的账号）。doctor 说缺的就是这里缺。
3. **选对目标**：第一次只选你自己的网站（或 example.com 这类公开演示站），保持默认 Clean room —— 只看访客能看到的表面，先别碰 Deep。
4. **跑一次完整重建**：`craft-anything-cli rebuild https://example.com "Recreate this website"`，或在桌面应用 Simple 模式里选 Website、填地址、描述、按 Start。过程中它会用提问卡与批准卡找你拍板 —— 每个批准都先看清命令再点。
5. **看对照结果**：到 Check 步看三宽度对照清单：哪些页面对上了、相似度多少、还差在哪。挑一条差得最多的提修改要求，再走一轮 Refine，亲眼看它收敛。
6. **收尾自查**：打开 `checks/` 与 `investigation.cany/` 看证据记录 —— 每条结论的置信度与局限写在那里。记住两条红线：只研究你拥有或有权检查的软件；重建物里的 logo、照片、文案不是你的就别拿去用。
