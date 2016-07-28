---
layout: page
title: Αντιπαραβολές Προτύπων
category: basics
order: 4
lang: gr
---

Οι αντιπαραβολές προτύπων είναι ένα πολύ δυνατό μέρος της Elixir.  Μας επιτρέπει να αντιπαραβάλουμε απλές τιμές, δομές δεδομένων, ακόμα και συναρτήσεις.  Σε αυτό το μάθημα θα ξεκινήσουμε να βλέπουμε πως χρησιμοποιούνται οι αντιπαραβολές προτύπων.

{% include toc.html %}

## Τελεστής Αντιπαραβολής

Είστε έτοιμοι για μια έκπληξη;  Στην Elixir, ο τελεστής `=` στην πραγματικότητα χρησιμοποιείται σαν τελεστής αντιπαραβολής.  Μέσα από τον τελεστή αντιπαραβολής μπορούμε να ορίζουμε και μετά να αντιπαραβάλουμε τιμές.  Για να ρίξουμε μια ματιά:

```elixir
iex> x = 1
1
```

Τώρα ας προσπαθήσουμε μια απλή αντιπαραβολή:

```elixir
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

Ας το προσπαθήσουμε με μερικές από τις συναρτήσεις που ξέρουμε:

```elixir
# Λίστες
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

# Τούπλες
iex> {:ok, value} = {:ok, "Successful!"}
{:ok, "Successful!"}
iex> value
"Successful!"
iex> {:ok, value} = {:error}
** (MatchError) no match of right hand side value: {:error}
```

## Τελεστής Καρφίτσας

Μόλις μάθαμε ότι ο τελεστής αντιπαραβολής εκτελεί μια εκχώρηση όταν η αριστερή μεριά της αντιπαραβολής περιέχει μια μεταβλητή.  Σε μερικές περιπτώσεις, αυτή η επανασύνδεση δεν είναι επιθυμητή. Για αυτές τις περιπτώσεις έχουμε τον τελεστή καρφίτσας: `^`.

Όταν καρφιτσώνουμε μια μεταβλητή, αντιπαραβάλουμε με την υπάρχουσα τιμή αντί να επανασυνδέουμε σε μια νέα.  Ας δούμε πως δουλεύει:

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

Η Elixir 1.2 εισήγαγε υποστήριξη για καρφίτσες σε κλειδιά χαρτών και ρήτρες συναρτήσεων:

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

Ένα παράδειγμα καρφιτσώματος σε μια ρήτρα συνάρτησης:

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