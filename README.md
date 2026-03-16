# OpenClaw on MacOS 11

让 OpenClaw 在 MacOS 11 (Big Sur) 上运行的适配方案。

## 背景

OpenClaw 官方版本要求较新的系统依赖，在 MacOS 11 上安装会遇到兼容性问题。本项目提供修改后的配置，让 OpenClaw 3.3 版本能在 MacOS 11 上成功运行。

## 适配版本

- **OpenClaw 版本**: 3.3 (2026.3.3)
- **目标系统**: MacOS 11 (Big Sur)
- **Node.js 要求**: >= 22.12.0

## 主要修改

### 1. package.json 调整

#### 依赖版本降级（兼容 MacOS 11）

| 包名 | 原版 (3.14) | 修改版 (3.3) | 说明 |
|------|-------------|--------------|------|
| `@agentclientprotocol/sdk` | 0.16.1 | 0.14.1 | 降级 |
| `@aws-sdk/client-bedrock` | ^3.1009.0 | ^3.1000.0 | 降级 |
| `@clack/prompts` | ^1.1.0 | ^1.0.1 | 降级 |
| `@discordjs/voice` | ^0.19.1 | ^0.19.0 | 降级 |
| `@mariozechner/pi-*` | 0.58.0 | 0.55.3 | 降级 |
| `@slack/web-api` | ^7.15.0 | ^7.14.1 | 降级 |
| `discord-api-types` | ^0.38.42 | ^0.38.40 | 降级 |
| `file-type` | ^21.3.2 | ^21.3.0 | 降级 |
| `grammy` | ^1.41.1 | ^1.41.0 | 降级 |
| `https-proxy-agent` | ^8.0.0 | ^7.0.6 | 降级 |
| `tar` | 7.5.11 | 7.5.10 | 降级 |
| `undici` | ^7.24.1 | ^7.22.0 | 降级 |

#### 开发依赖调整

| 包名 | 原版 | 修改版 |
|------|------|--------|
| `@types/node` | ^25.5.0 | ^25.3.3 |
| `@typescript/native-preview` | 7.0.0-dev.20260313.1 | 7.0.0-dev.20260301.1 |
| `@vitest/coverage-v8` | ^4.1.0 | ^4.0.18 |
| `oxfmt` | 0.40.0 | 0.35.0 |
| `oxlint` | ^1.55.0 | ^1.50.0 |
| `oxlint-tsgolint` | ^0.16.0 | ^0.15.0 |
| `tsdown` | 0.21.2 | 0.21.0-beta.2 |
| `vitest` | ^4.1.0 | ^4.0.18 |

#### 关键修改

1. **新增 `preinstall` 脚本**: 运行 MacOS 兼容性安装脚本
   ```json
   "preinstall": "node scripts/macos-compat-install.js"
   ```

2. **新增 `overrides` 字段**: 强制使用特定 esbuild 版本
   ```json
   "overrides": {
     "esbuild": "0.19.12"
   }
   ```

3. **pnpm 覆盖调整**:
   - `hono`: 4.12.7 → 4.12.5
   - 新增 `esbuild`: 0.19.12 覆盖
   - 移除 `file-type` 覆盖（已在 dependencies 中指定）

4. **移除的依赖** (MacOS 11 不支持):
   - `@larksuiteoapi/node-sdk`
   - `@modelcontextprotocol/sdk`
   - `hono`
   - `jscpd`
   - `long`
   - `osc-progress`
   - `jsdom`

5. **构建脚本调整**:
   - 使用 `tsdown` 替代 `node scripts/tsdown-build.mjs`
   - 简化部分构建流程

## 安装方法

### 前提条件

1. 安装 Node.js >= 22.12.0
2. 安装 pnpm 10.23.0:
   ```bash
   npm install -g pnpm@10.23.0
   ```

### 安装步骤

1. **克隆本仓库**:
   ```bash
   git clone https://github.com/liujun2051/OpenClaw_on_Mac11.git
   cd OpenClaw_on_Mac11
   ```

2. **运行安装脚本**:
   ```bash
   pnpm install
   ```

3. **构建项目**:
   ```bash
   pnpm build
   ```

4. **启动 OpenClaw**:
   ```bash
   pnpm start
   ```

## 已知问题

- 部分新功能可能不可用（因依赖版本降级）
- 3.13 版本仍在攻克中，欢迎贡献

## 贡献者

- **Dreamer** - 项目发起、适配方案设计
- **飞儿** - 技术支持、测试验证
- **小龙** - 代码整理、GitHub 上传

## 许可证

MIT License - 与 OpenClaw 原项目保持一致

## 相关链接

- [OpenClaw 官方仓库](https://github.com/openclaw/openclaw)
- [OpenClaw 文档](https://docs.openclaw.ai)

---

**注意**: 本项目非官方支持，仅供 MacOS 11 用户参考使用。
