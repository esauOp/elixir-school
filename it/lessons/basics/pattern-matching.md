---
version: 0.9.0
layout: page
title: Pattern Matching
category: basics
order: 4
lang: it
---

_Pattern matching_ è un aspetto fondamentale di Elixir, permette di abbinare semplici valori, strutture dati e funzioni. In questa lezione inizieremo a vedere come viene usato il pattern matching.

{% include toc.html %}

## Operatore match

Se pronto ad essere spiazzato? In Elixir, l'operatore `=` è in realtà un operatore match. Tramite l'operatore match possiamo assegnare ed abbinare valori, diamo uno sguardo:

```elixir
iex> x = 1
1
```

Ora proviamo un semplice abbinamento:

```elixir
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

Ora proviamo con una delle collezioni che conosciamo:

```elixir
# Liste
iex> list = [1, 2, 3]
iex> [1, 2, 3] = list
[1, 2, 3]
iex> [] = list
** (MatchError) no match of right hand side value: [1, 2, 3]

iex> [1|tail] = list
[1, 2, 3]
iex> tail
[2, 3]
iex> [2|_] = list
** (MatchError) no match of right hand side value: [1, 2, 3]

# Tuple
iex> {:ok, value} = {:ok, "Successful!"}
{:ok, "Successful!"}
iex> value
"Successful!"
iex> {:ok, value} = {:error}
** (MatchError) no match of right hand side value: {:error}
```

## Operator pin

Abbiamo appena imparato che l'operatore match gestisce l'assegnazione quando la parte sinistra dell'abbinamento include una variabile. In alcuni casi questo comportamento, cambiare l'abbinamento di una variabile, non è desiderabile. Per queste situazioni, abbiamo l'operatore pin `^`.

Quando _fissiamo_ (_pin_) una variabile, abbiniamo il valore esistente invece di cambiare l'abbinamento con un nuovo valore. Vediamo come funziona:

```elixir
iex> x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
iex> {x, ^x} = {2, 1}
{2, 1}
iex> x
2
```

Elixir 1.2 ha introdotto il supporto per usare l'operatore pin per le chiavi delle mappe e nelle clausole delle funzioni:

```elixir
iex> key = "hello"
"hello"
iex> %{^key => value} = %{"hello" => "world"}
%{"hello" => "world"}
iex> value
"world"
iex> %{^key => value} = %{:hello => "world"}
** (MatchError) no match of right hand side value: %{hello: "world"}
```

Un esempio di _pinning_ in una clausola di una funzione:

```elixir
iex> greeting = "Hello"
"Hello"
iex> greet = fn
...>   (^greeting, name) -> "Hi #{name}"
...>   (greeting, name) -> "#{greeting}, #{name}"
...> end
#Function<12.54118792/2 in :erl_eval.expr/5>
iex> greet.("Hello", "Sean")
"Hi Sean"
iex> greet.("Mornin'", "Sean")
"Mornin', Sean"
```
