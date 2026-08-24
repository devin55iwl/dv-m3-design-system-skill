# DV M3 Design System Skill

一个让 AI 按 Material Design 3 规则设计、重建、审查和实现 UI 的可复用 Skill。

它不是只让 AI “看懂设计稿”的文档，而是让 AI 知道：该用什么 token、组件和交互；什么时候该保留本地设计；如何验证全站页面真的完成了迁移。

## 这个 Skill 是怎么搭建的

1. 参考 Google 的公开工程代码规范，让 AI 写出更清晰、可维护的实现。
2. 阅读 Material Design 3 网站，提取颜色、字体、组件、状态、布局和无障碍规则。
3. 阅读 Material 3 Figma Library，建立 token、组件变体和 Figma-to-code 的对应关系。
4. 将这些内容整理成 AI 可读的规则、执行步骤和 QA 清单。
5. 封装成 Skill，让不同 agent 都能用同一套方法工作。

## 它能做什么

- 新建符合 Material 3 的界面。
- 将 Figma Material 3 设计转换为代码实现。
- 重建已有页面，或迁移整套产品 UI。
- 补齐 loading、error、disabled、selected 等组件状态。
- 检查无障碍、响应式断点、页面溢出和共享组件复用。
- 在需要登录的页面中，只使用授权的测试登录方式完成验证。

## 它不是什么

- 不是 React、Flutter 或 Android 的组件库；它指导 AI 如何选择和实现组件，实际组件来自目标项目。
- 不会绕过登录，也不会索要生产账号、真实用户账号或密钥。
- 不会覆盖产品的业务规则、安全要求和明确的本地工程约定。

## 安装到 Codex

```bash
git clone https://github.com/devin55iwl/dv-m3-design-system-skill.git \
  ~/.codex/skills/dv-m3-design-system
```

安装后，在新的 Codex 对话回合中即可使用 `dv-m3-design-system`。

## 如何触发

直接用自然语言说明，例如：

```text
Use DV M3 Design System to rebuild the settings page.
```

如果任务是重建、重构或改造已有页面，Skill 会先要求用户输入两项选择：

```text
Mode: preserve-local | material3-overwrite
Scope: current-page | all-pages
```

完整示例：

```text
Use DV M3 Design System.
Mode: material3-overwrite
Scope: all-pages
用 Material 3 重建这个产品，但保留现有业务流程。
```

## 两种重建模式

| 模式 | 适用场景 | AI 的行为 |
|---|---|---|
| `preserve-local` | 真实项目维护 | 保留品牌、组件库和已有交互；用 Material 3 修正不一致、补状态、改善无障碍和自适应。 |
| `material3-overwrite` | 用 M3 彻底重做视觉 | 以 Material 3 tokens、组件、间距、圆角、阴影和状态替换本地样式；保留业务逻辑、信息层级和用户流程。 |

## 两种覆盖范围

| 范围 | AI 的行为 |
|---|---|
| `current-page` | 只处理指定页面、路由、屏幕或 Figma Frame，并明确说明没有做全站审计。 |
| `all-pages` | 先盘点全部相关路由、布局、共享组件和 token；先修共享基础，再迁移每一个相关页面。 |

当范围是 `all-pages` 时，Skill 会建立迁移审计表。只有所有相关页面都已迁移或明确排除，且没有 `blocked` 页面时，才能报告 `completed`；否则必须报告 `partial` 并列出原因。

## 全站迁移时的执行流程

1. 发现路由、页面、布局、共享组件、主题 token、UI stories 和测试资源。
2. 建立版本可追踪的迁移审计表。
3. 先修 tokens、布局、导航、输入控件和反馈组件等共享基础。
4. 按 UI 家族迁移代表页面，再把共享能力复用到其他页面。
5. 对登录保护页面，优先复用已有测试账号、mock、seed、浏览器 session 或 E2E 登录方案；没有时才向用户索取授权测试环境的登录方式。
6. 启动应用并通过浏览器自动化验证各路由。
7. 记录迁移状态、截图和 overflow 结果，供之后的 agent 继续工作。

## 响应式与无障碍验收

对 Web 响应式页面，必须验证下列 CSS 像素宽度：

```text
599, 600, 839, 840, 1199, 1200, 1599, 1600
```

还会检查：

- 是否出现页面级横向溢出、内容裁切或控件重叠。
- sticky 导航、锚点、表格、导航栏、抽屉和底部导航在不同断点是否可用。
- 键盘焦点、屏幕阅读器标签、对比度、文字缩放、点击区域和减少动画设置是否合理。

## 来源与优先级

- [Material Design 3](https://m3.material.io/)：视觉 tokens、组件、状态、交互、自适应布局和无障碍。
- [Material 3 Figma Design Kit](https://www.figma.com/design/rlipYt5ccndwzYHQLe53PZ/Material-3-Design-Kit--Community-?node-id=11-1833)：变量、样式、组件变体与 Figma-to-code 映射。
- [google/styleguide](https://github.com/google/styleguide)：公开的、语言相关的工程规范参考。

来源冲突时，优先级为：产品和安全要求 > 本地项目约定 > 所选重建模式 > Material 3 > Google 的公开工程参考。

## 仓库结构

```text
SKILL.md                                  AI 执行流程
agents/openai.yaml                        Codex 展示信息
references/design-system.md               完整 M3 设计系统与 QA 参考
references/google-styleguide-reference.md Google 工程规范来源映射
```

## 发布记录

本仓库于 `2026-08-24` 新建并推送，用于保留这一版 DV M3 Design System Skill 的发布记录。
