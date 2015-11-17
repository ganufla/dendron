# dendron
A simple tool for calculating frequency of patterns within dendrograms. Such calculation is performed using input patterns, stored in a file, and a set of dendrograms (replicas), stored in other file using newick format. Dendron's output is a table having input patterns and their frequencies.      

Input patterns may be sets, dendrograms or branches (dendrons). Such patterns are compared with those ones coming from the set of replicas using two methods: crispy and relaxed. Using 'crispy' method dendron search for exact matches of the given patterns throughout the replicas. When relaxed method is selected, then a relaxed measure of similarity is used for finding the most similar subpattern within each replica.

## Installation

Dendron is written in Common Lisp and source code needs to be compiled into an executable image file. The first thing we have to do is to install a Lisp interpreter. Dendron run nicely on [SBCL](http://www.sbcl.org). In a Debian system, SBCL is installed typing on a shell terminal:

> sudo apt-get install sbcl

If sbcl is properly installed, you can launch it writing `sbcl` on the terminal

> sbcl

Type (quit) or CTRL-d for quitting. 

Now, we need to install `buildapp`, which help us to make the executable file.

> sudo apt-get install buildapp

Next thing to do is to install the library manager for Lisp [quicklisp](http://www.quicklisp.org), by getting the file:
[http://beta.quicklisp.org/quicklisp.lisp](http://beta.quicklisp.org/quicklisp.lisp). Then you should type:

> sbcl --load quicklisp.lisp

Once you are in SBCL, type:
> *(quicklisp-quickstart:install)

and then:

> *(ql:add-to-init-file)

After that, we need to install the library [CFFI](https://common-lisp.net/project/cffi/) by typing in the sbcl REPL:

> *(ql:quickload "cffi")

Then type (quit) to exit SBCL. Now we are ready to compile our code. Type on the bash terminal:

> buildapp --load app.lisp --output dendron --entry dendron:support-patterns-from-file


## Usage     

> dendron --patterns FILE --replicas REP_FILE --output PREFIX --method METHOD <br/>&nbsp;&nbsp;--include-subdendrons --dendrons-as TYPE

### Options
`--dendron-as TYPE` 

When a dendron is found in the pattern file, use TYPE for comparison. TYPE might be one of the following values: `set`, `graph` or `both`. If the value of TYPE is `set`, then dendrons are treated as sets. If TYPE is `graph`, then they are treated as graphs (compare as drawings). If TYPE is `both` (default), then frequencies are calculated both as sets and as graphs.                                                   

`--help`
 
 Display this help.

`--include-subdendrons`

Recursively calculate frequency of subdendrons of each dendron
in FILE. Useful for calculating the frequency of a whole dendrogram and all its branches.Might be time consuming for large dendrograms, especially using graph comparison.

`--method METHOD`

METHOD may be one of:
* `crispy`, calculate frequency of patterns using the crispy comparison.
* `relaxed`, calculate frequency of patterns using a relaxed measure.
* `all`, calculate frequency of patterns using all measures (default).

`--output OUTPUT`

OUTPUT is the name of the output file. It stores all input patterns (see `--paterns`) and their corresponding frequency in a table. Fields are separated by a TAB character.

`--patterns FILE`

Patterns are read from file FILE, one per line. Patterns might be of the type `set` or `dendron`. Sets are lists of elements separated by spaces and enclosed by `(` and `)`. Dendrons are newick formated dendrograms or branches (see examples). Elements should not have any of the following characters: SPACE, TAB, LINEFEED, CARRIAGE_RETURN, COLON, COMMA, SEMICOLON, APOSTROPHE and DOUBLE_QUOTES.

`--replicas REP_FILE`
Read REP_FILE to load dendrograms where to calculate frequency of patterns. One dendrogram per line in newick format.                                  

## Examples:   
> dendron --patterns example.tre --output example-output --method crispy-set --replicas rep.tre

To calculate frequency of patterns in example.tre using a crispy set comparison whith dendrograms
from the file rep.tre.

> dendron --replicas rep.tre --patterns example2.tre --output example2-out --method all --include-subdendrons

To calculate frequency of patterns in the file example.tre, including their branches and using all methods.
