## 快速开始

### 1. 前置要求

- git

### 2. 克隆仓库

```powershell
git clone https://github.com/icgnos/poopsky.git
cd poopsky
```

### 3. 获取 packwiz

从 <https://nightly.link/packwiz/packwiz/workflows/go/main> 下载对应系统的压缩包，解压后放到仓库根目录。

### 4. 创建本地索引

手动创建一个空白的 `index.toml` 文件，然后刷新一次：

```powershell
./packwiz.exe refresh
```

### 5. 验证环境

```powershell
./packwiz.exe list
```

能列出 mod 即就绪。

## 常用命令

### 添加 mod

```powershell
./packwiz.exe modrinth add <slug/URL>
```

新 mod 优先放进匹配的分类目录（可依据MC百科）。

### 导出整合包

```powershell
./packwiz.exe modrinth export
```

### 刷新索引

```powershell
./packwiz.exe refresh
```

导出整合包时会自动刷新索引，所以一般无需手动刷新。