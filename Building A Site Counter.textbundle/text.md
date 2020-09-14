---
path: “/building-a-site-counter“
date: "2020-09-08”
title: “Building a site counter”
tags: [”phoenix”, “elixir”, “learning”]()
excerpt: “This is some of the stuff I learnt building a site counter in Phoenix”
---

This follows my first tutorial building a [Router in Phoenix][2], and here’s what I learnt along the way.

Remember those cute site counter things we used to see on websites back in the day? That felt like a reasonably simple project to start because it seems simple enough:
1. store a number
2. add 1 to it
3. save it somewhere
Turns out there’s a few twists and turns you may need to worry about. And my Dunning-Kruger-ness definitely got in the way (god I hope I used that correctly, because that’d be meta-Dunning-Kruger)

# First Approach

So, the first approach I tried is below ([link to commit][3]):  

```elixir
def add(conn, _params) do
	addition = Adder.Addition |> Ecto.Query.first() |> Adder.Repo.one()
	changeset = Adder.Addition.changeset(addition, %{sum: addition.sum + 1})
	Adder.Repo.update(changeset)
	json(conn, %{sum: addition.sum + 1})
end
```
  
Sounds simple enough right? Query the first row out of a database (I was using Postgres), add 1 to it, update the row, return JSON.

Turns out, you run into an issue: if you run lots of concurrent requests, eventually you’d have a place where two concurrent requests would add a number at the same time and instead of incrementing by 2, would increment by 1.

//TODO: insert a diagram here because I’m doing a terrible job explaining this

# Locks?

Here’s where having an approximate knowledge of many things comes back to bite you in the butt
![][image-1]

> Me internally: oh this is a concurrency issue, I should look at locks!

And so I did. I ended up using a row level lock in postgres, but that didn’t quite fix things either. I suspect this may have been me holding ecto wrong, and I still don’t quite know the answer yet.

```elixir
def add(conn, _params) do
	addition =
		Adder.Addition
		|> Ecto.Query.first()
		|> Ecto.Query.lock("FOR UPDATE")
		|> Adder.Repo.one()

	changeset = Adder.Addition.changeset(addition, %{sum: addition.sum + 1})
	Adder.Repo.update(changeset)

	json(conn, %{sum: addition.sum + 1})
end
```

# Raw SQL?

Welp, I was getting mad at this point and I needed _something_, so I decided to use Raw SQL to update the number, and this worked! 🎉 Since postgres was responsible for doing the math, it handled concurrency by itself. 

```elixir
def add(conn, _params) do
	{:ok, _} = Ecto.Adapters.SQL.query(Adder.Repo, "UPDATE Addition SET sum=sum+1")
	{:ok, results} = Ecto.Adapters.SQL.query(Adder.Repo, "SELECT * from Addition")
	[[_, sum]] = results.rows
	json(conn, %{sum: sum})
end
```

> But what if I need to do something more complex to process my data?

# Transactions

This question really did haunt me. A little bit of googling led me to Manning’s excellent Phoenix in Action book, that has a section on Transactions! 

```elixir
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
```

Turns out, wrapping our changes in an ecto transaction does the trick!

[2]:	/routing-in-phoenix
[3]:	https://github.com/codeOfRobin/try-phoenix-database-rollover/commit/fd50d5b2158021da65e4c115d777b2917bf9e31a#diff-a377387ada6665e4e5bbf23d0272f46c

[image-1]:	http://slackwise.net/files/images/Adventure%20Time/Adventure%20Time%20-%20I%20have%20approximate%20knowledge%20of%20many%20things.gif