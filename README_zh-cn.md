# GitHub MCP 服务器

[English](README.md) | [简体中文](README_zh-cn.md)

基于 GitHub API 的 MCP 服务器，支持文件操作、仓库管理、代码搜索等常用功能。

## Mission Squad 修改说明

- GitHub PAT 现在支持请求级别动态传入，不再强制依赖环境变量（但环境变量仍可用作后备方案）
- 服务名称：mcp-github

## 主要功能

- **自动创建分支**：执行文件操作时如果目标分支不存在，会自动创建新分支
- **完整错误处理**：所有常见错误都有清晰的提示信息
- **保持 Git 历史**：所有操作都遵循 Git 规范，不会丢失历史记录
- **批量文件操作**：支持单文件和多文件批量处理
- **高级搜索功能**：支持代码、Issue/PR、用户等多种维度搜索

## 可用工具列表

### 1. create_or_update_file

创建或更新单个文件

**参数说明：**
- `owner`（string）：仓库所有者（用户名或组织名）
- `repo`（string）：仓库名称
- `path`（string）：文件路径
- `content`（string）：文件内容
- `message`（string）：提交信息
- `branch`（string）：目标分支
- `sha`（可选，string）：要更新的文件 SHA（更新时使用）

**返回结果：**
文件内容和提交详情

### 2. push_files

批量推送多个文件（单次提交）

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `branch`（string）：目标分支
- `files`（array）：文件列表，每个文件需要 path 和 content
- `message`（string）：提交信息

**返回结果：**
更新后的分支信息

### 3. search_repositories

搜索 GitHub 仓库

**参数说明：**
- `query`（string）：搜索关键词
- `page`（可选，number）：页码，用于分页
- `perPage`（可选，number）：每页数量，最多 100

**返回结果：**
仓库搜索结果

### 4. create_repository

创建新仓库

**参数说明：**
- `name`（string）：仓库名称
- `description`（可选，string）：仓库描述
- `private`（可选，boolean）：是否设为私有
- `autoInit`（可选，boolean）：是否自动创建 README

**返回结果：**
新建仓库详情

### 5. get_file_contents

获取文件或目录内容

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `path`（string）：文件或目录路径
- `branch`（可选，string）：指定分支，默认使用默认分支

**返回结果：**
文件内容或目录列表

### 6. create_issue

发起新 Issue

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `title`（string）：Issue 标题
- `body`（可选，string）：Issue 详细描述
- `assignees`（可选，string[]）：指派给哪些用户
- `labels`（可选，string[]）：添加的标签
- `milestone`（可选，number）：关联的里程碑编号

**返回结果：**
Issue 详情

### 7. create_pull_request

发起 Pull Request

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `title`（string）：PR 标题
- `body`（可选，string）：PR 描述
- `head`（string）：来源分支（改动所在分支）
- `base`（string）：目标分支（要合并到的分支，通常为主分支）
- `draft`（可选，boolean）：是否创建为草稿
- `maintainer_can_modify`（可选，boolean）：是否允许维护者修改

**返回结果：**
PR 详情

### 8. fork_repository

fork仓库

**参数说明：**
- `owner`（string）：原仓库所有者
- `repo`（string）：原仓库名称
- `organization`（可选，string）：要fork到的组织

**返回结果：**
fork仓库信息

### 9. create_branch

创建新分支

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `branch`（string）：新分支名称
- `from_branch`（可选，string）：源分支，默认使用仓库默认分支

**返回结果：**
新建分支详情

### 10. list_issues

列出仓库 Issues

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `state`（可选，string）：筛选状态（open、closed、all）
- `labels`（可选，string[]）：按标签筛选
- `sort`（可选，string）：排序方式（created、updated、comments）
- `direction`（可选，string）：排序方向（asc、desc）
- `since`（可选，string）：时间筛选（ISO 8601 格式）
- `page`（可选，number）：页码
- `per_page`（可选，number）：每页数量

**返回结果：**
Issue 列表

### 11. update_issue

更新已有 Issue

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `issue_number`（number）：要更新的 Issue 编号
- `title`（可选，string）：新标题
- `body`（可选，string）：新描述
- `state`（可选，string）：新状态（open 或 closed）
- `labels`（可选，string[]）：新标签列表
- `assignees`（可选，string[]）：新指派人员
- `milestone`（可选，number）：新里程碑编号

**返回结果：**
更新后的 Issue 详情

### 12. add_issue_comment

为 Issue 添加评论

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `issue_number`（number）：Issue 编号
- `body`（string）：评论内容

**返回结果：**
新建评论详情

### 13. search_code

搜索代码

**参数说明：**
- `q`（string）：搜索语句（使用 GitHub 代码搜索语法）
- `sort`（可选，string）：排序字段（仅支持 indexed）
- `order`（可选，string）：排序顺序（asc 或 desc）
- `per_page`（可选，number）：每页数量，最多 100
- `page`（可选，number）：页码

**返回结果：**
代码搜索结果

### 14. search_issues

搜索 Issues 和 PRs

