# 🚀 代码仓库智能协作分析工具

使用 Gemini CLI 和 Claude Code 协作深度分析代码仓库。

**分析参数**: $ARGUMENTS

## 📋 支持的分析类型

- `architecture` - 架构和设计模式分析
- `functions` - 函数接口和调用关系分析  
- `quality` - 代码质量和可维护性评估
- `security` - 安全漏洞和风险扫描
- `performance` - 性能瓶颈和优化建议
- `documentation` - 自动文档生成
- `full` - 完整的综合分析（推荐）

## 🔧 环境检查和初始化

### 1. 验证工具可用性

```bash
# 检查 Gemini CLI
`which gemini > /dev/null 2>&1 || (echo "❌ Gemini CLI 未安装，请运行: npm install -g @google/gemini-cli" && exit 1)`

# 验证仓库路径
`[[ -d "${2:-.}" ]] || (echo "❌ 仓库路径不存在: ${2:-.}" && exit 1)`
```

### 2. 设置工作环境

```bash
# 设置变量
ANALYSIS_TYPE="${1:-full}"
REPO_PATH="${2:-.}"
OUTPUT_DIR="${3:-./analysis_results_$(date +%Y%m%d_%H%M%S)}"

# 创建输出目录结构
`mkdir -p "$OUTPUT_DIR"/{raw_data,reports,documentation,metrics}`
`echo "📁 输出目录创建成功: $OUTPUT_DIR"`

# 记录分析元数据
`cat > "$OUTPUT_DIR/metadata.json" << EOF
{
  "analysis_type": "$ANALYSIS_TYPE",
  "repository_path": "$(realpath "$REPO_PATH")",
  "start_time": "$(date -u +"%Y-%m-%dT%H:%M:%SZ")",
  "tools": {
    "gemini_version": "$(gemini --version 2>/dev/null || echo 'unknown')",
    "claude_model": "claude-3-5-sonnet-20241022"
  }
}
EOF`
```

## 🔍 第一阶段：Gemini 全局扫描分析

### 架构扫描（利用1M Token优势）

```bash
`echo "🔍 开始Gemini全局架构扫描..."`

# 收集仓库统计信息
`echo "📊 仓库统计:" && find "$REPO_PATH" -type f -name "*.{js,py,java,cpp,go,rs,ts,jsx,vue}" 2>/dev/null | wc -l | xargs -I {} echo "源代码文件数: {}"`

# 执行全局架构分析
`gemini -p "
 @$REPO_PATH
请对这个代码仓库进行全面的架构分析：

1. 项目结构和组织
   - 识别主要目录结构和其用途
   - 分析模块划分策略
   - 评估代码组织的合理性

2. 技术栈识别
   - 主要编程语言和框架
   - 依赖管理和第三方库
   - 构建和部署工具链

3. 架构模式
   - 识别使用的设计模式（MVC, MVVM, 微服务等）
   - 分析分层架构
   - 评估架构的可扩展性

4. 核心组件
   - 列出关键业务模块
   - 分析核心数据模型
   - 识别关键接口和服务

5. 依赖关系
   - 模块间的依赖关系图
   - 循环依赖检测
   - 耦合度分析

请以JSON格式输出，包含：
{
  'project_overview': {...},
  'architecture_pattern': {...},
  'core_components': [...],
  'dependencies': {...},
  'quality_metrics': {...},
  'recommendations': [...]
}
"> "$OUTPUT_DIR/raw_data/gemini_architecture.json"`

`echo "✅ 架构扫描完成"`
```

### 功能模块深度分析

```bash
`echo "🔍 开始功能模块分析..."`

