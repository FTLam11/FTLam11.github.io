---
layout: post
title:  Ecto Validations
date:   2019-10-17
tags: [elixir]
---
Recently I've been trying my hand at building a small project with
Phoenix. Most of my time learning Elixir was just spent on reading about
GenServer lol. It's nice to spend some quality time on personal projects
for a change.

I'm still trying to shake the Rails bias, I suppose Rails does spoil us
quite a bit. Modeling is a bit different with Phoenix. Data schemas, changesets,
and repositories are still pretty exotic but I've hardly scraped the surface.

My first roadblock was actually validating a `belongs_to` association on
the child record. Below is a summary of my findings:

> An Ecto schema simply maps a data source into an Elixir struct

Setting up a `belongs_to` association in the child schema does not
automatically validate its presence at the application level. However
this information is used by `assoc_constraint/3` to infer the foreign key
constraint name.

> assoc_constraint/3 and foreign_key_contraint/3 do not validate
> presence of the foreign key

These two constraints come into play once the actual database query is
made. I only assumed it'd suffice because the
[Hexdocs](https://hexdocs.pm/ecto/Ecto.Changeset.html#validate_required/3)
explicitly state not to use `validate_required/3` to validate
associations.

My rationale is I don't even want to attempt the database query if the
foreign key column is `nil`. At the database level, the foreign key is
non-nullable. I ended going against the documentation and included
the foreign key in `validate_required/3`.

> Using changesets is not mandatory

It was kinda shocking to be able to easily insert records into the
database. I got wrecked while following along in the book I was using,
since it used `Kernel.struct/2` and chained that into a repository
insert before changesets were introduced. Without using changesets,
Phoenix will throw an error...

```elixir
defmodule Disc.Dawg.Release do
  use Ecto.Schema
  import Ecto.Changeset

  schema "releases" do
    field(:title, :string)
    field(:cover_url, :string)
    field(:description, :string)
    field(:released_at, :date)
    timestamps()
    belongs_to :artist, Disc.Dawg.Artist
  end

  def changeset(release, params \\ %{}) do
    release
    |> cast(params, [:artist_id, :title, :cover_url, :description, :released_at])
    |> validate_required([:title, :artist_id])
    |> assoc_constraint(:artist)
  end
end
```

```elixir
defmodule Disc.Dawg.Grid do
  alias Disc.Dawg.{Artist, Release, Repo}

  @repo Repo

  def insert_record(module, attrs) do
    model = module |> struct(attrs)
    apply(module, :changeset, [model, attrs])
    |> @repo.insert()
  end
end
```
