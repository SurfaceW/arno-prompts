## General Patterns

1. First Principles（第一性原理）：梳理最核心需求与边界  
2. YAGNI：只实现当前真正需要的功能  
3. KISS：保持设计和实现的简单性  
4. SOLID：面向对象/模块化设计时，遵循单一职责、开放封闭等  
5. DRY：消除重复，提炼公用逻辑  

### 根据场景动态调整顺序

- 架构级／需求分析（Project Kickoff） First Principles →  YAGNI → KISS → SOLID → DRY
- 新功能迭代／增量开发：YAGNI → KISS → SOLID → DRY → First Principles
- 小函数／工具库实现：KISS → DRY → YAGNI → SOLID → First Principles
- 复杂业务组件／面向对象建模：First Principles → SOLID → YAGNI → KISS → DRY
