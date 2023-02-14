# Researching Commands

We're going to be taking a look at a few command-line options for the `grep` command.

We'll be taking a look directly at the `man` (manual) page for `grep`, but Git Bash doesn't come with the `man` command.

Insteaad, we'll be looking at the manual [online](https://linuxcommand.org/lc3_man_pages/grep1.html): https://linuxcommand.org/lc3_man_pages/grep1.html

All of the following options are taken from the website.

## `-r, --recursive`
```
Read all files under each directory, recursively, following
symbolic links only if they are on the command line.
```

It sounds like we don't need to `find` files and direct the paths to the `grep` command anymore! Let's try out the `-r` flag with `grep`.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/Week 3/docsearch (main)
$ grep -r hello 
written_2/travel_guides/berlitz1/WhereToHongKong.txt:        the local people smile “hello” and, if you’re lucky, point you to a
written_2/travel_guides/berlitz1/WhereToItaly.txt:        (or Ca’ Grande), while gondoliers claim Othello’s Desdemona lived in
```

Nice! It found a few files with `hello` as a pattern.

Let's try a more specific pattern... We'll look for the word `oxymoron`.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/Week 3/docsearch (main)
$ grep -r oxymoron ./written_2/
./written_2/non-fiction/OUP/Abernathy/ch1.txt:For many commentators, a book about the future of the U.S. apparel and textile industries is still an oxymoron. The conventional wisdom paints a grim picture of where these industries are headed. Low-cost labor overseas and the increasing penetration of imports have certainly undercut American apparel manufacturers; apparel imports grew rapidly in most categories starting in the mid-1970s. If we measure import penetration in physical units (rather than dollar value),12 import penetration for men’s and boys’ suits, for example, went from just 10 percent in 1973 to 43 percent by 1996. A similar expansion in imports occurred for men’s and boys’ trousers, women’s and girls’ dresses, and women’s slacks and shorts.13
```

Looks like `Abernathy/ch1.txt` has an instance.

## `-l, --files-with-matches`
```
Suppress  normal  output;  instead  print  the name of each
input file from  which  output  would  normally  have  been
printed.  The scanning will stop on the first match.
```

Looks like this completely changes the output to *only* output file names that contain at least one instance of a pattern! We'll be using it in conjunction with `-r`.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/Week 3/docsearch (main)
$ grep -r -l colores ./written_2/
./written_2/non-fiction/OUP/Castro/chV.txt
```

As we can see, there's no output of the actual line that contains `colores`. Just the file name.

Let's look for the pattern `video games`.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/Week 3/docsearch (main)
$ grep -r -l "video games" ./written_2/
./written_2/non-fiction/OUP/Berk/ch1.txt
./written_2/non-fiction/OUP/Berk/CH4.txt
./written_2/non-fiction/OUP/Berk/ch7.txt
./written_2/travel_guides/berlitz2/Poland-WhatToDo.txt
./written_2/travel_guides/berlitz2/Vallarta-WhatToDo.txt
```

That's quite a few. Didn't expect that. I guess Poland likes video games? We can't tell any of the context because the line with the match isn't printed.

## `-c, --count`
```
Suppress normal output; instead print a count  of  matching
lines  for  each  input  file.  With the -v, --invert-match
option (see below), count non-matching lines.
```

Looks like this is another option that allows us to override the output. Instead, we get *all* the files with a corresponding count for each match.

We'll be combining this with `-r`.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/Week 3/docsearch (main)
$ grep -r -c juego ./written_2/
./written_2/non-fiction/OUP/Abernathy/ch1.txt:0
./written_2/non-fiction/OUP/Abernathy/ch14.txt:0
./written_2/non-fiction/OUP/Abernathy/ch15.txt:0

...

./written_2/travel_guides/berlitz2/Cancun-WhereToGo.txt:1

...

