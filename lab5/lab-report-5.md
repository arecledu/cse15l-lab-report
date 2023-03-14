# Researching Commands (Part 2)

We're doing Lab Report 3 again, but with a different command for Lab Report 5!

We'll be looking at the `sed` and exploring its options.

We'll also be using `|` (pipe), `curl`, and other commands, including `sed`'s own script commands, to mess around with `./written_2`.

Again, we'll be looking at the manual [online](https://linuxcommand.org/lc3_man_pages/sed1.html) for `sed`: 

https://linuxcommand.org/lc3_man_pages/sed1.html

## `-i[SUFFIX], --in-place[=SUFFIX]`
```
edit files in place (makes backup if SUFFIX supplied)
```

Sounds like we won't have to output sed to the same file name with `>`.

Let's try it out on a random file in `./written_2`. We'll be using `sed`'s `s/regexp/replacement` command a lot.

Let's replace each vowel in `written_2/non-fiction/OUP/Abernathy/ch1.txt` with `e`. We can do that with the script `s/[aeiou]/e/g`.


```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ sed -i 's/[aeiou]/e/g' written_2/non-fiction/OUP/Abernathy/ch1.txt

ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ cat written_2/non-fiction/OUP/Abernathy/ch1.txt




In the lete 1940s, Bend Steres, the lergest men’s cletheng cheen et the teme, creeted e senseteen en New Yerk Cety by effereng e wede selecteen ef seets weth twe peers ef pents ensteed ef ene, reentredeceng e level ef predect cheece net seen sence befere the wer.1 When the lene ef hepefel beyers et ets Temes Sqeere stere stretched ereend the bleck, Bend hed te empese e lemet ef twe seets per cestemer. Dereng Werld Wer II, the epperel end textele endestrees hed been cenverted te sepply feeld jeckets, everceets, end eneferms te the U.S. end Alleed Ferces.

...

```

That's interesting! Looks like the result doesn't print out when executing `sed`, but we can see that the file has been modified with `cat`.

Let's try another example by printing the line number before each line in another file. We can do that with `=` as a `sed` script.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ sed -i '=' written_2/non-fiction/OUP/Abernathy/ch2.txt

ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ cat written_2/non-fiction/OUP/Abernathy/ch2.txt
1

2

3

4

5
The emergence of textile, apparel, and retail enterprises in the United States is full of fascinating twists. In 1790, for instance, an act of industrial espionage is said to have launched the domestic textile industry, if not American manufacturing in general. At that time, Samuel Slater, a skilled mechanic, built the first successful water-powered yarn spinning mill in Pawtucket, Rhode Island. Yarn was in short supply in the new country and much in demand in households that did hand weaving as well as in workplaces with looms that produced sheeting, shirting, and stockings for commerce. Some of the American states and improvement societies had even offered generous rewards for the establishment of water-powered combing and spinning, especially those based on state-of-the-art English Arkwright operations. But British law strictly prohibited the export of drawings, plans, or models of these new technologies. It took somebody like Slater—an indentured apprentice for over six years at the Arkwright and Strut’s plant in Milford, England—to ferry the plans to America.1
6



...
```

Yep. That works! There's line numbers even for the extra whitespace at the top of the file.

## `-f script-file, --file=script-file`
```
add  the contents of script-file to the commands to be executed
```

Looks like we can externalize our `sed` script instead of putting it in the command line!

Let's make a file with `nano` and call it `sed_test.txt`:
```
s/[lrv]/w/g
s/\ws/z/g
s/\./. :3 /g
s/c/k/g
```

And let's run it on `written_2/non-fiction/OUP/Abernathy/ch3.txt`!

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ nano sed_test.txt

ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ sed -f sed_test.txt written_2/non-fiction/OUP/Abernathy/ch3.txt




In 1911, John Wanamakew opened hz fwazhip stowe in downtown Phiwadewphia. :3  The twewwe-stowy buiwding, with iz fowty-fiwe akwz of fwoow spake, wz the wawgzt of iz time dewoted to wetaiw mewkhandzing. :3  Iz kentwaw “Gwand Couwt” had mawbwe awkhz that wze 150 feet and wz kapped by a dome. :3  Majow phzikaw innowatioz wewe hidden behind thz wzuaw wondew: sixty-eight state-of-the-awt ewewatoz; the watzt in fiwepwoofing; a wawge powew pwant dewoted entiwewy to the stowe; and sophztikated heating, wentiwating, and sanitation sztez. :3


...
```

