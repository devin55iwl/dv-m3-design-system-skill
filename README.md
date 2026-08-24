# DV M3 Design System Skill

[中文](#中文) | [English](#english)

## 中文

一个让 AI 按 Material Design 3 规则设计、重建、审查和实现 UI 的可复用 Skill。

它不只是让 AI “看懂设计稿”，还让 AI 知道：该用什么 token、组件和交互；什么时候保留本地设计；如何验证全站页面真的完成迁移。

### 这个 Skill 是怎么搭建的

1. 参考 Google 的公开工程代码规范，让 AI 写出更清晰、可维护的实现。
2. 阅读 Material Design 3 网站，提取颜色、字体、组件、状态、布局和无障碍规则。
3. 阅读 Material 3 Figma Library，建立 token、组件变体和 Figma-to-code 的对应关系。
4. 将这些内容整理成 AI 可读的规则、执行步骤和 QA 清单。
5. 封装成 Skill，让不同 agent 都能用同一套方法工作。

### 它能做什么

- 新建符合 Material 3 的界面，或将 Figma Material 3 设计转换为代码。
- 重建已有页面，或迁移整套产品 UI。
- 补齐 loading、error、disabled、selected 等组件状态。
- 检查无障碍、响应式断点、页面溢出和共享组件复用。
- 在需要登录的页面中，只使用授权的测试登录方式完成验证。

### 它不是什么

- 不是 React、Flutter 或 Android 的组件库；实际组件来自目标项目。
- 不会绕过登录，也不会索要生产账号、真实用户账号或密钥。
- 不会覆盖产品的业务规则、安全要求和明确的本地工程约定。

### 安装到 Codex

```bash
git clone https://github.com/devin55iwl/dv-m3-design-system-skill.git \
  ~/.codex/skills/dv-m3-design-system
```

安装后，在新的 Codex 对话回合中即可使用 `dv-m3-design-system`。

### 如何触发

```text
Use DV M3 Design System to rebuild the settings page.
```

重建、重构或改造已有页面时，Skill 会先要求用户输入：

```text
Mode: preserve-local | material3-overwrite
Scope: current-page | all-pages
```

### 两种重建模式

| 模式 | 适用场景 | AI 的行为 |
|---|---|---|
| `preserve-local` | 真实项目维护 | 保留品牌、组件库和已有交互；用 Material 3 修正不一致、补状态、改善无障碍和自适应。 |
| `material3-overwrite` | 用 M3 彻底重做视觉 | 以 Material 3 tokens、组件、间距、圆角、阴影和状态替换本地样式；保留业务逻辑、信息层级和用户流程。 |

### 两种覆盖范围

| 范围 | AI 的行为 |
|---|---|
| `current-page` | 只处理指定页面、路由、屏幕或 Figma Frame，并说明没有做全站审计。 |
| `all-pages` | 先盘点相关路由、布局、共享组件和 token；先修共享基础，再迁移每一个相关页面。 |

当范围是 `all-pages` 时，只有所有相关页面都已迁移或明确排除，且没有 `blocked` 页面时，才能报告 `completed`；否则必须报告 `partial` 并列出原因。

### 全站迁移与 QA

1. 发现路由、页面、布局、共享组件、主题 token、UI stories 和测试资源。
2. 建立版本可追踪的迁移审计表，先修共享 tokens 和组件，再迁移页面。
3. 对登录保护页面，优先复用已有测试账号、mock、seed、浏览器 session 或 E2E 登录方案。
4. 启动应用，用浏览器自动化验证各路由，并记录状态、截图和 overflow 结果。

Web 响应式页面必须验证：

```text
599, 600, 839, 840, 1199, 1200, 1599, 1600
```

还会检查页面级横向溢出、内容裁切、控件重叠、键盘焦点、屏幕阅读器标签、对比度、文字缩放和减少动画设置。

### 来源与优先级

- [Material Design 3](https://m3.material.io/)：视觉 tokens、组件、状态、交互、自适应布局和无障碍。
- [Material 3 Figma Design Kit](https://www.figma.com/design/rlipYt5ccndwzYHQLe53PZ/Material-3-Design-Kit--Community-?node-id=11-1833)：变量、样式、组件变体与 Figma-to-code 映射。
- [google/styleguide](https://github.com/google/styleguide)：公开的、语言相关的工程规范参考。

冲突时的优先级：产品和安全要求 > 本地项目约定 > 所选重建模式 > Material 3 > Google 的公开工程参考。

### 仓库结构

```text
SKILL.md                                  AI 执行流程
agents/openai.yaml                        Codex 展示信息
references/design-system.md               完整 M3 设计系统与 QA 参考
references/google-styleguide-reference.md Google 工程规范来源映射
```

## English

A reusable skill that helps AI agents design, rebuild, review, and implement UI
using Material Design 3 rules.

It does more than help an agent read a design. It defines which tokens,
components, and interactions to use, when to preserve a local design system,
and how to verify that an app-wide migration is actually complete.

### How This Skill Was Built

1. Use Google's public engineering style guides as a reference for readable and
   maintainable implementation.
2. Read the Material Design 3 site for color, typography, components, states,
   layout, and accessibility guidance.
3. Read the Material 3 Figma Library to map tokens, component variants, and
   Figma concepts to code.
4. Convert these inputs into AI-readable rules, workflows, and QA checklists.
5. Package the result as a Skill that multiple agents can reuse consistently.

### What It Can Do

- Create Material 3 UI or translate a Material 3 Figma design into code.
- Rebuild an existing page or migrate a product's UI across routes.
- Cover component states such as loading, error, disabled, and selected.
- Check accessibility, responsive breakpoints, overflow, and shared-component
  reuse.
- Verify protected routes only through authorized test authentication.

### What It Is Not

- It is not a React, Flutter, or Android component library; the target project
  supplies the actual implementation library.
- It does not bypass sign-in or request production credentials, real-user
  accounts, or secrets.
- It does not override business requirements, security requirements, or
  explicit local engineering conventions.

### Install In Codex

```bash
git clone https://github.com/devin55iwl/dv-m3-design-system-skill.git \
  ~/.codex/skills/dv-m3-design-system
```

Start a new Codex turn, then use `dv-m3-design-system`.

### Trigger It

```text
Use DV M3 Design System to rebuild the settings page.
```

For an existing-page rebuild, refactor, or visual transformation, the skill
asks for:

```text
Mode: preserve-local | material3-overwrite
Scope: current-page | all-pages
```

### Rebuild Modes

| Mode | Use When | Agent Behavior |
|---|---|---|
| `preserve-local` | Maintaining a real product | Preserve the brand, component library, and interaction patterns; use Material 3 to repair inconsistency, missing states, accessibility, and responsive behavior. |
| `material3-overwrite` | Replacing the visual system with M3 | Replace local tokens, components, spacing, shape, elevation, and states while preserving business logic, content hierarchy, and user flow. |

### Coverage Scopes

| Scope | Agent Behavior |
|---|---|
| `current-page` | Change only the named page, route, screen, or Figma frame and state that no app-wide audit was performed. |
| `all-pages` | Inventory relevant routes, layouts, shared components, and tokens; repair shared foundations first, then migrate every affected page. |

For `all-pages`, the task is `completed` only when every relevant route is
migrated or explicitly excluded and none is `blocked`. Otherwise the task is
reported as `partial` with its blockers.

### App-Wide Migration And QA

1. Discover routes, pages, layouts, shared components, theme tokens, UI stories,
   and test assets.
2. Create a version-controlled migration audit; repair shared tokens and
   components before migrating pages.
3. For protected routes, reuse an existing authorized test account, mock, seed,
   browser session, or E2E authentication flow where available.
4. Run the app, use browser automation to validate routes, and record status,
   screenshots, and overflow results.

Responsive web pages must be checked at:

```text
599, 600, 839, 840, 1199, 1200, 1599, 1600
```

QA also covers page-level horizontal overflow, clipping, overlapping controls,
keyboard focus, screen-reader labels, contrast, text scaling, and reduced
motion.

### Sources And Precedence

- [Material Design 3](https://m3.material.io/): visual tokens, components,
  states, interaction, adaptive layout, and accessibility.
- [Material 3 Figma Design Kit](https://www.figma.com/design/rlipYt5ccndwzYHQLe53PZ/Material-3-Design-Kit--Community-?node-id=11-1833): variables, styles, variants, and Figma-to-code mapping.
- [google/styleguide](https://github.com/google/styleguide): public,
  language-specific engineering guidance.

When sources conflict, use this order: product and security requirements >
local project conventions > selected rebuild mode > Material 3 > public Google
engineering guidance.

### Repository Structure

```text
SKILL.md                                  Agent workflow
agents/openai.yaml                        Codex display metadata
references/design-system.md               Full M3 system and QA reference
references/google-styleguide-reference.md Engineering-guide source map
```
