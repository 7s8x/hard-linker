<img src="Icon-iOS-Default-1024x1024@1x.png" width="200px" />

# Hard Linker

[简体中文](#简体中文) · [English](#english)

> Fork note: This repository is a fork of [joshferrara/hard-linker](https://github.com/joshferrara/hard-linker) at `v1.0.9`. The changes on `main` add local job history, verification, and scoped link search; they are also proposed upstream in [PR #1](https://github.com/joshferrara/hard-linker/pull/1), which has not been merged yet.
>
> Fork 说明：本仓库基于 [joshferrara/hard-linker](https://github.com/joshferrara/hard-linker) 的 `v1.0.9`。`main` 上的改动增加了本地作业历史、核验和范围内的链接查询，并已通过 [PR #1](https://github.com/joshferrara/hard-linker/pull/1) 提交给上游，目前尚未合并。

---

## 简体中文

Hard Linker 是一个原生 macOS 工具，使用 `cp -al` 为选中的文件或文件夹创建硬链接副本。

### 功能

- 拖入文件或文件夹，或使用系统文件选择器选择
- 将多个来源链接到同一个目标文件夹
- 预检重复目标名、已存在的目标、缺失文件以及递归选择目标目录等问题
- 原生的进度与状态反馈
- 本地保存已完成的硬链接作业，可核验来源与目标文件
- 在已记录的位置或手动添加的文件夹中，按文件或文件夹查询已有硬链接
- 当前应用版本号显示在标签页旁，读取自应用包的 `Info.plist`

### 什么是硬链接

硬链接是指向磁盘上同一份文件数据的目录项。额外的硬链接不会复制文件内容，因此可用来节省空间。

在 macOS 上，目录本身不能被硬链接。选择文件夹时，Hard Linker 会创建目标目录结构，并通过 `cp -al` 为其中的文件创建硬链接。硬链接必须在同一个卷内创建，跨卷会失败。

### 系统要求

- macOS 13.0 或更高版本
- 对所选来源有读取权限，对目标文件夹有写入权限

### 安装

1. 从本仓库的 [Releases](https://github.com/7s8x/hard-linker/releases) 页面下载 `Hard-Linker-1.1.0.zip`。
2. 解压并将 `Hard-Linker.app` 移到 `/Applications`。
3. 首次打开时若 Gatekeeper 提示，右键选择「打开」。

本 release 的 App 使用 ad hoc 签名、未经公证，且未配置自动更新源；请勿依赖它自动升级到上游版本。

### 使用

1. 从 Finder 拖入一个或多个文件或文件夹到 **来源**，或点击该区域选择。
2. 拖入一个文件夹到 **目标目录**，或点击该区域选择。
3. 点击 **创建硬链接**。

每个来源会以其原文件名或文件夹名链接到目标目录。如果目标路径已存在，应用会在执行 `cp` 前停止。

**历史记录**页会把本机成功的作业记录到 `~/Library/Application Support/Hard Linker/history.json`。点击 **核验** 可按卷和 inode 比对来源与目标文件；**忘记记录** 只移除历史条目，不删除任何文件。在 **查询**页，把文件或文件夹从 Finder 拖到该页任意位置，或点击 **选择文件或文件夹**。查询范围包括已记录的位置和你手动添加的文件夹。结果会显示找到的路径数以及文件的硬链接总数。早期作业不会导入，应用也不会自动扫描整个卷。

### 开发

构建并运行 Swift 包：

```bash
swift build
swift run HardLinkCreator
```

运行测试：

```bash
swift test
```

打包签名版本（需要 Developer ID 签名身份、公证配置和 Sparkle 私钥）：

```bash
./build-and-release.sh -v 1.1.0 -b 1
```

### 项目结构

```text
Hardlinker/
├── .github/workflows/release.yml
├── AppIcon.icns
├── AppIcon.iconset/
├── Package.swift
├── README.md
├── Sources/
│   └── HardLinkCreator/
│       ├── Entitlements.plist
│       ├── HardLinkCreator.swift
│       ├── LinkHistory.swift
│       ├── LinkInspector.swift
│       ├── Localization.swift
│       └── ManagementViews.swift
├── Tests/
│   └── HardLinkCreatorTests/
│       ├── HardLinkPlanTests.swift
│       └── LinkInspectorTests.swift
├── appcast.xml
└── build-and-release.sh
```

### 来源与许可

基于 [joshferrara/hard-linker](https://github.com/joshferrara/hard-linker) 的 `v1.0.9`（提交 `c358cbc7a8bdc4f0a4d6286cd9dde59cd6d43645`）。Copyright © 2025 Josh Ferrara. All rights reserved.

上游项目未声明开源许可证（仅标注 "All rights reserved"）。本 fork 为衍生版本，再分发前请自行确认授权情况。

---

## English

Hard Linker is a native macOS utility for creating hard-linked copies of selected files and folders with `cp -al`.

### Features

- Drag and drop files or folders, or choose them with the native file picker
- Link multiple sources into one destination folder
- Preflight validation for duplicate target names, existing targets, missing files, and recursive destination choices
- Native progress and status feedback
- Local history of completed hard-link jobs with source/target verification
- Search for existing hard links by file or folder within recorded locations or selected search folders
- The current app version is shown beside the tabs and is read from the app bundle's `Info.plist`

### What are hard links?

Hard links are directory entries that point to the same file data on disk. They are useful for space-efficient copies because additional hard links do not duplicate file contents.

On macOS, directories themselves are not hard-linked. When you choose a folder, Hard Linker creates the destination directory structure and hard-links the files inside it through `cp -al`.

Hard links must be created on the same volume. Cross-volume attempts will fail.

### Requirements

- macOS 13.0 or later
- Read access to selected sources and write access to the selected destination

### Installation

1. Download `Hard-Linker-1.1.0.zip` from this repository's [Releases](https://github.com/7s8x/hard-linker/releases) page.
2. Unzip it and move `Hard-Linker.app` to `/Applications`.
3. Right-click and choose Open the first time if macOS Gatekeeper prompts.

This release is ad hoc signed, not notarized, and has no automatic update feed configured. Do not rely on it to auto-update to an upstream version.

### Usage

1. Drag one or more source files or folders from Finder into **Sources**, or click the area to choose them.
2. Drag a destination folder into **Destination**, or click the area to choose it.
3. Click Create Hard Links.

Each selected source is linked into the destination using its original file or folder name. If that target path already exists, the app stops before running `cp`.

The **History** tab records successful jobs on this Mac in `~/Library/Application Support/Hard Linker/history.json`. Use **Verify** to compare source and target files by volume and inode; **Forget** removes only the history entry, not any files. On the **Search** tab, drag a file or folder from Finder anywhere in the tab, or click **Choose file or folder**. Search scans recorded locations plus any folders you add. Results show how many paths were found and the file's total hard-link count. Earlier jobs are not imported, and the app does not automatically scan entire volumes.

### Development

Build and run the Swift package:

```bash
swift build
swift run HardLinkCreator
```

Run tests:

```bash
swift test
```

Create a signed release bundle (requires a Developer ID signing identity, notarization profile, and Sparkle private key):

```bash
./build-and-release.sh -v 1.1.0 -b 1
```

### Project structure

```text
Hardlinker/
├── .github/workflows/release.yml
├── AppIcon.icns
├── AppIcon.iconset/
├── Package.swift
├── README.md
├── Sources/
│   └── HardLinkCreator/
│       ├── Entitlements.plist
│       ├── HardLinkCreator.swift
│       ├── LinkHistory.swift
│       ├── LinkInspector.swift
│       ├── Localization.swift
│       └── ManagementViews.swift
├── Tests/
│   └── HardLinkCreatorTests/
│       ├── HardLinkPlanTests.swift
│       └── LinkInspectorTests.swift
├── appcast.xml
└── build-and-release.sh
```

### Attribution and license

Based on [joshferrara/hard-linker](https://github.com/joshferrara/hard-linker) `v1.0.9` (commit `c358cbc7a8bdc4f0a4d6286cd9dde59cd6d43645`). Copyright © 2025 Josh Ferrara. All rights reserved.

The upstream project declares no open-source license (only "All rights reserved"). This fork is a derivative work; confirm the licensing situation before redistributing.