It works! It's scary how powerful `sed` scripts can be...

Let's try a different example... `sed_test2.sed`:

```
/[t|T]he/d
/\w/iWow! This sentence doesn't have the most common word:
```

This script removes all lines that have the word `the` or `The` and, for the remaining lines, inserts the line `Wow! This sentence doesn't have the most common word:` before them.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ nano sed_test2.sed

ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ sed -f sed_test2.sed written_2/non-fiction/OUP/Abernathy/ch3.txt




Wow! This sentence doesn't have the most common word:
Product Proliferation
Wow! This sentence doesn't have the most common word:
Retail Overcapacity
Wow! This sentence doesn't have the most common word:
—William Howell, Chairman, J.C. Penney, 1995
Wow! This sentence doesn't have the most common word:
Mass Merchants: Wal-Mart
Wow! This sentence doesn't have the most common word:
National Chains: J. C. Penney
Wow! This sentence doesn't have the most common word:
Department Stores: Dillard’s and Federated
```

That's not a lot of lines! Looks like `the` really *is* the most common word...

## `--debug`

Interestingly, this option isn't on the `man` page but can be seen in the [GNU `sed` manual](https://www.gnu.org/software/sed/manual/html_node/Command_002dLine-Options.html#Command_002dLine-Options):

https://www.gnu.org/software/sed/manual/html_node/Command_002dLine-Options.html#Command_002dLine-Options

```
Print the input sed program in canonical form, and annotate program execution.
```

This seems interesting! Let's try it.

We'll be using a `sed` script that changes all `a`'s to uppercase, and we'll pipe (`|`) the output to `less` so we can see the top of the output.

Here, I've skipped a few lines for conciseness.

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ sed --debug 's/a/A/g' written_2/non-fiction/OUP/Abernathy/ch7.txt | less
```

```
SED PROGRAM:
  s/a/A/g
INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 1
PATTERN:
COMMAND: s/a/A/g
PATTERN:
END-OF-CYCLE:


...


INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 5
PATTERN:
COMMAND: s/a/A/g
PATTERN:
END-OF-CYCLE:

INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 6
PATTERN:         Oahu (Including Honolulu)
COMMAND: s/a/A/g
MATCHED REGEX REGISTERS
  regex[0] = 9-10 'a'
PATTERN:         OAhu (Including Honolulu)
END-OF-CYCLE:
        OAhu (Including Honolulu)
INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 7
PATTERN:         Aston Waikiki Sunset $$$ 229 Paoakalani Avenue, Honolulu, HI
COMMAND: s/a/A/g
MATCHED REGEX REGISTERS
  regex[0] = 15-16 'a'
PATTERN:         Aston WAikiki Sunset $$$ 229 PAoAkAlAni Avenue, Honolulu, HI
END-OF-CYCLE:
        Aston WAikiki Sunset $$$ 229 PAoAkAlAni Avenue, Honolulu, HI


...
```

As we can see, the `--debug` option allows us to see how `sed` works with each line, even if the script doesn't directly deal with the line.

The whitespace at the top of the file doesn't have a pattern for the input nor a pattern for the output of the command.

However, as can be seen in line 6, the original line is labeled as an input pattern:

```
INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 6
PATTERN:         Oahu (Including Honolulu)
``` 

`sed` runs the script on the line and returns any RegEx matches, as well as the final output line (as a pattern):

```
COMMAND: s/a/A/g
MATCHED REGEX REGISTERS
  regex[0] = 9-10 'a'
PATTERN:         OAhu (Including Honolulu)
END-OF-CYCLE:
        OAhu (Including Honolulu)
```

The matched RegEx registers here shows a match with the characters in the range of indices 9-10, which is the letter `a` in `Oahu`.

Let's try another example.


Here, we'll be removing every line with the letter `a`.
```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ sed --debug '/a/d' written_2/travel_guides/berlitz1/HandRHawaii.txt | less
```

