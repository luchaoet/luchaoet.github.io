---
title: git提交日志规范
date: 2017-12-05 20:22:24
tags: ['git', 'Commitizen']
summary: git提交日志规范,让开发打消杀人的念头
---
为什么要对 Git 提交日志（以下简称 log）的格式进行规范约束？

1. __最重要的原因__ 自然是：便于人类程序员对提交历史进行追溯，了解发生了什么情况。因此像「update」甚至于「u」这样的 log，都是不合格的写法，想必诸如此类的 log 已经被大家咒骂过一遍遍。
2. 另外，一旦约束了提交日志，意味着我们将慎重地进行每一次提交，不能再一股脑儿地把各种各样的改动都放到一个提交里面，这样一来，整个代码改动的历史也将更加清晰。
3. 想得更远一点，格式化的 log 才可能用于自动化输出 changelog。

对 Git 提交日志进行格式约束是否带来较高的执行成本？

1. 所谓仓廪实而知礼节，随着大型共建项目 / 开源项目的增多，必然要用更专业化的态度去面对。规范化的 Git log 正是其中一环。
2. 最后，如果实在无法完美遵循日志规范，最最重要的原则是：__至少要保证在整个项目中 log 格式的一致性__！不要做一个朝秦暮楚的人。

### type

提交类型 type 用来描述一次提交行为的改动方向。

type 的可选值如下。注意：Git log 的 type 和 changelog 的 type 存在紧密联系；然而它们两者之间并非一一对应，比如在 changelog 中一般不会指出文档 docs 或测试用例 test 等方面发生的变化。

* feat: 新增功能
* fix: 修复 bug
* docs: 文档相关的改动
* style: 对代码的格式化改动，代码逻辑并未产生任何变化
* test: 新增或修改测试用例
* refactor: 重构代码或其他优化举措
* perf: 优化相关，比如提升性能、体验
* chore: 项目工程方面的改动，改变构建流程、或者增加依赖库、工具等，</span></span>代码逻辑并未产生任何变化。
* revert: 回滚到上一个版本


### scope

改动范围 scope 用来描述一次提交行为涉及到的模块 / 功能 / 或是其他任何限定的范围。

scope 的具体取值视项目而定。以淘宝详情页为例，取值可以是：header, footer, favorite, sku, etc, ...

### subject

主题 subject 用来概括一次提交行为囊括的内容。

1. 时态方面使用一般现在时，不要用过去时态。虽然查看 log 时，log 内容本身都发生在过去，然而对于主题来说，使用现在时的时态更简洁明确，并且更易达成一致性。
2. 句式使用祈使句式。即一般情况不要增加主语。因为在绝大情况下，主语都是作者「我」。
3. 句尾无需结束标点；如果使用英语，则句首同样无需大写。同样是因为主题（或称标题）本身不用形成完整的句子。
示例：

```
// good
docs: 删除冗余文档

// bad
docs: 我删除了冗余文档
```

### log body

日志的内容主体 body 用来描述详细的提交内容，可写可不写。

1. 时态方面使用一般现在时，不要用过去时态。
2. 句式视情况而定，一般使用祈使句式。
3. 标点方面遵循一般的文档格式规范。

### log footer

日志的内容页脚 footer 用来描述一些补充信息，可写可不写。

* 注明修复的问题。使用 `Closes` 关键字，后面跟上（Gitlab / Redmine 或其他平台的）issue ID。
    ```
      Closes #120
    
      Closes #35, #38, #49
    ```

### Commitizen
[Commitizen](https://github.com/commitizen/cz-cli)<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">是一个撰写合格 Commit message 的工具</span></span>

安装
```plain
npm install -g commitizen
```

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">然后，在项目目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式</span></span>
```
commitizen init cz-conventional-changelog --save --save-exact
```

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">以后，凡是用到</span></span>`git commit`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">命令，一律改为使用</span></span>`git cz`<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">。这时，就会出现选项，用来生成符合格式的 Commit message</span></span>



![粘贴图片.png | center | 650x461](https://cdn.nlark.com/yuque/0/2018/png/115449/1540362574576-35ec7d1a-cfe3-40e0-9df2-7e5d5f244f69.png "")