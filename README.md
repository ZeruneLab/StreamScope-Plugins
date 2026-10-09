# StreamScope 官方插件库

为 [StreamScope](https://github.com/ZeruneLab/StreamScope) 提供独立发布的签名功能插件及共享运行时。面向 Windows x64，用户下载和安装不需要 GitHub 登录。

## 安装

安装 StreamScope v0.2.0 或更高版本，在“插件管理”点击“检查官方插件目录”，按需选择功能并确认发布者、权限和原生代码风险。媒体功能会安装共享 FFmpeg 运行时，无需单独配置系统 FFmpeg。

离线用户可从本仓库 Releases 下载 `.ssp`，先导入依赖再导入功能包。插件按 Windows 用户安装，报告在卸载插件时保留。

## 目录结构与信任

- `catalog.json`：所有已发布兼容版本的目录，包 URL 指向本仓库具体 Release 标签，不能使用 `releases/latest` 作为包地址。
- `catalog.json.sig`：目录原始字节的 Ed25519 签名，Base64 格式。
- `catalog.pub`：目录公钥，仅供核对；软件不会动态相信仓库内的新公钥，信任根来自已安装主程序。
- `publisher.pub`：官方插件发布者公钥，仅供核对。
- `releases/download/<批次>/<插件ID>-<版本>-windows-x86_64.ssp`：签名插件包；同一版本不覆盖，更新须发布新版本。

固定目录地址：https://raw.githubusercontent.com/ZeruneLab/StreamScope-Plugins/main/catalog.json

正式插件发布者指纹：`sha256:8f1e1ccefc22442539ee9084cd16835ae4e211008a99a130f4dd1263b465bbbf`。

签名证明来源和完整性，不保证原生代码安全。插件是独立 EXE，以当前 Windows 用户权限运行，没有系统级沙箱；请只安装可信发布者的插件。

## 从本地预览迁移

0.1.10/0.1.11 本地预览插件由临时密钥签名，不自动换成正式签名。先备份 `%LOCALAPPDATA%\StreamScope\plugins`，在无任务运行时先卸载测试功能插件，再卸载被它们依赖的测试运行时包，随后通过官方目录重新确认安装。不要手动改注册表或绕过验签；报告不会随插件卸载删除。

## 维护者发布流程

1. 插件源代码、Schema 和开发文档在主程序仓库维护；本仓库只存公开目录、公钥及发布说明。私钥不进任何仓库或 Release。
2. 使用固定发布者密钥构建 `.ssp`，每个插件分别递增 SemVer；运行隔离安装、目录验签、依赖和工作流回归。
3. 创建新批次的草稿 Release，上传完整包并验证，之后发布。永远不覆盖已经被目录引用的同版本包。
4. 用固定目录密钥签署新目录，在单个提交中更新 `catalog.json` 和 `.sig`。保留仍受支持的旧版本条目；不要覆盖在用平台或删除旧包。
5. 验证公开目录及全部资产无需身份认证即可访问，哈希、签名、Host API 和依赖一致；主程序 Release 不受插件发布影响。

主程序更新清单仅在 `ZeruneLab/StreamScope` 发布。本仓库发布任何新插件，都不会改变主程序的最新版本。
