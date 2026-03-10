# Quickstart

## 什么时候用

适合：

- bugfix
- feature
- refactor
- 性能优化
- 测试补强
- 事故复盘
- 架构审计
- gate / handoff

尤其适合：

- “问题复杂、不能只看单个文件”
- “离线通过但真实环境失败”
- “需要最小可验证变更”
- “需要系统级收口，不只是修一行代码”

## 最短使用姿势

在提示词里显式写：

- `$cybernetic-systems-engineering`

如果任务会持续多轮、需要检查点和恢复，建议同时写：

- `$harness`

## 最小输出模板

### Control Contract

- Goal:
- Acceptance:
- Constraints:
- Boundary:
- Risks:

### State Estimate

- Entry:
- Key state:
- Key invariants:
- Current error:

### Plan

1. 先测量
2. 再最小修复
3. 分层验证
4. 记录 residual risk

## 一个典型例子

### 用户问题

“本地 `cargo test` 全绿，但真实 Windows + VPN 环境下 `PG_CH` 回测炸了。请定位原因并补离线测试。”

### 这个 skill 会做什么

1. 先建立控制合同
2. 区分：
   - 语义层测试
   - schema 契约层测试
   - 真实环境 gate
3. 把问题从“单个 bugfix”升级成：
   - 根因定位
   - 测试缺口审计
   - 最小修复
   - 离线回归矩阵
   - gate handoff

## 记住

这个 skill 的核心不是：

- 用更多术语
- 画更复杂的图

而是：

- 更快得到可信误差信号
- 更小地改动
- 更清楚地区分“已验证”与“尚未验证”