./written_2/travel_guides/berlitz2/Vallarta-History.txt:0
./written_2/travel_guides/berlitz2/Vallarta-WhatToDo.txt:0
./written_2/travel_guides/berlitz2/Vallarta-WhereToGo.txt:0
```

Wow! That's too big to fit in a lab report, and, considering that there's only one file with a match, it's like finding a needle in a haystack. Let's try a more common word...

How about... *`the`* most common word?

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/Week 3/docsearch (main)
$ grep -r -c the ./written_2/
./written_2/non-fiction/OUP/Abernathy/ch1.txt:68
./written_2/non-fiction/OUP/Abernathy/ch14.txt:57
./written_2/non-fiction/OUP/Abernathy/ch15.txt:62
./written_2/non-fiction/OUP/Abernathy/ch2.txt:63
./written_2/non-fiction/OUP/Abernathy/ch3.txt:50
./written_2/non-fiction/OUP/Abernathy/ch6.txt:60

...

./written_2/travel_guides/berlitz2/PuertoRico-History.txt:22
./written_2/travel_guides/berlitz2/PuertoRico-WhatToDo.txt:57
./written_2/travel_guides/berlitz2/PuertoRico-WhereToGo.txt:116
./written_2/travel_guides/berlitz2/Vallarta-History.txt:32
./written_2/travel_guides/berlitz2/Vallarta-WhatToDo.txt:61
./written_2/travel_guides/berlitz2/Vallarta-WhereToGo.txt:111
```

Wow, this'll be really useful for arbitrarily seeing the number of matches in each group of files since the paths are ordered alphabetically.


## `-o, --only-matching`
```
Print  only  the  matched  (non-empty)  parts of a matching
line, with each such part on a separate output line.
```

Interesting! We can declutter the screen a bit when looking for common patterns.

In fact, it'll allow us to more easily see all kinds of matches that can fit a RegEx pattern.

For this, we'll have to be more inventive with our RegEx. Luckily, I have a bit of experience with it.

Let's look for...
*Words that alternate between letters and vowels.*

We can do this by looking for words with many of the group `(\w[aeiou])`, which simply looks for a word character (`\w`) followed by any vowel (`[aeiou]`).

There are a lot of words like that, so we look for one that's surrounded by whitespace (`\s`) and that has more than 6 of the group (`{6,}`).

All together, we get the pattern `'\s\(\w[aeiou]\)\{6,\}\s'`, with escape characters included for the parentheses.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/Week 3/docsearch (main)
$ grep -r -o '\s\(\w[aeiou]\)\{6,\}\s' ./written_2/
./written_2/non-fiction/OUP/Berk/ch7.txt: manipulative 
./written_2/non-fiction/OUP/Kauffman/ch1.txt: degenerative 
./written_2/travel_guides/berlitz1/HistoryFrance.txt: remilitarize 
./written_2/travel_guides/berlitz1/HistoryHawaii.txt: Liliuokalani
./written_2/travel_guides/berlitz1/WhatToJapan.txt: Wakakusayama 
./written_2/travel_guides/berlitz1/WhereToJapan.txt: Sakuranomiya 
./written_2/travel_guides/berlitz1/WhereToJapan.txt: Sakuranomiya
```

Wow! These are some neat words! `remilitarize`? `degenerative`? Of course, since Japanese words follow a pattern like this, we get `Sakuranomiya`.

This is fun, and it doesn't clutter our screen! Let's try another one.

...Apparently, `\d` for digit doesn't work on Git Bash. I have no clue why, but the easy solution is to just look for any digit in `[0123456789]`.

`Athens-WhatToDo.txt` has a few codes formatted `##-#######`. Let's get them.

Our pattern is `\([0123456789]\)*-\([0123456789]\)\{7,\}` with escape characters included.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/Week 3/docsearch (main)
$ grep -r -o '\([0123456789]\)*-\([0123456789]\)\{7,\}' ./written_2/
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-9694500
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-7282333
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-3232771
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-7227233
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-3225904
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-3244395
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-8960341
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-7720201
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-7290731
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-3235560
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-4515731
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-9846820
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-2324555
./written_2/travel_guides/berlitz2/Athens-WhatToDo.txt:01-6834060
```

Wow! There's all the numbers. Too bad we can't really use them.

But what if we could? That would be bad... but that's just the power of `grep`ing big databases.