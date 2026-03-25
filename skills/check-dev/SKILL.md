---
name: check-dev
description: "检查刚写的代码是否符合相关开发指南。"
---

# 开发后检查

实现完成后，检查改动是否仍符合相关指南。

执行以下步骤：
1. 先做通用复核：
   - [ ] 已运行 `git status` 与 `git diff --name-only`，明确改动范围
   - [ ] 当前任务的 `notes.md` 已更新，且改动范围与任务记录一致
   - [ ] 验证结论已写明依据，而不只是写“已验证”
   - [ ] 本次出现的问题、例外或阻塞已记录
   - [ ] 若本次暴露出 workflow、skill、spec 或模板缺口，已在 `notes.md` 中标记
2. 判断适用哪些检查：
   - 前端改动 → 前端指南检查
   - 后端改动 → 后端指南检查
   - 跨层或影响面大的改动 → 跨层检查
3. 若有前端改动，阅读：
   - `.trellis/spec/frontend/index.md`
   - `.trellis/spec/frontend/component-guidelines.md`
   - `.trellis/spec/frontend/hook-guidelines.md`
   - `.trellis/spec/frontend/state-management.md`
   - `.trellis/spec/frontend/type-safety.md`
   - `.trellis/spec/frontend/quality-guidelines.md`
4. 若有后端改动，阅读：
   - `.trellis/spec/backend/index.md`
   - `.trellis/spec/backend/database-guidelines.md`
   - `.trellis/spec/backend/error-handling.md`
   - `.trellis/spec/backend/logging-guidelines.md`
   - `.trellis/spec/backend/quality-guidelines.md`
5. 若跨层或高影响改动，另做下列核对：
   - 阅读 `.trellis/spec/guides/cross-layer-thinking-guide.md`
   - 阅读 `.trellis/spec/guides/code-reuse-thinking-guide.md`
   - 核对所触及各层的数据流
   - 搜索重复的常量、工具函数及遗漏的并列更新
   - 核对 import 路径与同域一致性
6. 对照所选指南审阅代码
7. 报告违规项，若发现则修复

## 跨层检查清单

当改动跨多层或可能影响共享行为时使用本节。

### 维度 A：跨层数据流

**触发条件**：改动涉及 3 层或以上

- [ ] 读路径：数据库 → 服务 → API → UI
- [ ] 写路径：UI → API → 服务 → 数据库
- [ ] 类型或 schema 在各层之间正确传递
- [ ] 错误正确传播给调用方
- [ ] 各层正确处理加载或 pending 状态

### 维度 B：代码复用

**触发条件**：
- 修改常量、标签、图标、颜色或共享配置
- 新建工具或辅助函数
- 在多个相似文件上批量修改

- [ ] 先搜索：同一值在多少处定义
- [ ] 若有 2 处以上定义同一值，判断是否抽出共享常量
- [ ] 修改后所有使用处均已更新
- [ ] 若新建工具函数，确认是否已存在类似实现

### 维度 C：import 与依赖路径

**触发条件**：新建源文件

- [ ] import 路径符合项目约定
- [ ] 无明显循环依赖
- [ ] 模块放置位置符合项目组织方式

### 维度 D：同层一致性

**触发条件**：
- 修改展示逻辑或格式化
- 同一领域概念出现在多处

- [ ] 搜索该概念的其他用法
- [ ] 保持这些用法一致
- [ ] 判断是否需要共享配置或常量
