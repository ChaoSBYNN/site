---
title: Git 查看从某一日期开始变更的文件
date: 2024-07-12 08:32:39
tags: 
    - "Git"
categories:
    - "Program"
    - "Git"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/git.webp"
excerpt: "Git 日志查询"
---
要查看从某一日期开始变更的文件，可以使用 `git` 命令来列出这些变更。以下是一些常用的 `git` 命令和方法，用于查看自特定日期以来的文件变更记录。

## 1. **列出从某一日期开始的所有提交记录**

首先，你可以使用 `git log` 命令来列出从某一日期开始的所有提交记录。这些提交记录会包含文件的变更信息。

```bash
git log --since="YYYY-MM-DD" --pretty=format:"%h %s" --name-only
```

- `--since="YYYY-MM-DD"`: 设定起始日期。
- `--pretty=format:"%h %s"`: 以提交的简短哈希和提交信息的形式输出日志。
- `--name-only`: 仅显示文件名，不显示详细的内容变更。

### 示例

```bash
git log --since="2024-01-01" --pretty=format:"%h %s" --name-only
```

这会列出从 2024 年 1 月 1 日以来的所有提交记录，并显示涉及的文件。

## 2. **列出从某一日期开始的文件变更记录**

如果你想要查看从某一日期开始每个文件的变更情况，可以使用以下命令：

```bash
git log --since="YYYY-MM-DD" --name-only --pretty=format:""
```

- `--pretty=format:""`: 只显示文件名，不显示提交信息。

### 示例

```bash
git log --since="2024-01-01" --name-only --pretty=format:""
```

## 3. **查看从某一日期开始的每个文件的详细变更信息**

如果你想要查看从某一日期开始的每个文件的详细变更信息，可以使用以下命令：

```bash
git log --since="YYYY-MM-DD" --stat
```

- `--stat`: 显示每次提交中修改的文件以及更改的行数。

### 示例

```bash
git log --since="2024-01-01" --stat
```

## 4. **查看从某一日期开始的每个文件的变更内容**

如果你需要查看从某一日期开始的每个文件的实际变更内容，可以使用以下命令：

```bash
git log --since="YYYY-MM-DD" -p
```

- `-p`: 显示每个提交的差异。

### 示例

```bash
git log --since="2024-01-01" -p
```

## 5. **查看从某一日期开始的所有变更文件及其详细信息**

结合 `grep` 命令，可以筛选出从某一日期开始的文件并查看详细的变更信息：

```bash
git log --since="YYYY-MM-DD" --name-only | grep -v '^$' | xargs -I {} git log --since="YYYY-MM-DD" --oneline -- {}
```

- `grep -v '^$'`: 去除空行。
- `xargs -I {} git log --since="YYYY-MM-DD" --oneline -- {}`: 对每个文件显示从该日期开始的提交记录。

### 示例

```bash
git log --since="2024-01-01" --name-only | grep -v '^$' | xargs -I {} git log --since="2024-01-01" --oneline -- {}
```

## 6. **列出某一日期之后每个文件的所有变更记录**

你可以使用 `git diff` 命令来比较某一日期之前和之后的所有变更记录：

```bash
git diff --name-only $(git rev-list -n 1 --before="YYYY-MM-DD" master) HEAD
```

- `$(git rev-list -n 1 --before="YYYY-MM-DD" master)`: 获取指定日期之前的最后一次提交的哈希值。
- `HEAD`: 当前最新提交的哈希值。

### 示例

```bash
git diff --name-only $(git rev-list -n 1 --before="2024-01-01" master) HEAD
```

## 7. **查看从某一日期开始的所有文件变更记录（包含每个提交的文件变更内容）**

你还可以结合 `grep` 和 `awk` 命令来提取从某一日期开始的文件变更记录及其详细内容：

```bash
git log --since="YYYY-MM-DD" --pretty=format:"%h %s" --name-only | awk 'NF{print $0}'
```

### 示例

```bash
git log --since="2024-01-01" --pretty=format:"%h %s" --name-only | awk 'NF{print $0}'
```

## 参考资料

- [Git log Documentation](https://git-scm.com/docs/git-log)
- [Git Diff Documentation](https://git-scm.com/docs/git-diff)
- [Git Rev-list Documentation](https://git-scm.com/docs/git-rev-list)
- [Git Log --stat Option](https://git-scm.com/docs/git-log#git-log--stat)

通过这些命令，你可以根据需求查看从特定日期开始的文件变更记录，并对其进行深入分析。如果有其他具体需求，可以进一步定制这些命令来满足你的要求。