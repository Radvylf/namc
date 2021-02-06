# Not A Markov Chain

But it is!

Basically, NAMC is a project of mine, intended to be used to generate text that sounds like something a human would write. It didn't work. I'm putting it here since I think it's cool, and it still outperforms a normal markov chain in my opinion (for text generation).

## Markov Chains

A Markov chain is pretty simple; for each state, in this case the last word generated, there are a number of states it can choose next. For example, for the word `the`, the states it could choose given the sentence `the brown dog was under the shed but the shed did not shelter the dog` would be:

```
the -> brown
the -> shed
the -> shed
the -> dog
```

We can expand this to every word in the sentence, which results in:

```
        -> the
the     -> brown
brown   -> dog
dog     -> was
was     -> under
under   -> the
the     -> shed
shed    -> but
but     -> the
the     -> shed
shed    -> did
did     -> not
not     -> shelter
shelter -> the
the     -> dog
dog     -> 
```

Notice how there are two places where no word is listed; this indicates either the beginning of the sentence or the end. Notice how some words are mapped to multiple things; this results in something like:

```
        -> [the]
the     -> [brown, shed, shed, dog]
brown   -> [dog]
dog     -> [was, []]
was     -> [under]
under   -> [the]
shed    -> [but, did]
but     -> [the]
did     -> [not]
not     -> [shelter]
shelter -> [dog]
```

The initial state will be `the`. Now, until the state is `[]`, we pick a random item the state can map to. The result could look something like:

```
the shed did not shelter the shed but the brown dog was under the dog
```

You can also get overly short or nonsensical results like `the dog` or `the shed did not shelter the shed did not shelter the shed did not ... shelter the dog`.

## NAMC

My idea was pretty simple: Rather than looking _only_ at the previous word (current state), what if you looked multiple levels back? This is the basis of Not-A-Markov-Chain. For this example, we'll use a depth-2 NAMC. This means, rather than a single map, we get two:

**D1:**

```
        -> [the]
the     -> [brown, shed, shed, dog]
brown   -> [dog]
dog     -> [was, []]
was     -> [under]
under   -> [the]
shed    -> [but, did]
but     -> [the]
did     -> [not]
not     -> [shelter]
shelter -> [dog]
```

**D2:**

```
        -> [brown]
the     -> [dog, but, did, []]
brown   -> [was]
dog     -> [under, []]
was     -> [the]
under   -> [shed]
shed    -> [the, not]
but     -> [shed]
did     -> [shelter]
not     -> [the]
shelter -> [dog]
```

_WIP_
