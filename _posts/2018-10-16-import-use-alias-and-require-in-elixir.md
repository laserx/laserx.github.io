---
layout: single
title: elixir 中的 alias, import, require 和 use
date: 2018-10-15 15:12 +0800
toc: true
toc_label: "toc"
tags:
  - elixir
categories: elixir

---

elixir 针对代码复用, 提供了 `alias`, `import` 和 `require` 以上 3 个指令, 以及 1 个名为 `use` 的宏.

简单的罗列一下:

* `alias` 允许我们为模块创建别名
* `import` 可以让我们更方便的调用模块的宏或者函数, 同时, 可以特定引入的宏或者函数
* `require` 用于当我们需要调用模块的宏时
* `use` 用于将模块的 `__using__` 代码注入当前上下文环境中

## alias 和 import

`alias` 和 `import` 从使用上更相似, 在这里, 我们可以一起来看一看:

我们可以这样使用 `alias`,

```elixir
defmodule World do
  defmodule China do
    def location(), do: "Asia"
  end
end

defmodule Hello do
  alias World.China, as: C
  def continent(), do: C.location
end
```

当然, 如果省略 `as` 也是完全没有问题的, `alias` 会使用最后的模块名称作为别名, 上面的例子中, 简写的话, `China` 就将是别名.

如果我们对上面的代码进行一下调整, 观察一下如何使用 `import`,

```elixir
defmodule World do
  defmodule China do
    def location(), do: "Asia"
  end
end

defmodule Hello do
  import World.China, only: [location: 0]
  def continent(), do: location()
end
```

我们可以使用 `only`, `except` 导入所需的函数, 当然, 可以使用 `only: :macros` 或者 `only: :functions`, 选择性的只导入函数或者宏, **注意**, `except` 不支持该种写法.

`alias` 和 `import` 具有相同的词法作用域, 意味着我们即可以在模块中使用, 也可以在模块中的函数内使用. 在特定的模块函数中使用, 其可见性只在该函数作用域范围内生效.

## require

先让我们认识一下 `require`:

```elixir
defmodule World do
  defmacro is_continent do
    quote do: "Asia"
  end
end

defmodule Hello do
  require World

  def continent, do: World.is_continent
end
``` 

当我们需要调用模块中的宏时, 首先需要使用 `require` 加载. 如果不加载而直接调用, 会抛出编译异常.

同时, `require` 指令和 `alias` 一样, 提供 `as` 别名能力.

## use

让我们先看一下 [`use`](https://github.com/elixir-lang/elixir/blob/2f71726757fa0f5a684505dd6eb4e271e64337f3/lib/elixir/lib/kernel.ex#L4937) 的源代码.

我们可以发现, 使用 `use` 本质上等价于:

```elixir
defmodule Example1 do
  use Feature, option: :value
end

defmodule Example2 do
  require Feature
  Feature.__using__(option: :value)
end
```

## 结论

elixir 提供了以上功能使我们能更好的在项目中复用代码.

同时, 在 `iex` 中执行 `h require` 等查询详细的帮助说明, 让我们在开发中可以更直观的查看指定函数的使用方式(文档很详细).