# 分析主要源代码目录
`gemini -p "
 @$REPO_PATH
分析代码仓库中的所有功能模块，对每个模块执行以下任务：

1. 模块功能识别
   - 模块的主要职责和功能
   - 对外暴露的公共接口
   - 模块的使用场景

2. 函数和方法分析
   - 列出所有公共函数/方法
   - 分析函数签名和参数
   - 识别返回值类型和异常

3. 数据流分析
   - 输入数据的处理流程
   - 数据转换和验证逻辑
   - 输出数据的格式化

4. 调用关系
   - 内部函数调用链
   - 外部依赖调用
   - 回调和事件处理

5. 使用示例生成
   - 为主要功能生成调用示例
   - 最佳实践建议
   - 常见错误和解决方案

输出格式要求：
{
  'modules': [
    {
      'name': '模块名',
      'path': '文件路径',
      'description': '功能描述',
      'public_functions': [...],
      'usage_examples': [...],
      'dependencies': [...]
    }
  ]
}
" > "$OUTPUT_DIR/raw_data/gemini_functions.json"`

`echo "✅ 功能模块分析完成"`
```

## 🎯 第二阶段：Claude 精细化处理

### 代码质量深度评估

基于 Gemini 的全局分析结果，我现在进行精细化的代码质量分析：

```bash
# 读取Gemini分析结果
READ_FILE "$OUTPUT_DIR/raw_data/gemini_architecture.json"
READ_FILE "$OUTPUT_DIR/raw_data/gemini_functions.json"
```

现在基于这些数据进行深度分析：

#### 1. 代码复杂度分析

- **圈复杂度评估**: 识别复杂度过高的函数
- **认知复杂度**: 评估代码的可读性和理解难度
- **嵌套深度**: 检查过深的控制流嵌套
- **代码重复**: 识别重复代码块

#### 2. 最佳实践检查

- **命名规范**: 变量、函数、类的命名一致性
- **错误处理**: 异常捕获和错误传播机制
- **日志记录**: 日志级别和内容的合理性
- **注释质量**: 代码注释的完整性和准确性

#### 3. 性能分析

- **算法复杂度**: 识别O(n²)及以上的低效算法
- **数据库查询**: N+1查询问题检测
- **内存泄漏风险**: 未释放的资源和循环引用
- **并发问题**: 竞态条件和死锁风险

### 安全漏洞扫描

```bash
`echo "🔒 开始安全性分析..."`
```

执行安全检查：

1. **输入验证**
   - SQL注入风险点
   - XSS攻击向量
   - 路径遍历漏洞

2. **认证和授权**
   - 硬编码凭证检测
   - 弱加密算法使用
   - 权限提升漏洞

3. **数据保护**
   - 敏感数据泄露
   - 不安全的数据传输
   - 日志中的敏感信息

## 📝 第三阶段：文档自动生成

### API 文档生成

基于分析结果，生成完整的API文档：

```bash
WRITE_FILE "$OUTPUT_DIR/documentation/API.md" "# API 文档

## 概述

本文档基于代码仓库自动分析生成，包含所有公共API的详细说明。

## 模块列表

[基于分析结果生成模块列表和API详情]

## 使用示例

[为每个主要API提供调用示例]

## 错误处理

[说明错误码和异常处理方式]
"
```

### 架构文档生成

```bash
WRITE_FILE "$OUTPUT_DIR/documentation/ARCHITECTURE.md" "# 系统架构文档

## 架构概览

[基于Gemini分析生成架构描述]

## 技术栈

[列出识别的技术栈]

## 核心组件

[描述各个核心组件及其职责]

## 数据流图

\`\`\`mermaid
graph TD
    A[客户端] --> B[API网关]
    B --> C[业务服务]
    C --> D[数据层]
\`\`\`

## 部署架构

[描述部署结构和要求]
"
```

### 开发者指南

```bash
WRITE_FILE "$OUTPUT_DIR/documentation/DEVELOPER_GUIDE.md" "# 开发者指南

## 快速开始

### 环境准备
[开发环境配置说明]

### 项目结构
[详细的目录结构说明]

### 开发流程
[代码提交、审查、测试流程]

## 核心概念

[解释项目中的关键概念和术语]

## 常见任务

### 添加新功能
[步骤说明和示例]

### 调试指南
[调试工具和技巧]

### 性能优化
[性能分析和优化方法]

## FAQ

[常见问题和解答]
"
```

