![SetVer Logo](https://raw.githubusercontent.com/RocketRace/setver/main/logo.png)

The modern landscape of software development is hectic and unforgiving. Dependency hell, management hell, Jira (which is a hell of its own)... It's impossible to maintain order — or to maintain a project — without a sufficiently rigid foundation, else your whole team crumbles apart. You need to pick something solid to base your project on... So *why not pick the most solid foundation of them all?* ***Why not pick set theory?***

--------

> Setaceous: resembling a bristle in form or texture

**Setaceous Versioning (SetVer)** is a versioning scheme designed to revolutionize the world of software development. It is based on the purest, most* foundational mathematical object, the set. Under SetVer, your projects don't get a version number — they get a **version set**. When beginning your project, assign `{}` as your version set. Each time your project updates, add a new element to your version set. Version sets can be compared very elegantly, using simply the subset relation `⊆`.

# Here's a bunch of definitions and prose

## Version Sets

A **version set** represents a set containing zero or more arbitrarily nested unordered sets, with the restriction that every element inside a set is unique. These are represented with the brace characters `{` (U+007B) and `}` (U+007D). Elements of sets are delimited by an empty string, i.e. concatenated. Below is a regular expression (using PCRE) that parses valid version sets:

```regex
^({(?:((?1))(?!(?1)*\2))*})$
```

"Thank you, Olivia! It's all so clear to me now," you say. Well, you're welcome! But in case you aren't welcome, here are some examples of valid and invalid version sets.

Valid:
+ `{}`
+ `{{}}`
+ `{{{}}{}{{{}}}}`
+ `{{{{}{{}}}{{}}}{}}`

Invalid:
- `{`
- `{{{}}}}`
- `{{}{}}`
- `{{{}{}}{}}`

The **initial version set** of a project should be `{}`. This is analogous to a "version zero" in other less elegant versioning schemes (gross).

## Version Comparison

Two version sets are compared using the subset relation `⊆` defined on the sets represented by a version set. That is, for any version sets `X` and `Y`, we say that `X ⊆ Y` if every element (i.e. immediate child) of the set represented by `X` is also an element of the set represented by `Y`. The `⊆` relation is a partial ordering on sets, which gives us quite a few useful properties: 

* For any version set `X`, `X ⊆ X`.
* For any version sets `X` and `Y`, if `X ⊆ Y` and `Y ⊆ X`, then `X` and `Y` must be the same set.
* For any version sets `X`, `Y`, and `Z`, if `X ⊆ Y` and `Y ⊆ Z`, then `X ⊆ Z`.

It must be noted that `⊆` is not a total order over the class of sets. This means that there exists a pair of version sets `A` and `B` such that neither `A ⊆ B` nor `B ⊆ A`. In fact, there are infinitely many such pairs of version sets! (Proof is left as an exercise to the reader.) *However,* when a programmer is faced with an unwanted quirk in the system, they simply reframe the bug as a feature! That is to say, the existence of incomparable version sets is cool and good, actually. More on that later.

## Version Comparison Comparison

When two version sets `X` and `Y` are compared, there are four possible outcomes to the situation:

* `X ⊆ Y` and `Y ⊆ X`, in which case we say `X = Y`.
* `X ⊆ Y` but not `Y ⊆ X`, in which case we say `X ⊂ Y`.
* `Y ⊆ X` but not `X ⊆ Y`, in which case we say `X ⊃ Y`.
* Neither `X ⊆ Y` nor `Y ⊆ X`, in which case we say `X ⫓̸ Y`.

These four outcomes (`⊂`, `=`, `⊃` and `⫓̸`) can *also* be compared under a partial order, with the following rules:

| Row ≤ column | `⊂` | `=` | `⊃` | `⫓̸` |
| ------------:|:---:|:---:|:---:|:---:|
|          `⊂` | ✅ | ✅ | ✅ | ❌ |
|          `=` | ❌ | ✅ | ✅ | ❌ |
|          `⊃` | ❌ | ❌ | ✅ | ❌ |
|          `⫓̸` | ❌ | ❌ | ❌ | ❌ |

"**But wait!!!** This seems really familiar," you might say. And you'd be correct! Let us rename:

* `⊂` => `0`
* `=` => `1`
* `⊃` => `Infinity`
* `⫓̸` => `NaN`

And voilà!

| Row ≤ column | `0` | `1` | `Infinity` | `NaN` |
| ------------:|:---:|:---:|:----------:|:-----:|
|          `0` | ✅ | ✅ | ✅ | ❌ |
|          `1` | ❌ | ✅ | ✅ | ❌ |
|   `Infinity` | ❌ | ❌ | ✅ | ❌ |
|        `NaN` | ❌ | ❌ | ❌ | ❌ |

This motivates the following.

SetVer defines a **comparison function**, which is a function that takes as input two version sets denoted by `X` and `Y`, and returns a floating-point number representing the result of the comparison. The output value is either `0`, `1`, `Infinity`, or `NaN`, denoting `X ⊂ Y`, `X = Y`, `X ⊃ Y` and `X ⫓̸ Y` respectively.

> For the technically inclined, this subset of floating-point numbers can be represented using a 1-bit significant and 1-bit mantissa, omitting the sign bit entirely. Unfortunately, binary2 is not a valid floating point format according to IEEE 754. As I cry myself to sleep, I rewatch [NaN Gates and Flip FLOPS](https://www.youtube.com/watch?v=5TFDG-y-EHs) for the seventh time.

## Alternative Representations

SetVer defines three alternative representations for version sets:

### The Integralternative

By replacing the `{` characters in a version set with `0` and `}` with `1`, one obtains a bitstring which can then be interpreted as an integer and efficiently transmitted over the network, stored in files or spoken out loud to your friends. (You should mention SetVer to your friends.) 

As an example: `{{{{}}{}}{{}}}` => `00001101100111` => `871`

As version sets are unordered, we also have: `{{{}}{{{}}{}}}` => `00011000110111` => `1591`

Therefore, under the Integralternative scheme, `871 = 1591`.

### The Picturrogate

A version set can be encoded in a picture, such that every pair of braces `{}` is represented by a closed loop, and every element inside braces is drawn inside the loop.

As an example: `{{{{}}{}}{{}}}` => 

![Visual representation of the version set with loops]()

### The To-Be-Finished

