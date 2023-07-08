# 一、WSL的安装



##  1、为Windows启用Linux子系统功能

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```



## 2、启用Windows的虚拟机功能

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```



## 3、更新Linux内核

```http
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
```



## 4、设置WSL2为默认版本

```powershell
wsl --set-default-version 2
```



## 5、到微软商店下载Linux系统并安装





# 二、WSL的基本指令

以下 WSL 命令以 PowerShell 或 Windows 命令提示符支持的格式列出。 若要通过 Bash/Linux 发行版命令行运行这些命令，必须将 `wsl` 替换为 `wsl.exe`。

## 安装

```powershell
wsl --install
```

安装 WSL 和 Linux 的 Ubuntu 发行版。 [了解详细信息](https://learn.microsoft.com/zh-cn/windows/wsl/install)。

## 安装特定的 Linux 发行版

```powershell
wsl --install --distribution <Distribution Name>
```

通过将 `<Distribution Name>` 替换为发行版名称，指定除默认发行版 (Ubuntu) 之外的 Linux 发行版进行安装。 此命令也可输入为：`wsl -d <Distribution Name>`。

## 列出可用的 Linux 发行版

```powershell
wsl --list --online
```

查看可通过在线商店获得的 Linux 发行版列表。 此命令也可输入为：`wsl -l -o`。

## 列出已安装的 Linux 发行版

```powershell
wsl --list --verbose
```

查看安装在 Windows 计算机上的 Linux 发行版列表，其中包括状态（发行版是正在运行还是已停止）和运行发行版的 WSL 版本（WSL 1 或 WSL 2）。 [比较 WSL 1 和 WSL 2](https://learn.microsoft.com/zh-cn/windows/wsl/compare-versions)。 此命令也可输入为：`wsl -l -v`。 可与 list 命令一起使用的其他选项包括：`--all`（列出所有发行版）、`--running`（仅列出当前正在运行的发行版）或 `--quiet`（仅显示发行版名称）。

## 将 WSL 版本设置为 1 或 2

```powershell
wsl --set-version <distribution name> <versionNumber>
```

若要指定运行 Linux 发行版的 WSL 版本（1 或 2），请将 `<distribution name>` 替换为发行版的名称，并将 `<versionNumber>` 替换为 1 或 2。 [比较 WSL 1 和 WSL 2](https://learn.microsoft.com/zh-cn/windows/wsl/compare-versions)。

## 设置默认 WSL 版本

```powershell
wsl --set-default-version <Version>
```

若要将默认版本设置为 WSL 1 或 WSL 2，请将 `<Version>` 替换为数字 1 或 2，表示对于安装新的 Linux 发行版，你希望默认使用哪个版本的 WSL。 例如，`wsl --set-default-version 2`。 [比较 WSL 1 和 WSL 2](https://learn.microsoft.com/zh-cn/windows/wsl/compare-versions)。

## 设置默认 Linux 发行版

```powershell
wsl --set-default <Distribution Name>
```

若要设置 WSL 命令将用于运行的默认 Linux 发行版，请将 `<Distribution Name>` 替换为你首选的 Linux 发行版的名称。

## 将目录更改为主页

```powershell
wsl ~
```

`~` 可与 wsl 一起使用，以在用户的主目录中启动。 若要在 WSL 命令提示符中从任何目录跳回到主目录，可使用命令 `cd ~`。

## 通过 PowerShell 或 CMD 运行特定的 Linux 发行版

```powershell
wsl --distribution <Distribution Name> --user <User Name>
```

若要通过特定用户运行特定 Linux 发行版，请将 `<Distribution Name>` 替换为你首选的 Linux 发行版的名称（例如 Debian），将 `<User Name>` 替换为现有用户的名称（例如 root）。 如果 WSL 发行版中不存在该用户，你将会收到一个错误。 若要输出当前用户名，请使用 `whoami` 命令。

## 更新 WSL

```powershell
wsl --update
```

手动更新 WSL Linux 内核的版本。 还可以使用 `wsl --update rollback` 命令回滚到 WSL Linux 内核的上一版本。

## 检查 WSL 状态

```powershell
wsl --status
```

查看有关 WSL 配置的常规信息，例如默认发行版类型、默认发行版和内核版本。

## Help 命令

```powershell
wsl --help
```

查看 WSL 中可用的选项和命令列表。

## 以特定用户的身份运行

```powershell
wsl -u <Username>`, `wsl --user <Username>
```

若要以指定用户身份运行 WSL，请将 `<Username>` 替换为 WSL 发行版中存在的用户名。

## 更改发行版的默认用户

PowerShell复制

```powershell
<DistributionName> config --default-user <Username>
```

更改用于发行版登录的默认用户。 用户必须已经存在于发行版中才能成为默认用户。

例如：`ubuntu config --default-user johndoe` 会将 Ubuntu 发行版的默认用户更改为“johndoe”用户。

## 关闭

```powershell
wsl --shutdown
```

立即终止所有正在运行的发行版和 WSL 2 轻量级实用工具虚拟机。 在需要重启 WSL 2 虚拟机环境的情形下，例如[更改内存使用限制](https://learn.microsoft.com/zh-cn/windows/wsl/vhd-size)或更改 [.wslconfig 文件](https://learn.microsoft.com/zh-cn/windows/wsl/manage#)，可能必须使用此命令。

## Terminate

```powershell
wsl --terminate <Distribution Name>
```

若要终止指定的发行版或阻止其运行，请将 `<Distribution Name>` 替换为目标发行版的名称。

## 将发行版导出到 TAR 文件

PowerShell复制

```powershell
wsl --export <Distribution Name> <FileName>
```

将分发版导出到 tar 文件。 在标准输出中，文件名可以是 -。

## 导入新发行版

PowerShell复制

```powershell
wsl --import <Distribution Name> <InstallLocation> <FileName>
```

导入指定的 tar 文件作为新的分发版。 在标准输入中，文件名可以是 -。 `--version` 选项还可与此命令一起使用，用于指定导入的发行版将在 WSL 1 还是 WSL 2 上运行。

## 注销或卸载 Linux 发行版

尽管可以通过 Microsoft Store 安装 Linux 发行版，但无法通过 Store 将其卸载。

注销并卸载 WSL 发行版：

PowerShell复制

```powershell
wsl --unregister <DistributionName>
```

如果将 `<DistributionName>` 替换为目标 Linux 发行版的名称，则将从 WSL 取消注册该发行版，以便可以重新安装或清理它。 **警告：**取消注册后，与该分发版关联的所有数据、设置和软件将永久丢失。 从 Store 重新安装会安装分发版的干净副本。 例如：`wsl --unregister Ubuntu` 将从可用于 WSL 的发行版中删除 Ubuntu。 运行 `wsl --list` 将会显示它不再列出。

## WSL与Windows互传文件



## WSL报错Error code: Wsl/Service/0x8007273d

以管理身份打开cmd/powershell，运行以下命令：

```powershell
netsh winsock reset
```