```
SED PROGRAM:
  /a/ d
INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 1
PATTERN:
COMMAND: /a/ d
END-OF-CYCLE:


...


INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 5
PATTERN:
COMMAND: /a/ d
END-OF-CYCLE:

INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 6
PATTERN:         Oahu (Including Honolulu)
COMMAND: /a/ d
END-OF-CYCLE:
INPUT:   'written_2/travel_guides/berlitz1/HandRHawaii.txt' line 7
PATTERN:         Aston Waikiki Sunset $$$ 229 Paoakalani Avenue, Honolulu, HI
COMMAND: /a/ d
END-OF-CYCLE:


...
```

As can be seen here, even though the lines are meant to be removed, the original lines still appear with `--debug`, but, *unlike* the `s` replacement command, using `d` to delete the line immediately returns `END-OF-CYCLE:` instead of a resulting pattern.

## `-l N, --line-length=N`
```
specify the desired line-wrap length for the `l' command
```

Lastly, `-l N` seems to give us a way to adjust line-wrap, but it only works specifically for the `l` command, which, according to the aforementioned GNU `sed` manual, "Print[s] the pattern space in an unambiguous form."

For our script, we'll be replacing every word letter (`\w`) with `A` and every period with an exclamation point.

We'll do `50` characters for our argument:
```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ sed -nl 50 's/\w/A/g;s/\./!/g;l' written_2/non-fiction/OUP/Abernathy/ch3.txt
$
$
$
$
AA AAAA, AAAA AAAAAAAAA AAAAAA AAA AAAAAAAA AAAAA\
 AA AAAAAAAA AAAAAAAAAAAA! AAA AAAAAA-AAAAA AAAAA\
AAA, AAAA AAA AAAAA-AAAA AAAAA AA AAAAA AAAAA, AA\
A AAA AAAAAAA AA AAA AAAA AAAAAAA AA AAAAAA AAAAA\
AAAAAAAA! AAA AAAAAAA \342\200\234AAAAA AAAAA\342\
\200\235 AAA AAAAAA AAAAAA AAAA AAAA AAA AAAA AAA\
 AAA AAAAAA AA A AAAA! AAAAA AAAAAAAA AAAAAAAAAAA\
 AAAA AAAAAA AAAAAA AAAA AAAAAA AAAAAA: AAAAA-AAA\
AA AAAAA-AA-AAA-AAA AAAAAAAAA; AAA AAAAAA AA AAAA\
AAAAAAAA; A AAAAA AAAAA AAAAA AAAAAAA AAAAAAAA AA\
 AAA AAAAA; AAA AAAAAAAAAAAAA AAAAAAA, AAAAAAAAAA\
A, AAA AAAAAAAAAA AAAAAAA!$
AAAAAAAAA AAA AAAA AA AAA AAAAAAAAA AA AAAAAAAAA \
AAA AAAA AAAA AAAAAA AAAAA AA AAA AAAA AA AAAAAA \
AAAA AAAAA! AAA AAAA AAA AA AAAAAAA AAA AAAAAAA A\
AAAAAAA AAAAAAAAAA AA AAAAA AAAAAAAA AAAAAAAAA AA\
AAA AAAAAAAAAA AAA AAAAAAAA AAAAAA AAA AAAAAAA AA\
AAAAAAA! AA AAA AAAA AAAA, AA AAAAA AAA AAAAAAAAA\
 AAAAAA AA AAAA AAAAAAAAAAA AAAAAAAAAA: AAA AAAAA\
 AAA A AAAAAAA (AA AAAAAAAA); AAAAAA AAAAAAAAAA A\
A AA \342\200\234AA AAAAAAA AAAAA AAAA AAA AAAAAA\
 AAAAAAAAA\342\200\235; AAAAAAAAAA AA AAAA AAAAAA\
AA AAAA, AA AAAAA AA AAAA AAAAAA AAA; AAA AAAA AA\
AAAAA AA AAAAAAAAA AAA AAAAAAAAAAA AAAAAAAAA! AAA\
A AAAAA AAAAAAAAAAA AAAAAAAAAA AA AAAA, AA AAAAAA\
 AAA AA AAA AAAAAAA AAAAAAAAA AA AAA AAA!A$
AAAAA AAAAA AAAAA AAA AAAAAAA AA AAAAAAAAA\342\
\200\231A AAAAAAAAAAAA AAAAA, AAAAAAA AAAAAAAAAAA\
A AAAAAAAAAAA A AAA AA AAAAAAAA AAAAAAAAAAAA AAA,\
 AAAAA AAAA A AAAAAA AA AAAAA AAAAAAAAA, AAAAA AA\
