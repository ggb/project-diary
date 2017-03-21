Title: Updating Elixir
Date: 2017-03-20 11:20
Category: Programming

I just updated a [small project](https://github.com/ggb/triptych) I wrote in elixir almost four years (!) ago. The original project was written with elixir version 0.11.3-beta, the version currently installed on my laptop is 1.3.4 (although 1.4 is already available). The update went very smooth and I would like to document the steps necessary below:

#### Pipes need parantheses

It is not longer possible to write something like

    :::elixir
    foo 1 |> bar 2 |> baz 3

As the well written error message states: This "is ambiguous and should be written as":

    :::elixir
    foo(1) |> bar(2) |> baz(3)

#### HashDict and HashSet are no more

The modules HashDict (*HashDict.new/1 is undefined or private*) and HashSet (*HashSet.new/1 is undefined or private*) are deprecated (and I think they are deprecated since version 1.1). Just replace every HashDict with Map and every HashSet with MapSet. As the APIs are almost the same as before this should work out very well.

#### Remove "Behaviour"

This (*module Supervisor.Behaviour is not loaded and could not be found*) is another no-brainer: Write

    :::elixir
    use Supervisor
    # or
    use GenServer

instead of

    :::elixir
    use Supervisor.Behaviour
    # or
    use GenServer.Behaviour

#### list_to_tuple

Seems that the function *list_to_tuple* was removed from the elixir kernel-module (*undefined function list_to_tuple/1*). Instead just call the function from the erlang stdlib like this:

    :::elixir
    :erlang.list_to_tuple([1,2,3])

And except from some warnings this was all for me to move my little toy project from a very old to a new version of elixir. Nice.