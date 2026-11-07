# Craft Anything 速查表 | Craft Anything Cheatsheet | Guía rápida de Craft Anything

**先看懂任何软件怎么运作，再让 AI 照着做你自己的版本 —— 但只对你有权查看的软件用**

> 一页看懂 Craft Anything：一个纯 Rust 写的开源「洁净室」逆向与重建引擎。把一个网站、桌面应用、手机应用、游戏或文件指给它，它先用真正的检查工具把原作研究透（每条结论都带证据），再让编程助手按你的要求从零建一个新项目，然后拿成品逐项和原作对照、列出还差在哪、继续修到接近为止。320 个工具同时是 CLI 命令与 MCP 工具 —— 桌面应用能做的，任何智能体也能驱动。

- 上游项目：[storytold/craft-anything](https://github.com/storytold/craft-anything)（2026-11-07 实测 19,736 ★ / 3,746 forks，MIT OR Apache-2.0 双许可；最新版 v0.5.0，2026-11-06 发布；状态：early alpha）
- 平台：macOS / Windows / Linux / FreeBSD 桌面应用＋CLI＋MCP 服务器，部分引擎可跑在浏览器 WebAssembly；构建重建功能需要本机装有并已登录的编程助手（Claude Code 首选支持）
- 本页结构：中文主文 → English → Español → Português

---

Apoyo y más: [GitHub Sponsors](https://github.com/sponsors/Ezra-Zhao) · [Gumroad — plantillas y cuadernos que uso](https://ezraflow28.gumroad.com) · [Ko-fi](https://ko-fi.com/ezra777zhao)

## 它是什么、不是什么

| 是 | 不是 |
|---|---|
| 一个纯 Rust 的逆向检查引擎（320 个工具）＋「研究 → 重建 → 对照」工作流：看懂原作、让助手重建、拿证据验收 | 不是一键克隆机：它重建的是布局、风格与行为，助手被明确要求不复制原作的 logo、照片与文案（除非你确认有权） |
| 两种观察深度：Clean room（只看表面）与 Deep（读代码与文件），规则写死在工具层，越界的工具会被拒绝并告诉你怎么开 Deep | Clean room 不是操作系统沙箱：原仓明说它不隔离你的电脑，你跑的编程助手仍以你的权限行事 |
| 证据驱动：每个结论是带置信度、覆盖范围与局限的证据记录，大结果先给摘要、再分页细看 | 不是破解工具：不绕 DRM、登录与反作弊；只应用于你拥有或有权检查的软件（原仓负责任使用条款） |
| 智能体就绪：每个工具同时是 CLI 子命令和 MCP 工具，检查环节本身不需要 AI | 状态是 early alpha：不少能力「部分完成」，别拿它当生产流水线（见诚实标注区） |

## 四步工作流卡

1. **Study（研究）**：检查原作 —— 网站看各宽度下的页面，程序看它的代码、文件与实际运行；每个答案背后都有证据。
2. **Build（重建）**：编程助手按你点名的语言/框架/引擎（没点名用合理默认）从零建新项目。
3. **Check（对照）**：成品与原作逐项比对（网站在三种屏幕宽度下比、游戏跑它自带的自测、应用看实际运行），列出还没对上的地方。
4. **Ready（交付）**：打开、试玩或预览成品；想改就提要求，它继续修。

两种用法走同一引擎：**Simple**（默认）只问「你想做什么」，选网站/应用/游戏、描述、按 Start，项目页用大白话显示四步进度、提问卡与批准卡、相似度；「Just look inside」只看不建（Deep 模式下可用）。**Advanced** 是全套工作台：全部工具（Ctrl/Cmd+K）、函数、字符串、伪 C、反汇编、十六进制、调用图、证据与未知项、任务与日志、智能体控制台。

## Clean room 与 Deep 对照表（先分清再动手）

| | Clean room（默认） | Deep |
|---|---|---|
| 看什么 | 只看表面：你加的图片、网站以访客身份看到的样子（截图、可见文字、实测布局、颜色、字体、间距、展开的菜单），经允许可看应用窗口截图 | 全看：程序的代码与文件用内置引擎读（装了 Ghidra 时也用 Ghidra），网站读它的 JavaScript 并观察起始页 |
| 工具行为 | 会「往里看」的工具被拒绝，并提示如何开 Deep | 全部工具可用 |
| 定位 | 公开重建、学习与竞品表面研究 | 仅供你私人用途（你自己的或有权深查的软件） |
| 切换 | 标题栏或 Settings；`settings_set deep_mode true`；单次 CLI 运行加 `--deep`（或 `--clean-room`） | 同左 |

## 安装与上手卡

**省事路线**：Releases 页有 Windows / macOS / Linux / FreeBSD 的签名安装包（仍为 alpha），macOS 版已签名并公证；Linux 有 AppImage / Flatpak / deb / rpm / tarball（注意：Flatpak 是沙箱版，起不了本机浏览器，研究网站别用它）。

**源码路线**：需要 Rust **1.97 或更新**（有依赖要求它，先 `rustup update`）；Debian/Ubuntu 跑桌面应用还要先装系统库：

```bash
sudo apt-get install libxkbcommon-dev libwayland-dev libx11-dev libxrandr-dev libxi-dev libgl1-mesa-dev libgtk-3-dev
git clone https://github.com/storytold/craft-anything && cd craft-anything
cargo run --release -p craft-anything            # 桌面应用
cargo install --path apps/craft-anything-cli     # CLI，装好叫 craft-anything-cli
craft-anything-cli doctor                        # 检查安装、编程助手与引擎齐不齐
```

**命令行重建**（问题会在终端里问你）：

```bash
craft-anything-cli rebuild https://example.com "Recreate this website"
craft-anything-cli rebuild - "A snake game in Rust" --rounds 1
craft-anything-cli rebuild https://example.com "Make the header blue" --continue-in "<project folder>"
craft-anything-cli commands    # 全部工具，按工具集列出
craft-anything-cli mcp         # 把工具以 MCP 方式供任何 MCP 客户端调用
craft-anything-cli setup       # 把 Craft Anything 接到编程助手应用里
```

- 新项目默认落在 `Documents/Craft Anything`（Settings → General 可改）；每个项目文件夹含 `craft.json`（请求、目标、状态与历史）、`capture/`（从目标录到的东西）、`app/`（生成的项目）、`checks/`（每次与原作的对照）、`investigation.cany/`（证据）、`inbox/`（待答问题）。
- 编程助手现状：**Claude Code 首选支持**（跑你自己的、未改动的 `claude`，用你自己的登录，Craft Anything 不经手你的账号）；Codex 适配器写了但**还没在真机上验证过**；GitHub Copilot 走 ACP 用你装的 Copilot CLI（另行 `copilot login`）；Grok Build 走 ACP（`grok agent stdio`，另行 `grok login`）；其他 ACP 助手（Gemini CLI、Qwen Code、opencode、goose、Hermes、Maki）可切换，其中只有 Maki 真跑通过。问答与分析可用 Anthropic / OpenAI / OpenRouter / xAI 或本地 OpenAI 兼容服务（Settings → AI）。

## 引擎能查什么（320 工具的面）

自带解码器、反汇编器与反编译器，不依赖外部程序；覆盖：原生程序、.NET、Java、Android、Apple 平台、Electron/JavaScript、网站、固件、抓包、数据库与游戏文件；Deep 模式全开，并可调用已安装的 Ghidra。游戏重建可走 Unity、Unreal 或 Godot；生成的图片与 3D 模型是可选功能，用你自己的 fal.ai key。

## 诚实标注区（这一栏是这张速查的命）

- **它自己说：early alpha**。原仓状态页写得明白 —— 今天能用的：桌面应用（Simple 与 Advanced）、CLI、MCP 服务器、以 Claude Code 为构建者的四步流、网站三宽度对照、Unity/Unreal/Godot 游戏重建、Deep＋Ghidra；部分完成的：用 API key 与其他编程助手构建（写好了、没真跑过）、应用与游戏的行为对照、少数只拿替身测过的引擎工具。按这个口径期待它，别当成熟产品。
- **装依赖就是跑代码**（原仓安全提示）：构建时装项目依赖，命令在你的电脑上执行，包可以在安装期跑代码（npm 默认跑各包的 install scripts，Rust 构建脚本与 Python 源码包同理）。Craft Anything 遇到本项目没见过的命令种类会先问你；对 `npm` 选了「总是允许」就等于以后都不问 —— 只对依赖可信的项目这么干；想禁掉安装脚本用 `npm install --ignore-scripts`。
- **Clean room 的边界要懂**：它约束的是 Craft Anything 自己提供与拒绝什么，不是系统级隔离；编程助手本人的工具权限它管不全（各助手不同，Claude Code 的细则在原仓 docs/claude-code.md）。
- **版权与授权红线**：只研究你拥有或有权检查的软件；重建复制布局、风格与行为，助手被要求不复制 logo、照片与文案，除非你确认拥有或获许可。拿它「克隆」别人的商业软件去卖，不在这个工具的负责任使用范围内，也大概率踩法律线 —— 本页不提供这类用法指导。
- **多语文档现状**：原仓自带含简体中文在内的 9 种语言 README —— 这点和很多项目不同，本页的价值在速查与避坑，不在补语言空缺。
- **势头数据**：19,736 ★ / 3,746 forks（2026-11-07 原仓实测，证据 A），fork 率约 19%，是真有人拿回去用的信号；最新版 v0.5.0（2026-11-06），迭代很快，本页细节以同日上游 README 为准。

---

## English (summary)

Craft Anything (19,736 stars / 3,746 forks as of 2026-11-07, MIT OR Apache-2.0, latest v0.5.0, early alpha) is an open-source, clean-room reverse-engineering and rebuilding engine written in pure Rust. Point it at a website, desktop app, phone app, game or file: it studies the original with real inspection tools (every answer backed by an evidence record with confidence, coverage and limitations), has a coding agent build a new project in the stack you name, compares the result against the original (web pages at three screen widths, a game's own self-test, an app's runs), and refines until it is close. Its 320 tools are each both a CLI subcommand and an MCP tool; no AI is needed just to inspect.

Two observation depths: **Clean room** (default) reads nothing inside the original — only what a visitor can see: screenshots, visible text, measured layout, colours, fonts, spacing — and tools that would look inside are refused. **Deep** mode reads code and files with the built-in engines (plus Ghidra when installed) and is for your private use on software you own or are allowed to examine. Note that clean room is a rule about what the tool offers and refuses, not an OS sandbox. Two ways to use it: Simple (pick a target, describe what you want, press Start) and the Advanced workbench (functions, strings, pseudo-C, disassembly, hex, call graphs, evidence).

Get started from the Releases page (signed installers for Windows, macOS, Linux, FreeBSD — still alpha) or from source with Rust 1.97+: `cargo run --release -p craft-anything` for the desktop app, `cargo install --path apps/craft-anything-cli` for the CLI, then `craft-anything-cli doctor`. Rebuild from the command line with `craft-anything-cli rebuild <target> "<request>"`. Building needs a signed-in coding agent: Claude Code is supported first (it runs your own unmodified `claude`); the Codex adapter is unverified on a real install; Copilot and Grok Build connect via ACP.

Honest notes: it is early alpha — API-key and other-agent building paths are built but not yet run live, and some checks are partial. Installing a build's dependencies runs code on your machine (npm install scripts, Rust build scripts), so only "always allow" dependency installs for projects you trust. Responsible use: only examine software you own or are permitted to; rebuilds recreate layout, style and behaviour, and the agent is told not to copy logos, photos or text unless you confirm the rights. It never bypasses DRM, logins or anti-cheat.

---

## Español (guía rápida — traducción de estudio, pendiente de revisión por un hablante nativo)

**Entiende cómo funciona cualquier software y haz que la IA construya tu propia versión, solo con software que tengas derecho a examinar.**

Craft Anything (19.736 estrellas / 3.746 forks a 7 de noviembre de 2026, MIT o Apache-2.0, última versión v0.5.0, alfa temprana) es un motor de código abierto de ingeniería inversa «de sala limpia» y reconstrucción, escrito en Rust puro. Apúntalo a un sitio web, una aplicación de escritorio o de teléfono, un juego o un archivo: estudia el original con herramientas de inspección reales (cada respuesta lleva un registro de evidencia con confianza, cobertura y limitaciones), un agente de programación construye un proyecto nuevo en la pila que pidas, el resultado se compara con el original (páginas web en tres anchos de pantalla, la autoprueba del propio juego, ejecuciones de la aplicación) y se refina hasta acercarse. Sus 320 herramientas son, cada una, un subcomando de CLI y una herramienta MCP; para inspeccionar no hace falta IA.

Dos profundidades de observación: **Clean room** (por defecto) no lee nada del interior del original — solo lo que ve un visitante: capturas, texto visible, diseño medido, colores, fuentes y espaciados — y las herramientas que mirarían dentro son rechazadas. El modo **Deep** lee código y archivos con los motores integrados (y Ghidra si está instalado) y es para tu uso privado, con software tuyo o que tengas permiso de examinar. Ojo: clean room es una regla sobre lo que la herramienta ofrece y rechaza, no un sandbox del sistema operativo. Dos formas de usarlo: Simple (elige el objetivo, describe lo que quieres, pulsa Start) y el banco de trabajo Advanced (funciones, cadenas, pseudo-C, desensamblado, hexadecimal, grafos de llamadas, evidencia).

Empieza desde la página de Releases (instaladores firmados para Windows, macOS, Linux y FreeBSD, aún en alfa) o desde el código fuente con Rust 1.97+: `cargo run --release -p craft-anything` para la aplicación de escritorio, `cargo install --path apps/craft-anything-cli` para la CLI y después `craft-anything-cli doctor`. Reconstruye desde la línea de comandos con `craft-anything-cli rebuild <objetivo> "<petición>"`. Para construir necesitas un agente de programación con sesión iniciada: Claude Code es el primero soportado (ejecuta tu propio `claude` sin modificar); el adaptador de Codex aún no se ha verificado en una instalación real; Copilot y Grok Build se conectan vía ACP.

Notas honestas: es alfa temprana — las vías de construcción con claves API y otros agentes están escritas pero aún no probadas en vivo, y algunas comprobaciones son parciales. Instalar las dependencias de un proyecto ejecuta código en tu máquina (scripts de instalación de npm, scripts de compilación de Rust), así que «permitir siempre» solo para proyectos de confianza. Uso responsable: examina solo software tuyo o que tengas permiso de examinar; las reconstrucciones recrean diseño, estilo y comportamiento, y el agente tiene instrucción de no copiar logotipos, fotos ni textos salvo que confirmes los derechos. Nunca elude DRM, inicios de sesión ni sistemas antitrampas.

---

## Português (guia rápido — tradução de estudo, aguardando revisão de um falante nativo)

**Entenda como qualquer software funciona e faça a IA construir a sua própria versão — só com software que você tem o direito de examinar.**

O Craft Anything (19.736 estrelas / 3.746 forks em 7 de novembro de 2026, MIT ou Apache-2.0, versão mais recente v0.5.0, alfa inicial) é um motor de código aberto de engenharia reversa «de sala limpa» e reconstrução, escrito em Rust puro. Aponte-o para um site, um aplicativo de desktop ou de celular, um jogo ou um arquivo: ele estuda o original com ferramentas reais de inspeção (cada resposta vem com um registro de evidência com confiança, cobertura e limitações), um agente de programação constrói um projeto novo na pilha que você pedir, o resultado é comparado com o original (páginas web em três larguras de tela, o autoteste do próprio jogo, execuções do aplicativo) e refinado até ficar próximo. Suas 320 ferramentas são, cada uma, um subcomando de CLI e uma ferramenta MCP; para inspecionar, não é preciso IA.

Duas profundidades de observação: **Clean room** (padrão) não lê nada de dentro do original — só o que um visitante vê: capturas de tela, texto visível, layout medido, cores, fontes e espaçamentos — e as ferramentas que olhariam para dentro são recusadas. O modo **Deep** lê código e arquivos com os motores integrados (e o Ghidra, quando instalado) e é para o seu uso privado, com software seu ou que você tenha permissão de examinar. Atenção: clean room é uma regra sobre o que a ferramenta oferece e recusa, não uma sandbox do sistema operacional. Dois jeitos de usar: o Simple (escolha o alvo, descreva o que quer, aperte Start) e a bancada Advanced (funções, strings, pseudo-C, desmontagem, hexadecimal, grafos de chamadas, evidência).

Comece pela página de Releases (instaladores assinados para Windows, macOS, Linux e FreeBSD, ainda em alfa) ou pelo código-fonte com Rust 1.97+: `cargo run --release -p craft-anything` para o aplicativo de desktop, `cargo install --path apps/craft-anything-cli` para a CLI e depois `craft-anything-cli doctor`. Reconstrua pela linha de comando com `craft-anything-cli rebuild <alvo> "<pedido>"`. Para construir, você precisa de um agente de programação com sessão iniciada: o Claude Code é o primeiro suportado (ele roda o seu próprio `claude`, sem modificações); o adaptador do Codex ainda não foi verificado numa instalação real; Copilot e Grok Build se conectam via ACP.

Notas honestas: é alfa inicial — os caminhos de construção com chaves de API e outros agentes estão escritos, mas ainda não testados ao vivo, e algumas verificações são parciais. Instalar as dependências de um projeto executa código na sua máquina (scripts de instalação do npm, scripts de build do Rust), então «permitir sempre» só para projetos confiáveis. Uso responsável: examine apenas software seu ou que você tenha permissão de examinar; as reconstruções recriam layout, estilo e comportamento, e o agente é instruído a não copiar logotipos, fotos nem textos, salvo se você confirmar os direitos. Ele nunca contorna DRM, logins nem sistemas anti-trapaça.

---

## 来源与声明

- 灵感来源 / Source / Fuente / Fonte: [storytold/craft-anything](https://github.com/storytold/craft-anything)（官网 getartcraft.com；最新版 v0.5.0，2026-11-06；原仓自带含简体中文在内的多语 README）
- 本仓是个人学习整理：内容用我自己的话总结自上游公开文档，不搬运原文；功能、命令与状态口径以 2026-11-07 的上游 README 与其 Status 说明为准，上游迭代后请以官方文档为最终依据。只应用于你拥有或有权检查的软件。
- 西语、葡语部分为学习译稿，待母语校对。Las secciones en español y portugués son traducciones de estudio pendientes de revisión nativa.
- License: MIT © 2026 Guangyi Zhao