## 📊 第四阶段：结果整合和报告生成

### 生成综合分析报告

```bash
`echo "📊 生成综合分析报告..."`

WRITE_FILE "$OUTPUT_DIR/reports/comprehensive_analysis.md" "# 代码仓库综合分析报告

**生成时间**: $(date)
**仓库路径**: $REPO_PATH
**分析类型**: $ANALYSIS_TYPE

## 执行摘要

通过 Gemini CLI (1M token上下文) 和 Claude Code 的协作分析，我们对代码仓库进行了全面评估。

### 关键发现

- **代码规模**: [从分析中提取]
- **技术债务评级**: [基于质量分析评定]
- **安全风险等级**: [基于安全扫描结果]
- **文档完整度**: [基于文档分析]

## 详细分析结果

### 1. 架构质量评估

[基于Gemini和Claude的分析结果]

### 2. 代码质量指标

| 指标 | 分值 | 评级 | 建议 |
|------|------|------|------|
| 可维护性 | - | - | - |
| 可测试性 | - | - | - |
| 复杂度 | - | - | - |
| 重复度 | - | - | - |

### 3. 安全性评估

[列出发现的安全问题和修复建议]

### 4. 性能分析

[性能瓶颈和优化建议]

## 行动计划

### 立即处理（P0）
1. [关键安全漏洞]
2. [严重性能问题]

### 短期改进（P1）
1. [代码质量问题]
2. [架构优化]

### 长期规划（P2）
1. [技术债务清理]
2. [架构重构]

## 工具协作优势

- **Gemini CLI**: 提供了完整的全局视图和上下文理解
- **Claude Code**: 提供了精细的代码分析和文档生成
- **协作效果**: 实现了1+1>2的分析效果

## 附录

- 原始数据: $OUTPUT_DIR/raw_data/
- API文档: $OUTPUT_DIR/documentation/API.md
- 架构文档: $OUTPUT_DIR/documentation/ARCHITECTURE.md
- 开发指南: $OUTPUT_DIR/documentation/DEVELOPER_GUIDE.md
"
```

### 生成可视化报告

```bash
WRITE_FILE "$OUTPUT_DIR/reports/metrics_dashboard.html" "<DOCTYPE html>
<html>
<head>
    <title>代码分析仪表板</title>
    <script src='https://cdn.plot.ly/plotly-latest.min.js'></script>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .metric-card { 
            border: 1px solid #ddd; 
            padding: 15px; 
            margin: 10px; 
            border-radius: 5px;
            display: inline-block;
            width: 45%;
        }
        h1 { color: #333; }
        .chart { width: 100%; height: 400px; }
    </style>
</head>
<body>
    <h1>代码仓库分析仪表板</h1>
    
    <div class='metric-card'>
        <h3>代码质量分布</h3>
        <div id='quality-chart' class='chart'></div>
    </div>
    
    <div class='metric-card'>
        <h3>模块复杂度</h3>
        <div id='complexity-chart' class='chart'></div>
    </div>
    
    <div class='metric-card'>
        <h3>技术债务趋势</h3>
        <div id='debt-chart' class='chart'></div>
    </div>
    
    <div class='metric-card'>
        <h3>测试覆盖率</h3>
        <div id='coverage-chart' class='chart'></div>
    </div>
    
    <script>
        // [基于分析数据生成可视化图表的JavaScript代码]
    </script>
</body>
</html>"
```

## 🎯 执行完成和清理

