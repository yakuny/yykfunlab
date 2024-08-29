+++
author = "YYK"
title = "Git Commit Message规范 - Angular"
date = "2021-09-02"
categories = [
    "技术",
]
tags = [
    "git",
]
+++

一直都感觉项目里面的 Commit Message 十分混乱，大家都很随意的提交，随便几个字就敷衍过去了。在查看提交记录的时候，都不知道别人干了什么，还要去代码里看看，然而代码里也没注释，我经常看的一脸懵逼。直到有一回阅读别的项目的代码，看到他们的 Commit Message 整齐划一，一眼就能理解变更历史。经过搜索，才知道他们采用的是 Angular 规范。

Commit Message 有很多规范，如 jQuery、Angular 等。使用规范化的 Commit Message 有很多好处:
> 1. 清晰地知道每个 commit 的变更内容
> 2. 方便通过 Commit Message 查找特定版本
> 3. 可以生成 Change Log
> 4. 可以依据特定 commit类型 触发构建或者发布流程
> 5. 根据 commit 类型，确定相应的版本号

其中 Angular规范 是目前使用最广泛、最多的，这里只做 Commit Message 格式规范的整理。

主要参考：[https://github.com/angular/angular/blob/master/CONTRIBUTING.md](https://github.com/angular/angular/blob/master/CONTRIBUTING.md)

## Commit Message 格式
每一条 commit message 包含三部分，**header, body **和** footer**，格式如下：
```basic
<header>
<BLANK LINE>  //空行
<body>
<BLANK LINE>  //空行
<footer>
```

- **header:** 必填项，必须符合 **Commit Message Header **格式。
- **body: **必填项(除了“**docs**”以外)，必须至少有20个字符，必须符合 **Commit Message Body** 格式。
- **footer: **选填项，必须符合 **Commit Message Footer** 格式。

（任何一行 commit message 都不能超过100个字符）

### Commit Message Header
```basic
<type>(<scope>): <short summary>
  │       │             │
  │       │             └─⫸ Summary in present tense. Not capitalized. No period at the end.
  │       │
  │       └─⫸ Commit Scope: animations|bazel|benchpress|common|compiler|compiler-cli|core|
  │                          elements|forms|http|language-service|localize|platform-browser|
  │                          platform-browser-dynamic|platform-server|router|service-worker|
  │                          upgrade|zone.js|packaging|changelog|docs-infra|migrations|ngcc|ve
  │
  └─⫸ Commit Type: build|ci|docs|feat|fix|perf|refactor|test
```
其中 <type> 和 <summary> 为必填项，<scope> 为选填项。

**type** 须为以下八种之一：

- **build**: 构建系统或外部依赖项的变更 (example scopes: gulp, broccoli, npm)
- **ci**: 配置项的变更 (example scopes: Circle, BrowserStack, SauceLabs) (Configurtion Items)
- **docs**: 文档变更 (documentation)
- **feat**: 新功能 (feature)
- **fix**: bug修复
- **perf**: 性能提升的变更 (performance)
- **refactor**: 代码重构，不涉及bug修复和新功能添加
- **test**: 新增测试样例或者更改已有测试样例
- (**chore**: 其余变更，如：文档修改，构建流程)

(不知道为什么chore没有在参考文档里)

**scope **用于说明 commit 影响范围的，比如数据称、控制层、视图层等，名词。

**summary **简单明了等描述改动点，描述约束如下：

- 使用祈使句、现在时，使用 "change"，不使用 "changed" 或 "changes"
- 首字母小写
- 结尾不加句号 (.)

### Commit Message Body
与 **summary **使用的语句形式一样，祈使句、现在时，用于解释为什么要做这样的改动，可以与上一个版本的代码做对比，来说明变化的影响。

### Commit Message Footer
用来描述重大不兼容的改变或者指引到相应的 issues 列表等，格式如下：
```basic
BREAKING CHANGE: <breaking change summary>
<BLANK LINE> //空行
<breaking change description + migration instructions>
<BLANK LINE> //空行
<BLANK LINE> //空行
Fixes #<issue number>
```

## 例子
官方的例子应该是最权威的了吧，github commit 记录: [https://github.com/angular/angular/commits/master](https://github.com/angular/angular/commits/master) ，这里简单截取几条。
```basic
fix(migrations): migration failed finding tsconfig file (#43343)
refactor(common): removed TODO no longer considered necessary (#43378)
docs: remove Angular 9 from support table (#43350)
```

## 总结一下
这篇博文基本没我啥事儿，简单的翻译了下参考链接的内容，主要用于整理，方便之后我再回来看 Commit Message 规范格式。其实我在使用中也只是简单的规范一下自己的 commit message。



