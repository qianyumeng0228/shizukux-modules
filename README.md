# shizukux-modules

ShizukuX ADB 模块在线仓库（模块商店数据源）。

## 结构

```
modules.json                          # 商店索引（客户端读取）
modules/<id>-<version>.zip            # 模块包（module.prop + action.sh / service.sh）
src/<id>/                             # 模块源码（打包前）
```

## modules.json

```json
{
  "name": "ShizukuX 官方模块仓库",
  "schemaVersion": 1,
  "updatedAt": 1750000000,
  "modules": [
    {
      "id": "shizukux.recorder",
      "name": "记录模块",
      "version": "1.0.0",
      "versionCode": 1,
      "author": "qianyumeng0228",
      "description": "把设备信息与运行状态记录到模块目录。",
      "downloadUrl": "https://cdn.jsdelivr.net/gh/qianyumeng0228/shizukux-modules@main/modules/shizukux.recorder-1.0.0.zip",
      "fallbackUrl": "https://raw.githubusercontent.com/qianyumeng0228/shizukux-modules/main/modules/shizukux.recorder-1.0.0.zip",
      "sizeBytes": 2028,
      "sha256": "...",
      "minSdk": 28,
      "hasAction": true,
      "hasService": true,
      "usesWebUI": false,
      "tags": ["记录", "设备信息"],
      "lastUpdated": 1750000000
    }
  ]
}
```

`downloadUrl` 走 jsDelivr，`fallbackUrl` 走 GitHub raw；客户端任一可用即可。

## 模块包格式

ZIP 根目录必须包含 `module.prop`：

```
module.prop
action.sh
service.sh
webroot/index.html   # 可选
```

`module.prop` 字段：`id`（`[A-Za-z][A-Za-z0-9._-]{1,63}`）、`name`、`version`、
`versionCode`、`author`、`description`，可选 `banner`、`webui`/`webroot`、
`action`、`service`、`usesShellBridge`。

脚本通过 Shizuku 服务执行，工作目录为模块目录，环境变量 `MODDIR`、
`ASH_STANDALONE=1`、`SHIZUKU_MODULE_ID`、`SHIZUKU_MODULE_MODE`、
`SHIZUKU_MODULE_TRUSTED`、`SHIZUKU_MODULE_BACKGROUND`。

## 添加模块

1. 在 `src/<id>/` 写 `module.prop` 与脚本；
2. 打包成 `modules/<id>-<version>.zip`（ZIP 根即模块根，UTF-8、LF）；
3. 在 `modules.json` 增加条目（versionCode 递增，更新 `sizeBytes`/`sha256`/`lastUpdated`）。
