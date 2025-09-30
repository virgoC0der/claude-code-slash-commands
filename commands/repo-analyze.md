Analyze a code repository using Gemini CLI collaboration to leverage Gemini's large context window and Claude's detailed analysis capabilities.

**Analysis Type**: $ARGUMENTS (project, gitignored)

1. Environment Setup:
   - Verify Gemini CLI is available (npm install -g @google/gemini-cli if missing)
   - Validate repository path exists
   - Create output directory structure for results
   - Record analysis metadata (timestamp, tools, versions)

2. Gemini Global Analysis (Phase 1):
   - IMPORTANT: executing each step in this phase by using `gemini -p`
   - Execute comprehensive architecture scan using `gemini -p`
   - Analyze project structure and organization
   - Identify technology stack and dependencies
   - Map core components and their relationships
   - Detect circular dependencies and coupling issues
   - Generate JSON output for structured analysis

3. Detailed Code Analysis (Phase 2):
   - Review Gemini's global analysis results
   - Perform deep code quality assessment:
     * Cyclomatic complexity evaluation
     * Cognitive complexity and readability
     * Code duplication detection
     * Naming conventions and consistency
   - Identify performance bottlenecks:
     * Algorithmic complexity issues
     * N+1 query problems
     * Memory leak risks
     * Concurrency and race conditions
   - Security vulnerability scanning:
     * Input validation gaps (SQL injection, XSS, path traversal)
     * Authentication and authorization issues
     * Hardcoded credentials detection
     * Sensitive data exposure risks

4. Documentation Generation (Phase 3):
   - Create API documentation with usage examples
   - Generate architecture documentation with Mermaid diagrams
   - Produce developer guide covering:
     * Environment setup
     * Project structure explanation
     * Development workflow
     * Common tasks and debugging
     * Performance optimization tips

5. Report Compilation (Phase 4):
   - Generate comprehensive analysis report including:
     * Executive summary with key findings
     * Code quality metrics table
     * Security assessment with prioritized issues
     * Performance analysis and recommendations
     * Action plan (P0: critical, P1: short-term, P2: long-term)
   - Create interactive HTML dashboard with visualizations
   - Organize all outputs in structured directory

6. Advanced Options:
   - Batch processing mode for large repositories (>100k files)
   - Incremental analysis for changed files only
   - Custom analysis type combinations

Output structure saved to timestamped directory with subdirectories: raw_data/, reports/, documentation/, metrics/

Focus on actionable insights and prioritize findings by severity. Ask for clarification if repository scope or analysis depth is unclear.