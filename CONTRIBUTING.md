## 快速开始

### 1. 前置要求

- git

### 2. 克隆仓库

```powershell
git clone https://github.com/SJMC-Dev/SMP3-modpack.git
cd SMP3-modpack
git switch dev
```

日常开发都在 `dev` 分支上进行。

### 3. 获取 packwiz

从 <https://nightly.link/packwiz/packwiz/workflows/go/main> 下载对应系统的压缩包，解压后放到仓库根目录。

## 日常开发流程

一次完整的改动是：**搭好测试环境 → 加 mod / 改配置 → 仓库中同步修改 → 推送到 `dev`**。

推送到 `dev` 后，GitHub Actions 会自动刷新 packwiz 索引，并把快照 force push 到本仓库的 `release` 分支；
Gitee 镜像 [icgnos/smp3-modpack](https://gitee.com/icgnos/smp3-modpack) 会自动从 GitHub 同步该分支，玩家端 unsup 更新时就能拿到。

### 1. 搭建客户端环境

1. 准备一个 **Minecraft 26.3 + Fabric** 的客户端实例。
2. 将仓库根目录的`unsup.jar` 和 `unsup.ini` 一起放进实例所在的文件夹。
3. 在启动器的 JVM 参数里加上 `-javaagent:unsup.jar`。
4. 正常启动游戏：unsup 会按 `unsup.ini` 里的 `source`（指向 Gitee 上 `release` 分支的 `pack.toml`）拉取 mod 与配置并写入实例。
5. 之后每次启动都会自动检查更新。

### 2. 搭建服务端环境

1. 本地开一个 **Minecraft 26.3 + Fabric 服务端**。
2. 把 `unsup.jar` 和 `unsup.ini` 放到服务端根目录，启动脚本的 JVM 参数加 `-javaagent:unsup.jar`。
3. 正常启动，unsup 同样会拉取 mod 与配置。
4. `eula.txt`、`server.properties`、`ops.json` 这类只跟本机环境有关的文件不要提交。
5. 客户端专属的 mod（`.pw.toml` 里 `side = "client"`）在服务端不会被安装，反之亦然。

### 3. 添加模组

在实例中添加模组后，需要在仓库中通过 packwiz 同步添加。

- 请确定你的根目录下存在 `index.toml`. 如果没有，请手动创建一个空白文件。

优先用 Modrinth 来源添加：

```powershell
./packwiz.exe modrinth add <slug/URL>
```

其它来源：

```powershell
./packwiz.exe curseforge add <URL/slug/ID>   # CurseForge
./packwiz.exe github add <仓库地址>          # GitHub Release
./packwiz.exe url add <直链>                 # 直接下载链接
```

- 在 `modlist.csv` 里登记。
- 能在上面这些来源找到的 mod，仓库里只提交 `mods/*.pw.toml`，**不要**提交本地下载下来的 jar，除非的确没有可用来源。

### 4. 修改配置

- 在实例里把配置调好。
- 找出对应的 `config/` 文件，**只把与自动生成的默认值的差异**搬进仓库，不要整份复制过去。
- 可行做法：先在没改过的实例里把自动生成的文件存一份副本，再用 WinMerge 之类的对比工具找出差异，只提交差异部分。
- 日志、缓存、`eula.txt`、`server.properties` 这类端侧文件不属于仓库内容。

### 6. 提交并推送

- 确保处于 `dev` 分支。

## 常用命令

| 命令 | 作用 |
| --- | --- |
| `./packwiz.exe list` | 列出整合包里的所有 mod |
| `./packwiz.exe update [mod]` | 更新 mod（不带参数则更新全部） |
| `./packwiz.exe remove <mod>` | 移除 mod |

完整命令列表见 `./packwiz.exe --help`。
