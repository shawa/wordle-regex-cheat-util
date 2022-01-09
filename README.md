# Wordle Regex Cheat Util

`wrcutil` (Wordle Regex Cheat Util) is a little script I wrote to cheat at wordle using Perl regex and `/usr/share/dict/words`


[Wordle](https://www.powerlanguage.co.uk/wordle/) is a constraint-solver puzzle game; there's a 5x6 grid of letters, and you've got 6 guesses to guess the word. You get feedback on whether the letter is in the word, not in the word, in the right place in the word etc. See the site for the full rules.

`/usr/share/dict/words` is the 'standard' set of dictionary words that comes on most \*nix machines.

Thus we can cobble together a set of scripts to make guessing much easier, using [Regular Expressions](https://en.wikipedia.org/wiki/Regular_expression)s:


## Usage

```
$ ./wrcutil <PATTERN>
```

Where `<PATTERN>` is a Perl-compatible regular expression which matches 5-character strings. Here's an example run:


I'll use `-` to mean 'no match at all', `+` to mean 'in the word, but in the wrong place', and `^` to mean, in the word, and in the right place.

My first random guess was `APPLE`, which wordle returned as

```
APPLE
----^
```

So we know A, P, and L aren't in the string, and E is at the end. So we can build our first regex, and pick a random word that matches:

```
$ ./wrcutil '[^apl]{4}e' | shuf | head -n 1
thyme
```

Plug it in:

```
APPLE
THYME
----^
```

So now none of A, P, L, T, H, or Y are in the string:

```
./wrcutil '[^aplthym]{4}e' | shuf | head -n 1
swine
```

```
APPLE
THYME
SWINE
----^
```

Lots of exclusions...

```
./wrcutil '[^aplthymswin]{4}e' | shuf | head -n 1
grove
```

```
APPLE
THYME
SWINE
GROVE
-++-^
```

So here we can keep it simple, and just add v:

```
 ./wrcutil 'g[^aplthymswinv]{3}e'
geode
gorge
gouge
grebe
```

We can probably just look at the list and guess, but to go even more, we can just tack on another regex. We know the word has at an O and an R, so just grep for those again:

```
$ ./wrcutil 'g[^aplthymswinv]{3}e' | grep 'o' | grep 'r'
gorge
```