```bash
# 记录完成时间
`echo "{\"end_time\": \"$(date -u +"%Y-%m-%dT%H:%M:%SZ")\"}" >> "$OUTPUT_DIR/metadata.json"`

# 生成摘要
`echo ""`
`echo "========================================"`
`echo "✅ 代码仓库分析完成！"`
`echo "========================================"`
`echo ""`
`echo "📁 结果目录: $OUTPUT_DIR"`
`echo ""`
`echo "📊 主要输出:"`
`echo "  • 综合报告: $OUTPUT_DIR/reports/comprehensive_analysis.md"`
`echo "  • API文档: $OUTPUT_DIR/documentation/API.md"`
`echo "  • 架构文档: $OUTPUT_DIR/documentation/ARCHITECTURE.md"`
`echo "  • 开发指南: $OUTPUT_DIR/documentation/DEVELOPER_GUIDE.md"`
`echo "  • 可视化仪表板: $OUTPUT_DIR/reports/metrics_dashboard.html"`
`echo ""`
`echo "📈 分析统计:"`
`ls -la "$OUTPUT_DIR/raw_data/" | tail -n +2 | wc -l | xargs -I {} echo "  • Gemini分析文件: {} 个"`
`find "$OUTPUT_DIR/documentation" -name "*.md" | wc -l | xargs -I {} echo "  • 生成文档: {} 份"`
`echo ""`
`echo "💡 下一步建议:"`
`echo "  1. 查看综合分析报告了解整体情况"`
`echo "  2. 根据优先级处理发现的问题"`
`echo "  3. 使用生成的文档更新项目文档"`
`echo "  4. 在浏览器中打开仪表板查看可视化数据"`
```

## 🛠️ 高级选项和自定义

### 批量处理大型仓库

对于超大型仓库（>10万文件），使用分批处理：

```bash
# 如果检测到大型仓库，自动切换到批处理模式
`FILE_COUNT=$(find "$REPO_PATH" -type f | wc -l)`
`if [ $FILE_COUNT -gt 100000 ]; then
    echo "⚠️ 检测到大型仓库 ($FILE_COUNT 文件)，切换到批处理模式..."
    
    # 分批处理逻辑
    find "$REPO_PATH" -type f -name "*.{js,py,java}" | 
    split -l 1000 - "$OUTPUT_DIR/batch_" 
    
    for batch in "$OUTPUT_DIR"/batch_*; do
        gemini -p "分析这批文件  @$batch" >> "$OUTPUT_DIR/batch_results.jsonl"
        sleep 1  # 避免API限流
    done
fi`
```

### 增量分析模式

仅分析自上次分析后的变更：

```bash
# 检查是否存在上次分析记录
`if [ -f ".last_analysis_hash" ]; then
    echo "🔄 检测到上次分析记录，执行增量分析..."
    
    # 获取变更文件列表
    git diff --name-only $(cat .last_analysis_hash) HEAD > "$OUTPUT_DIR/changed_files.txt"
    
    # 仅分析变更文件
    gemini -p "分析这些变更文件的影响 @$OUTPUT_DIR/changed_files.txt"
fi`

# 保存当前提交哈希
`git rev-parse HEAD > .last_analysis_hash`
```

## 💡 使用示例

### 基础用法

```bash
# 分析当前目录，默认完整分析
/repo-analyze full

# 分析指定仓库
/repo-analyze full /path/to/repo

# 仅架构分析
/repo-analyze architecture ./my-project ./output

# 仅生成文档
/repo-analyze documentation
```

### 高级用法

```bash
# 安全专项分析
/repo-analyze security ./sensitive-project ./security-audit

# 性能优化分析
/repo-analyze performance ./slow-app ./perf-analysis

# 组合多种分析
/repo-analyze "architecture,quality,security" ./project ./combined-output
```

## 🔧 故障排除

### 常见问题

1. **Gemini CLI 未找到**
   ```bash
   npm install -g @google/gemini-cli
   ```

2. **API 配额超限**
   - 使用批处理模式
   - 增加请求间隔
   - 考虑升级API计划

3. **内存不足**
   - 减小批处理大小
   - 排除大型二进制文件
   - 使用流式处理

## 📚 相关资源

- [Gemini CLI 文档](https://github.com/google-gemini/gemini-cli)
- [Claude Code 文档](https://docs.anthropic.com/en/docs/claude-code)
- [MCP 协议规范](https://modelcontextprotocol.io)

---

*此命令通过 Claude Code 和 Gemini CLI 的协作，提供业界领先的代码仓库分析能力。*
