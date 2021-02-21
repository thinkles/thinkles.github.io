---

title: linear
date: {{ date }}
updated: {{date}}
tags: linear
categories: 工具

---

### preitter linner

- **为什么要两者配合使用？**

- 在 ESLint 推出 --fix 参数前，ESLint 并没有自动化格式代码的功能，要对一些格式问题做批量格式化只能用 Prettier 这样的工具。

- ESLint 的规则并不能完全包含 Prettier 的规则，两者并不是简单的谁替代谁的问题。

- > 在 ESLint 推出 --fix 命令行参数之后,也可以使用 standard aribnb 等规范格式化代码，如果觉得已经够用就可以不使用 Prettier。

- **ESLint 和 Prettier 冲突问题:**

- 对于他们交集的部分规则，ESLint 和 Prettier 格式化后的代码格式不一致。所以当你用 Prettier 格式化代码后再用 ESLint 去检测，会出现一些因为格式化导致的 warning。
- **解决方法:**

1. 运行 Prettier 之后，再使用 eslint --fix 格式化一把，这样把冲突的部分以 ESLint 的格式为标准覆盖掉，剩下的 warning 就都是代码质量问题了。

   > 简单理解: 使用 eslint 覆盖有冲突的规则

2. 在配置 ESLint 的校验规则时候把和 Prettier 冲突的规则 disable 掉，然后再使用 Prettier 的规则作为校验规则。那么使用 Prettier 格式化后，使用 ESLint 校验就不会出现对前者的 warning。

> 简单理解: 改变 eslint 规则,使其不再冲突

- **为什么不能先使用 ESLint 再使用 Prettier:**

- 如果你后使用 Prettier，那么格式化后提交的代码在下一次或者别人 checkout 代码后是通不过 lint 校验的。

- **方法一:**
- 安装 prettier-eslint , prettier-eslint-cli

- > prettier-eslint 会一次执行 prettier 和 eslint --fix 命令。
  >
  > 整个流程是：Code ➡️ prettier ➡️ eslint --fix ➡️ Formatted Code。

- 当然你也可以进行手动 fix
- **方法二:**

- 主要是要改变 eslint 的规则配置文件，安装 eslint-config-prettier

- > 把其配置到 extends 字段，实现 prettier 规则对 eslint 规则的覆盖。
  >
  > 如果只是单纯的覆盖效验时的规则, 那么我们还是需要分别运行 prettier 和 eslint 命令来完成
  >
  > 我们可以利用插件 把两个操作整合一下

- 利用 eslint-plugin-prettier 插件实现 将 prettier 规则以插件的形式加入到 ESLint 里面,让其作为规则运行, 不符合 prettier 规则的,整合为单独的 eslint 问题

- > 在使用 eslint --fix 时候，实际使用 prettier 来替代 eslint 的格式化功能。
  >
  > 来这里使用 官方推荐配置:
  >
  > ```json
  > "plugin:prettier/recommended"
  > //它是一些配置的缩写  可在官网查询
  > ```

- 当然你可以单独使用 eslint-plugin-prettier 插件, 但是必须关闭格式化代码的规则,因为本质上冲突的规则还是会引起 lint 使用 eslint-config-prettier 将其覆盖是最好的方法
