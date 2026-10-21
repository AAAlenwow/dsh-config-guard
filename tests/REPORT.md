# dsh-config-guard — Test Report

> Automated test harness, dep-free, runs in pure Node.js.

| Case | Result | Latency | Output | Tokens (est) | Assertions |
|------|--------|---------|--------|--------------|------------|
| 01-valid-yaml | ✅ | 1.3 ms | 490 chars | ~109 | 5 |
| 02-duplicate-keys | ✅ | 0.29 ms | 498 chars | ~111 | 6 |
| 03-tab-yaml | ✅ | 0.19 ms | 471 chars | ~105 | 6 |
| 04-bad-env | ✅ | 0.53 ms | 555 chars | ~123 | 5 |
| 05-missing-required | ✅ | 0.35 ms | 692 chars | ~154 | 7 |
| 06-typo-suggestion | ✅ | 0.3 ms | 623 chars | ~138 | 5 |

## Per-case details
### 01-valid-yaml
- ✅ `ok` → got `true`
- ✅ `errors` → got `[]`
- ✅ `info.fileType` → got `"yaml"`
- ✅ `info.keyCount` → got `7`
- ✅ `info.keys` → got `["server.port","server.host","dataSource.url","dataSource.username","logging.level","logging.appenders[0]","logging.appenders[1]"]`

### 02-duplicate-keys
- ✅ `ok` → got `false`
- ✅ `info.fileType` → got `"yaml"`
- ✅ `errors.0.severity` → got `"error"`
- ✅ `errors.0.message` → got `"重复的键 \"app.name\"(第一次出现在第 4 行)"`
- ✅ `errors.0.line` → got `8`
- ✅ `info.keyCount` → got `4`

### 03-tab-yaml
- ✅ `ok` → got `false`
- ✅ `info.fileType` → got `"yaml"`
- ✅ `errors.0.severity` → got `"error"`
- ✅ `errors.0.line` → got `3`
- ✅ `errors.0.message` → got `"YAML 不允许使用 tab 缩进,请改为空格"`
- ✅ `info.keyCount` → got `3`

### 04-bad-env
- ✅ `ok` → got `false`
- ✅ `info.fileType` → got `"env"`
- ✅ `info.keyCount` → got `3`
- ✅ `errors.0.severity` → got `"error"`
- ✅ `errors.0.message` → got `"键名 \"bad name\" 不是合法的标识符(应以字母或下划线开头)"`

### 05-missing-required
- ✅ `ok` → got `false`
- ✅ `info.fileType` → got `"yaml"`
- ✅ `info.keyCount` → got `2`
- ✅ `errors.0.severity` → got `"error"`
- ✅ `errors.0.message` → got `"缺少必需键 \"server.host\""`
- ✅ `errors.1.severity` → got `"error"`
- ✅ `errors.1.message` → got `"缺少必需键 \"dataSource.url\""`

### 06-typo-suggestion
- ✅ `ok` → got `false`
- ✅ `info.fileType` → got `"yaml"`
- ✅ `info.keyCount` → got `3`
- ✅ `warnings.0.severity` → got `"warning"`
- ✅ `warnings.0.message` → got `"缺少 \"dataSource.url\",是否想写 \"datasource.url\"?(编辑距离 1)"`