AAAAA AAAAAAAAAA AA AAA AAAAAAAA! AAA AAAAAA AAAA\
AAA AAAAA AA AAA AAAAA, AAA AAA-AAAA AAAAAAA AAAA\
AA AAA AAAAAAA AAAAAAAA AA AAA AAAAAA AAAAAA, AAA\
A AAAAA AAAAA AA AAAAAA AAAA AAAA AAAAAAAA AAA AA\
AAAAAA AAAAA AAAAAAA AA AAAAA, AAAAA AAAAAAA AAA \
AA!, AAA AAA AAAAAAAAAAA AAAAA AAA AAAAAA AA!, AA\
A AAAA AAAAA AAAAAAA AAAAAAAA AAAAAA AAAAAAAAAAAA\
A! AAAAAA\342\200\231A AAAAAAAAAA AAAAAAAAAAA AAA\
AAA AAAAAAAA AAAAAAAA AA AAAAA AAAA AAAAAAAA AAAA\
AAAAA AA AAAAA AAAAA AAAAAAAAA AAAAA AAAAAAA AAAA\
A! AA AAA AAAA AAAAA, A AAAAAAA AAAAAA AA AAAAAAA\
AA AAA AAAAAAA AAAAAAAA AAA AAA AAAA AAA AAAAAAAA\
!$

...
```

Y'know, this output reminds me of an old Wiki [article](https://en.uncyclopedia.co/wiki/AAAAAAAAA!)...

Yep. That's `50` characters per line, making a pretty neat rectangle of moderate screaming... and some `UTF-8` encoding.


Let's try one more thing. Since `-l N` only works for the `l` command in `sed`, let's try printing the pattern normally with the `p` command in `sed` and following it up with `l`.

Here, we're turning a text into an SCP Foundation article (not to be confused with the `scp` command) by replacing words starting with vowels with a bunch of full block (`█`) characters:

```
ambro@LAPTOP-UUG30S5P MINGW64 ~/Documents/Homework/CSE15L/lab-report-repo/cse15l-lab-report/lab5/test/docsearch (main)
$ sed -l 60 's/ [aeiou]\w*/ █████/g;p;l' written_2/non-fiction/OUP/Abernathy/ch7.txt | less
```

```
$


$


$


$
Retailers’ calls to █████ manufacturers █████ late delivery █████ the basis for many █████ tall tale █████ retail conventions. In the past, the standard reply to █████ query █████ what had happened to █████ █████ was “It’s █████ the loading dock.” Information systems █████ █████ factories were primitive. If █████ the SKUs for █████ █████ were not █████ the warehouse, substitutions █████ the same style █████ █████ different size would be █████ to the retailer. Or retailers might not █████ notice █████ █████ █████ substitution had been made because their █████ systems were █████ █████ primitive. If there were █████ SKUs █████ the requested style, the █████ would be shorted █████ █████ phone call made to the retail buyer to negotiate █████ solution to the problem. If no SKUs █████ the █████ were █████ the finished goods warehouse, then the search █████ the factory floor—where there might be tens █████ thousands █████ partially completed █████ to look through—would begin.
Retailers\342\200\231 calls to \342\226\210\342\226\210\342\
\226\210\342\226\210\342\226\210 manufacturers \342\226\210\
\342\226\210\342\226\210\342\226\210\342\226\210 late deliv\
ery \342\226\210\342\226\210\342\226\210\342\226\210\342\
\226\210 the basis for many \342\226\210\342\226\210\342\
\226\210\342\226\210\342\226\210 tall tale \342\226\210\342\
\226\210\342\226\210\342\226\210\342\226\210 retail convent\
ions. In the past, the standard reply to \342\226\210\342\
\226\210\342\226\210\342\226\210\342\226\210 query \342\226\
\210\342\226\210\342\226\210\342\226\210\342\226\210 what h\
ad happened to \342\226\210\342\226\210\342\226\210\342\226\


...
```

As we can see, the normal output by `p` doesn't wrap, but the output by `l` does.

Additionally, we can see that `l` doesn't actually have the full block character itself because it's *unambiguous*. `\342\226\210` corresponds to the full block character (`█`) in [`UTF-8` encoding (octal)](https://www.unicodepedia.com/unicode/block-elements/2588/full-block/), and we can see that in abundance here.