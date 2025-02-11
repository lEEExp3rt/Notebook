---
tags:
  - Git
icon: LiGithub
---

# Github 高级功能

> [!abstract] 本文摘要
> Github高级功能收录

> [!info] 社区
> [GitHub中文社区](https://www.github-zh.com/)

## Actions

### Introduction

总的来说，Github Actions就是Github自动化流水线

使用GitHub Actions几乎可以实现自动化软件开发流程中的方方面面，包括自动化测试、CI/CD持续部署、自动化代码审查、管理问题和拉取请求等

Github Actions的工作流配置以代码的形式保存在Git仓库中，可以很方便的在团队之间共享和重用

### Basic

#### Workflow

工作流是一个可配置的自动化流程，它将运行一个或多个Job

流程配置文件使用YAML格式，保存在仓库的`.github/workworks/`中

一个工作流中可以有若干Job，每个Job可以有多个Step

> [!note] 注意
> 不同Job默认是并行运行的互相不依赖的且在不同的虚拟环境中执行，而Step则是顺序执行的

#### Event

事件/触发器，用于触发Action

只有所选事件发生时，才会触发执行对应的Action

### Example

```yml
name: SemVer Auto Tag and Release

on:
  push:
    branches:
      - main

jobs:
  auto-semver:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Set up Git
      run: |
        git config user.name "${{ github.actor }}"
        git config user.email "${{ github.actor }}@users.noreply.github.com"

    - name: Get latest tag
      id: get-latest-tag
      run: |
        latest_tag=$(git describe --tags --abbrev=0 2>/dev/null || echo "0.0.0")
        echo "latest_tag=$latest_tag" >> $GITHUB_ENV

    - name: Parse commit messages
      id: parse-commits
      run: |
        # 获取最新的 commit 消息
        commits=$(git log --format=%B -n 5)
        echo "Commits: $commits"

        if echo "$commits" | grep -q "BREAKING CHANGE"; then
          echo "update_type=major" >> $GITHUB_ENV
        elif echo "$commits" | grep -q "feat:"; then
          echo "update_type=minor" >> $GITHUB_ENV
        else
          echo "update_type=patch" >> $GITHUB_ENV
        fi

    - name: Calculate next version
      id: calc-version
      run: |
        IFS='.' read -r -a parts <<< "$latest_tag"
        major=${parts[0]}
        minor=${parts[1]}
        patch=${parts[2]}

        case $update_type in
          major)
            major=$((major + 1))
            minor=0
            patch=0
            ;;
          minor)
            minor=$((minor + 1))
            patch=0
            ;;
          patch)
            patch=$((patch + 1))
            ;;
        esac

        new_version="$major.$minor.$patch"
        echo "new_version=$new_version" >> $GITHUB_ENV

    - name: Create new tag
      run: |
        git tag ${{ env.new_version }}
        git push origin ${{ env.new_version }}

    - name: Create GitHub Release
      uses: actions/create-release@v1
      with:
        tag_name: ${{ env.new_version }}
        release_name: Release ${{ env.new_version }}
        body: |
          ## Changes
          - Automated release for version ${{ env.new_version }}.

```

## Packages

## Codespaces
