---
name: Superpowers
slug: superpowers
version: "1.0.0"
description: "AI编码技能框架。强制需求讨论→设计方案→TDD→代码审查的专业开发流程。任何编码任务前必须先讨论设计，没有失败测试不写代码。"
metadata: {"emoji":"⚡"}
---

# Superpowers — AI 编码技能框架

> 改编自 [obra/superpowers](https://github.com/obra/superpowers)，适配 Trae CN 前端开发场景。

## 铁律

1. **没有设计方案，不写代码**
2. **没有失败测试，不写业务代码**
3. **没有根因调查，不提修复方案**

## 工作流程

```
需求讨论 → 设计方案 → 编写计划 → TDD实现 → 代码审查 → 提交
```

---

## 一、需求讨论（Brainstorming）

在写任何代码之前，必须先理解需求。

### 流程

1. **了解项目现状** — 检查项目文件结构、技术栈、已有组件
2. **逐个提问** — 一次只问一个问题
3. **提出 2-3 种方案** — 列出优劣和推荐
4. **分段呈现设计** — 每段 200-300 字，等用户确认
5. **保存设计文档** — 写入 `docs/specs/YYYY-MM-DD-<主题>-design.md`
6. **用户确认后** — 进入编写计划

### ⛔ 硬性门控

设计确认前禁止写代码、创建文件、执行实现。无论多简单的任务。

### 关键原则

- **YAGNI** — 削减不必要的功能
- **一次一个问题** — 不要多个问题轰炸
- **小而美的设计** — 组件职责单一，接口清晰，可独立测试

---

## 二、编写计划（Writing Plans）

设计方案确认后，拆解为极小任务。

### 任务粒度（每个步骤 2-5 分钟）

```
Step 1: 写失败测试
Step 2: 运行确认失败
Step 3: 写最小实现
Step 4: 运行确认通过
Step 5: 提交 commit
```

### 每个任务必须包含

- 精确文件路径（创建/修改/测试）
- 完整代码（禁止 TBD、TODO、"参考 Task N"）
- 运行命令和预期输出
- 规范的 commit message

### 保存位置

`docs/plans/YYYY-MM-DD-<功能名>.md`

---

## 三、测试驱动开发（TDD）

### 铁律

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

### Red-Green-Refactor

1. **RED** — 写一个失败测试，确认它失败
2. **GREEN** — 写最小代码通过测试，确认通过
3. **REFACTOR** — 清理代码，保持测试通过

### 前端测试工具推荐

| 场景 | 工具 |
|------|------|
| 组件单元测试 | Vitest + React Testing Library |
| API Mock | MSW |
| E2E | Playwright |

### ⛔ 借口表（全部无效）

| 借口 | 现实 |
|------|------|
| "太简单不用测" | 写测试 30 秒 |
| "先写代码后补测试" | 直接通过的测试等于没测 |
| "已手动测过" | 手动测试不可复现 |

### 前端 TDD 示例

```typescript
// RED: 写失败测试
test('空邮箱提交显示错误', async () => {
  render(<LoginForm onSubmit={vi.fn()} />)
  await userEvent.click(screen.getByRole('button', { name: /提交/i }))
  expect(screen.getByText('请输入邮箱')).toBeInTheDocument()
})
// 验证 RED → FAIL

// GREEN: 最小实现
// 验证 GREEN → PASS

// REFACTOR: 清理
```

---

## 四、系统化调试（Systematic Debugging）

### 铁律

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

### 四阶段

1. **根因调查** — 读错误、复现、检查变更、追踪数据流
2. **模式分析** — 找正常工作的代码对比差异
3. **假设验证** — 明确陈述假设，最小改动验证
4. **实现修复** — 先写失败测试复现 bug，修复根因

### ⛔ 3 次修复失败 → 停下来讨论架构

---

## 五、代码审查（Code Review）

### 必须审查

- 每个任务完成后
- 主要功能开发完
- 合并前

### 审查要点

- 正确性：符合设计文档？
- 测试：覆盖核心逻辑和边界？
- YAGNI：有过度设计？
- DRY：有重复代码？
- 前端特有：内存泄漏？useEffect 清理？状态管理？可访问性？性能？

---

## 六、提交前检查清单

- [ ] 所有测试通过
- [ ] 无 TypeScript 错误
- [ ] 无 ESLint 警告
- [ ] 无 console.log 遗留
- [ ] 审查问题已修复
- [ ] Commit message 规范（feat/fix/refactor/test/docs）

---

## 七、前端开发专属规则

### 组件设计

- 文件不超过 200 行，超过则拆分
- Props 用 TypeScript interface，避免 any
- 用 composition 而非 deep nesting

### 状态管理

- 组件内 → useState/useReducer
- 跨组件 → Context 或状态库
- 服务端 → SWR/React Query

### 性能

- React.memo/useMemo/useCallback 要有 profiling 依据
- 大列表虚拟滚动
- 路由懒加载

### 样式

- 优先 Tailwind CSS 或 CSS Modules
- mobile-first 响应式
- CSS 变量管理 design token

### 错误处理

- Error Boundary 包裹关键组件
- 统一 API 错误处理
- 优雅降级

---

*Based on Superpowers v4.1.1 by Jesse Vincent · MIT License*
