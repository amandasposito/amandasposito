---
layout: post
title:  "Ecto, what about it?"
author: "Amanda Sposito"
date:   2016-09-03 08:40:37 -0300
categories:
  - elixir
  - database
  - ecto
---

If you are studying Elixir and thought about how to connect with the database, you’ve probably heard of it.

![Photo by Francesco Ungaro on Unsplash](/assets/images/ecto-what-about-it-cover.jpg?v=1)

From its GitHub page: “Ecto is a domain-specific language for writing queries and interacting with databases in Elixir.”

Basically, it is a lib you can use to interact with your database. Think about it as a layer between the application and the database itself.

It comes with a series of facilities and you can do things like writing migrations, insert, delete, update and query your data, but most important, don’t think about it as if it was your ORM.

It is common people coming to Elixir from an OOP background and so thinking Ecto is your ORM. It is not.

ORM means object-relational mapping, it’s about objects. Elixir is a functional programming, there are no objects there. The idea is not to map your table into an object, the idea is to map your data source as needed.

It is split into 4 main components:

* Repo
* Schema
* Changeset
* Query

## Repo

Is your data storage, every time you want something from your database, you go to the repository. Think about it as a mediator between the domain and the access layer data.

Through the repo, you will execute your queries. There are some functions you can use, such as: insert, insert\_all, delete, delete\_all, update, update\_all.

[https://hexdocs.pm/ecto/Ecto.Repo.html](https://hexdocs.pm/ecto/Ecto.Repo.html)

## Schema

It maps the source data into an Elixir structure. You define it once and can use it to fetch data and coordinate changes.

In Ecto's first version, it was called Model, but in favor of a more data-focused mindset, they changed it.

{% highlight elixir %}
defmodule Course do
  use Ecto.Schema

  schema "courses" do
    field :name
    field :description

    has_many :classes, Class, on_delete: :delete_all

    timestamps()
  end
end
{% endhighlight %}

One thing that is important to be said, is that you don’t always need a schema to perform queries. You may want to do something that doesn’t necessary need a pre-defined structure, for example, a report query.

Also, you can create different schemas depending on what you want to do. For example, you can create a schema to read and another one to write.

[https://hexdocs.pm/ecto/Ecto.Schema.html](https://hexdocs.pm/ecto/Ecto.Schema.html)

## Changeset

This one caused me a lot of trouble at the beginning. I couldn’t figure out why did I need it in the first place. I came from an OOP background and when I want to validate something, the errors are stored in the object.

![](/assets/images/mindblown.gif)

Once again, there are no objects in Elixir, and then you can’t store validation data into the object. So that is what Changeset is about, it is a guarantee that we won't insert incorrect data into the database. Think about it as a collection of changes.

> Changesets allow filtering, casting, validation and definition of constraints when manipulating structs.

{% highlight elixir %}
def changeset(user, params \\ %{}) do
  user
  |> cast(params, [:name, :email, :age])
  |> validate_required([:name, :email])
  |> validate_format(:email, ~r/@/)
  |> validate_inclusion(:age, 18..100)
  |> unique_constraint(:email)
end
{% endhighlight %}

![Here is an example when I try to insert a data that breaks the changeset validation.](/assets/images/ecto-changset-validation-error.png)

[https://hexdocs.pm/ecto/Ecto.Changeset.html](https://hexdocs.pm/ecto/Ecto.Changeset.html)

## Query

Written in Elixir syntax, they generate a SQL instruction, to retrieve information through the repo. The queries are sanitized and **protected from SQL Injection**.

{% highlight elixir %}
(from c in Course,
 select: c)
{% endhighlight %}

Also, the queries are **composable**, you can continue to use a query in another part of the code if you want to.

{% highlight elixir %}
# Create a query
query = from u in User, where: u.age > 18

# Extend the query
query = from u in query, select: u.name
{% endhighlight %}

One thing that is important to be said, is that Ecto is **not lazy load** and it is designed to be this way to avoid N+1 problems.

When you have something that is lazily loaded, you don’t stop to think about how it will behave at the database level and it brings performance issues.

> “A lot of applications have N+1 query issues because things can be lazy-loaded automatically, you basically don’t care and then when you see you are loading a huge chunk of your database dynamically and in a non-performant way at all.” José Valim — [(https://changelog.com/208)](https://changelog.com/208)

So, if you want to bring together the association data, you need to tell the query to do so, using the preload. There are some ways you can do that.

In the example below, we are telling the query to bring the class association together. In this case, it will perform two queries, one for fetching the courses and another one to fetch the classes.

{% highlight elixir %}
(from c in Course,
 preload: [classes: class],
 select: c)
{% endhighlight %}

But very often you want them to come to the same query. You can do this by adding this join to query. It will fetch the class records in an inner join.

{% highlight elixir %}
(from c in Course,
join: class in assoc(c, :classes),
preload: [classes: class],
select: c)
{% endhighlight %}

You can use `left_join`, `right_join` and `full_join`; the inner join is the default.

You can pass other queries to the preload. For example, if you want to filter or customize how it will bring the results:

{% highlight elixir %}
classes = (from klass in Class,
           order_by: klass.created_at)

query = (from course in Course,
        preload: [classes: ^classes])
{% endhighlight %}

In the example above, it will fetch two queries, one for the courses and another one to bring the associated classes ordered by the creation date.

These are the main parts about Ecto, it is a great tool and it brings a lot of things that can help you interact with your data source, and it is definitely worth taking a look.

There was a lot of good changes in the second version, and it is a good idea to take a look at the documentation, it is a great resource to understand better how it works.

I hope it helps.

![](/assets/images/thats-all-folks.gif)
