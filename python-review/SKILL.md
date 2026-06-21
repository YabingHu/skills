---
name: python-review
description: "统一的 Python 代码检视入口，根据场景自动选择 Code Review 或 Committer Review。"
---

# Python Review Router

## 目标

统一处理所有 Python 代码检视需求。

根据用户意图、上下文和检视范围，自动选择最合适的检视模式。

原则：

- 默认使用 `python-code-review`
- 涉及团队规范、长期治理时使用 `python-committer-review`
- 必要时同时执行两种检视

---

## 分流规则

### Mode 1：Code Review（默认）

适用场景：

- review PR
- review diff
- review commit
- 看看这段代码有没有问题
- 检查 bug
- 提交前自查

关注：

- 正确性
- 健壮性
- 安全
- 性能
- 测试
- 可维护性

调用：

`python-code-review`

---

### Mode 2：Committer Review

出现以下任意情况时启用：

用户明确提到：

- committer
- Python语言检视
- 公司规范
- 团队规范
- 工程规范
- 长期维护
- 技术债

或者满足以下条件：

- 修改公共接口
- 修改核心基础库
- 修改框架代码
- 大范围重构
- 跨模块变更
- 新增通用组件

关注：

- 长期可维护性
- 模块边界
- 类型体系
- 工程一致性
- 技术债

调用：

`python-committer-review`

---

### Mode 3：Full Review（推荐）

满足以下任一条件时，同时执行两种检视：

- PR 文件数 ≥ 5
- 涉及多个模块
- 涉及公共接口
- 涉及异步/并发
- 涉及数据库
- 涉及配置体系
- 用户明确要求全面检视

执行顺序：

1. `python-code-review`
2. `python-committer-review`

最后合并输出。

---

## 输出要求

输出前先说明本次采用的检视模式。

例如：

```text
检视模式：Code Review

原因：
- 当前仅涉及业务逻辑修改
- 未涉及公共接口和长期工程治理
```

或者：

```text
检视模式：Full Review

原因：
- 涉及跨模块改动
- 涉及公共接口变更
- 同时进行缺陷检视和工程治理检视
```

然后输出统一的《Python代码检视报告》。

---

## 优先级原则

永远遵循：

真实缺陷 > 长期治理 > 风格统一

不要为了统一风格而修改代码。

不要为了凑数量而输出问题。

像资深同事一样做 Review，而不是像静态扫描器一样报错。