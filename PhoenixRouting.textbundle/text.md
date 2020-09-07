---
path: “/phoenix-beginner-routing“
date: "2020-09-07”
title: “Phoenix 0 to Routing”
tags: [”phoenix”, “elixir”, “learning”](#)
excerpt: “I’ve been beginning to learn elixir, and this was the simplest recipe I could make for a simple request”
---

I’ve been learning [the phoenix framework](https://www.phoenixframework.org "the phoenix framework") lately (loving it so far) and I’ve wanted a 0 to 100 path to making a request quickly. 

---

Do note that I’m a complete beginner so this is pretty basic. You want to setup a new app via `mix phx.new`

##  Routing
First of all, you have to add a new scope or a new route in `route.ex`

```elixir
scope "/add", <YourProject>Web do
    pipe_through :api

    post "/", <ControllerName>, :<functionName>
    get "/", <ControllerName>, :<functionName>
end
```

Here, we’re adding a new scope called add so this route will be accessible via `host/add`

//TODO: Q for later - Can you nest scopes?

## New Controller

Add a new controller in `<YourProject>_web/controllers` 

```elixir
defmodule AdderWeb.AddingController do
  use AdderWeb, :controller
  require Ecto.Query
  require Adder.Repo

  def add(conn, _params) do
    {:ok, dict} =
      Adder.Repo.transaction(fn ->
        addition =
          Adder.Addition
          |> Ecto.Query.first()
          |> Ecto.Query.lock("FOR UPDATE")
          |> Adder.Repo.one()

        changeset = Adder.Addition.changeset(addition, %{sum: addition.sum + 1})
        Adder.Repo.update(changeset)

        %{sum: addition.sum + 1}
      end)

    json(conn, dict)
  end

  def reset() do
    addition =
      Adder.Addition
      |> Ecto.Query.first()
      |> Ecto.Query.lock("FOR UPDATE")
      |> Adder.Repo.one()

    changeset = Adder.Addition.changeset(addition, %{sum: 0})
    Adder.Repo.update(changeset)
  end
end

```
  
And you’re done! 