**参数说明：**
- `q`（string）：搜索语句（使用 GitHub Issue 搜索语法）
- `sort`（可选，string）：排序字段（comments、reactions、created 等）
- `order`（可选，string）：排序顺序（asc 或 desc）
- `per_page`（可选，number）：每页数量，最多 100
- `page`（可选，number）：页码

**返回结果：**
Issue 和 PR 搜索结果

### 15. search_users

搜索用户

**参数说明：**
- `q`（string）：搜索语句（使用 GitHub 用户搜索语法）
- `sort`（可选，string）：排序字段（followers、repositories、joined）
- `order`（可选，string）：排序顺序（asc 或 desc）
- `per_page`（可选，number）：每页数量，最多 100
- `page`（可选，number）：页码

**返回结果：**
用户搜索结果

### 16. list_commits

获取分支提交记录

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `page`（可选，number）：页码
- `per_page`（可选，number）：每页数量
- `sha`（可选，string）：分支名称

**返回结果：**
提交记录列表

### 17. get_issue

获取 Issue 详情

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `issue_number`（number）：Issue 编号

**返回结果：**
Issue 详细信息

### 18. get_pull_request

获取 PR 详情

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `pull_number`（number）：PR 编号

**返回结果：**
PR 详细信息，包括差异和审查状态

### 19. list_pull_requests

列出仓库 PRs

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `state`（可选，string）：筛选状态（open、closed、all）
- `head`（可选，string）：按来源分支筛选
- `base`（可选，string）：按目标分支筛选
- `sort`（可选，string）：排序方式（created、updated、popularity、long-running）
- `direction`（可选，string）：排序方向（asc 或 desc）
- `per_page`（可选，number）：每页数量，最多 100
- `page`（可选，number）：页码

**返回结果：**
PR 列表

### 20. create_pull_request_review

为 PR 提交审查

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `pull_number`（number）：PR 编号
- `body`（string）：审查意见
- `event`（string）：审查操作（APPROVE、REQUEST_CHANGES、COMMENT）
- `commit_id`（可选，string）：要审查的提交 SHA
- `comments`（可选，array）：行级评论列表，每个需要 path（string）、position（number）、body（string）

**返回结果：**
审查详情

### 21. merge_pull_request

合并 PR

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `pull_number`（number）：PR 编号
- `commit_title`（可选，string）：PR标题
- `commit_message`（可选，string）：补充信息
- `merge_method`（可选，string）：合并方式（merge、squash、rebase）

**返回结果：**
合并结果

### 22. get_pull_request_files

获取 PR 中的文件变更

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `pull_number`（number）：PR 编号

**返回结果：**
变更文件列表，包括diff和status

### 23. get_pull_request_status

获取 PR 状态检查结果

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `pull_number`（number）：PR 编号

**返回结果：**
结果汇总及各检查项详情

### 24. update_pull_request_branch

更新 PR 分支（同步基础分支的最新改动）

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `pull_number`（number）：PR 编号
- `expected_head_sha`（可选，string）：Head 指向的提交哈希（防止并发冲突）
**返回结果：**
更新成功信息

### 25. get_pull_request_comments

获取 PR 的审查评论

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `pull_number`（number）：PR 编号

**返回结果：**
审查评论列表

### 26. get_pull_request_reviews

获取 PR 的审查记录

**参数说明：**
- `owner`（string）：仓库所有者
- `repo`（string）：仓库名称
- `pull_number`（number）：PR 编号

**返回结果：**
审查记录列表，包括状态和审查者信息

## 搜索语法示例

### 代码搜索
- `language:javascript`：按语言搜索
- `repo:owner/name`：限定仓库
- `path:app/src`：限定路径
- `extension:js`：按扩展名搜索
- 示例：`q: "import express" language:typescript path:src/`

### Issue 搜索
- `is:issue` 或 `is:pr`：按类型筛选
- `is:open` 或 `is:closed`：按状态筛选
- `label:bug`：按标签搜索
- `author:username`：按作者搜索
- 示例：`q: "memory leak" is:issue is:open label:bug`

### 用户搜索
- `type:user` 或 `type:org`：按账户类型筛选
- `followers:>1000`：按关注者数量筛选
- `location:London`：按地区搜索
- 示例：`q: "fullstack developer" location:London followers:>100`

更多搜索语法请查看 [GitHub 官方文档](https://docs.github.com/zh/search-github/searching-on-github)
或参阅 [GitHub 官方文档(英文)](https://docs.github.com/en/search-github/searching-on-github)


## 配置和使用

### 准备访问令牌

1. 访问 [Personal access tokens](https://github.com/settings/tokens)
2. 选择仓库访问权限
3. 创建令牌时勾选 `repo` 权限（操作私有仓库需要）
   - 如果只操作公开仓库，勾选 `public_repo` 即可
4. 保存生成的令牌

### 在 Claude Desktop 中使用

#### Docker 方式

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN",
        "mcp/github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "PAT，修改为你的访问令牌"
      }
    }
  }
}
```

#### NPX 方式

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "修改为你的PAT"
      }
    }
  }
}
```

## 构建

```bash
docker build -t mcp/github -f src/github/Dockerfile .
```

## 许可证

基于 MIT 许可证开源，详见项目中的 [LICENSE](LICENSE) 文件