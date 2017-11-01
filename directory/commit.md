# 代码贡献规范
有任何疑问，欢迎提交 issue， 或者直接修改提交 PR!

## 提交 issue
* 请确定 issue 的类型。
* 请避免提交重复的 issue，在提交之前搜索现有的 issue。
* 在标签(分类参考标签分类), 标题 或者内容中体现明确的意图。

随后 负责人 会确认 issue 意图，更新合适的标签，关联 milestone，指派开发者。

标签可分为两类，type 和 scope

* type: issue 的类型，如 `feature`, `bug`, `documentation`, `performance`, `support` ...
* scope: 修改文件的范围，如 `core: xx`，`plugin: xx`，`deps: xx`

### 常用标签说明
* `support`: issue 提出的问题需要开发者协作排查，咨询，调试等等日常技术支持。
* `bug`: 一旦发现可能是 bug 的问题，请打上 `bug`，然后等待确认，一旦确认是 bug，此 issue 会被再打上 `confirmed`。
	* 此时 issue 会被非常高的优先级进行处理。
	* 如果此 bug 是正在影响线上应用正常运行，会再打上 `critical`，代表是最高优先级，需要马上立刻处理！
	* bug 会在最低需要修复的版本进行修复，如是在 `0.9.x` 要修复的，而当前最新版本是 `1.1.x`， 那么此 issue 还会被打上 `0.9`，`0.10`，`1.0`，`1.1`，代表需要修复到这些版本。
* `core: xx`: 代表 issue 跟 core 内核相关，如 core: antx 代表跟 antx 配置相关。
* `plugin: xx`: 代表 issue 跟插件相关，如 deps: session 代表跟 session 插件相关。
* `deps: xx`: 代表 issue 跟 dependencies 模块相关，如 deps: `project-cors` 代表跟 `project-cors` 模块相关。
* `chore: documentation`: 代表发现了文档相关问题，需要修复文档说明。
* `cbd`: 代表跟服务器部署相关

## 编写文档
所有功能点必须提交配套文档，文档须满足以下要求

* 必须说清楚问题的几个方面：what（是什么），why（为什么），how（怎么做），可根据问题的特性有所侧重。
* how 部分必须包含详尽完整的操作步骤，必要时附上 足够简单，可运行 的范例代码， 所有范例代码放在 `project/examples` 库中。
* 提供必要的链接，如申请流程，术语解释和参考文档等。
同步修改中英文文档，或者在 PR 里面说明。

##  提交代码
### fork 项目
如果对项目感兴趣并希望贡献代码，先fork项目

```sh
$ git remote add fork-name fork-url
$ git pull
```

### 提交 Pull Request(也叫Merge Request)
如果你有仓库的开发者权限，而且希望贡献代码，那么你可以创建分支修改代码提交 PR，开发团队 会 review 代码合并到主干。

```sh
# 先创建开发分支开发，分支名应该有含义，避免使用 update、tmp 之类的
$ git checkout -b branch-name
# 开发完成后跑下测试是否通过，必要时需要新增或修改测试用例
# 编译通过后，提交代码，message 见下面的规范
$ mvn package
$ git add -A # git add -u 删除文件
$ git commit -m "fix(role): role.use must xxx"
$ git push origin branch-name
```

提交后就可以创建 Pull Request 了。

由于谁也无法保证过了多久之后还记得多少，为了后期回溯历史的方便，请在提交 MR 时确保提供了以下信息。

需求点（一般关联 issue 或者注释都算）
升级原因（不同于 issue，可以简要描述下为什么要处理）
框架测试点（可以关联到测试文件，不用详细描述，关键点即可）
关注点（针对用户而言，可以没有，一般是不兼容更新等，需要额外提示）

### 代码风格

你的代码风格必须使用与项目一致的格式。

### Commit 提交规范

根据 [angular 规范](https://github.com/angular/angular.js/blob/master/CONTRIBUTING.md#commit-message-format)提交 commit， 这样 history 看起来更加清晰，还可以自动生成 changelog。

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

（1）type

提交 commit 的类型，包括以下几种

* feat: 新功能
* fix: 修复问题
* docs: 修改文档
* style: 修改代码格式，不影响代码逻辑
* refactor: 重构代码，理论上不影响现有功能
* perf: 提升性能
* test: 增加修改测试用例
* chore: 修改工具相关（包括但不限于文档、代码生成等）
* deps: 升级依赖

（2）scope

修改文件的范围（包括但不限于 doc, middleware, core, config, plugin）

（3）subject

用一句话清楚的描述这次提交做了什么

（4）body

补充 subject，适当增加原因、目的等相关因素，也可不写。

（5）footer

* 当有非兼容修改(Breaking Change)时必须在这里描述清楚
* 关联相关 issue，如 Closes #1, Closes #2, #3
* 如果功能点有新增或修改的，还需要关联文档 doc

示例

	fix($compile): [BREAKING_CHANGE] couple of unit tests for IE9
	
	Older IEs serialize html uppercased, but IE9 does not...
	Would be better to expect case insensitive, unfortunately jasmine does
	not allow to user regexps for throw expectations.
	Closes #392
	
	BREAKING CHANGE:
	  Breaks foo.bar api, foo.baz should be used instead

查看具体[文档](../assets/doc/GitCommitMessageConventions.html)
## 发布管理
基于 [semver](http://semver.org/lang/zh-CN/) 语义化版本号进行发布。

### 分支策略

`master` 分支为当前稳定发布的版本，`next` 分支为下一个开发中的大版本。

* 只维护两个版本，除非有安全问题，否则修复只会 patch 到 `master` 和 `next` 分支，其他更新推动上层框架升级到稳定大版本的最新版本。
* 所有 API 的废弃都需要在当前的稳定版本上 `deprecate` 提示，并保证在当前的稳定版本上一直兼容到新版本的发布。
* `master` 分支不设置 publish tag，上层框架基于 semver 依赖稳定版本。
* `next` 分支设置 tag 为 `next`，上层框架可以通过 `project@next` 引用开发中的版本进行测试。
* projecy 持续维护的版本以 Milestone 为准，只要是开着的版本都会进行修复。