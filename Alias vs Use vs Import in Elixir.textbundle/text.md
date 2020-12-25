---
path: “/use-vs-import-vs-alias-in-elixir“
date: "2020-11-13”
title: “Use vs Import vs Alias in Elixir”
tags: [”Elixir”]()
---

Code example we’re using throughout this post:  

```elixir
def changeset(item, params \\ %{}) do
    item
    |> Ecto.Changeset.cast(params, @changeset_validating_fields)
    |> Ecto.Changeset.validate_required(:title)
    |> Ecto.Changeset.validate_lenght(:title, min: 3)
```

# Import 

If we `import Ecto.Changeset`, we can change this code to:

```elixir
def changeset(item, params \\ %{}) do
    item
    |> cast(params, @changeset_validating_fields)
    |> validate_required(:title)
    |> validate_lenght(:title, min: 3)
  end
```

# Alias

if we `alias Ecto.Changeset` we can change this code to:
```elixir
def changeset(item, params \\ %{}) do
    item
    |> Changeset.cast(params, @changeset_validating_fields)
    |> Changeset.validate_required(:title)
    |> Changeset.validate_length(:title, min: 3)
  end
```

we can also `alias Ecto.Changeset, as: c`

# Use 

Use is a bit of a special thing in elixir: it leverages metaprogramming capabilities into elixir. It’s also how elixir gets subclassing like behaviours without actually having subclasses!

Ref:
1. The excellent [“Phoenix in Action” book by Manning][2]
2.  [https://curiosum.dev/blog/alias-import-require-use-in-elixir][3]
3. [https://zohaib.me/use-in-elixir-explained/][4]

[2]:	https://www.manning.com/books/phoenix-in-action
[3]:	https://curiosum.dev/blog/alias-import-require-use-in-elixir
[4]:	https://zohaib.me/use-in-elixir-explained/