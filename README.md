setop
-----

**Set** **op**erations in the UNIX shell!

Resting on the shoulders of giants like `grep`, `cat`, `sort`, `uniq`, `comm`, `diff`, `cut`, `awk`, and more.

## Installation

Using `npm`:

    $ npm install -g setop

..or, copy the [script](https://raw.githubusercontent.com/cdax/setop/master/bin/setop) to a file called `setop`, give it the proper permissions, and move it to somewhere in your PATH, like so:

    $ chmod u+x setop
    $ mv setop /usr/local/bin

## Usage

* [Membership Test](#is-member)
* [Equality Test](#equals)
* [Cardinality](#count)
* [Subset Test](#is-subset)
* [Union](#union)
* [Intersection](#inter)
* [Complement](#minus)
* [Symmetric Difference](#sym-diff)
* [Cartesian Product](#product)
* [Disjoint Sets Test](#are-disjoint)
* [Empty Set Test](#is-empty)
* [Minimum Element](#min)
* [Maximum Element](#max)

#### <a name="is-member"></a>Membership Test

    $ setop is-member <kwd> <set1> <set2> ... <setn>

>Tests whether the line `kwd` is present in the files `set1`, `set2`, ..., `setn`.

For example:

    $ setop is-member abc set-1.txt
    1
    $ setop is-member mno set-1.txt set-2.txt set-3.txt
    0
    $ setop is-member xyz set-*.txt
    1

`set-1.txt`

    abc
    def
    abc
    ghi

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

#### <a name="equals"></a>Equality Test

    $ setop equals <set1> <set2>

>Tests whether the unique lines in file `set1` are the same as the unique lines in `set2`.

For example:

    $ setop equals set-1.txt set-2.txt
    1
    $ setop equals set-1.txt set-3.txt
    0

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi


#### <a name="count"></a>Cardinality

    $ setop count <set1> <set2> ... <setn>

>Counts the number of unique lines in files `set1`, `set2`, ..., `setn` combined.

For example:

    $ setop count set-1.txt
    3
    $ setop count set-1.txt set-2.txt set-3.txt
    4
    $ setop count set-*.txt
    4

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

#### <a name="is-subset"></a>Subset Test

    $ setop is-subset <base> <set1> <set2> ... <setn>

>Tests whether all lines in file `base` are present in files `set1`, `set2`, ..., `setn` combined

For example:

    $ setop is-subset set-3.txt set-1.txt
    0
    $ setop is-subset set-3.txt set-2.txt set-3.txt
    1
    $ setop is-subset set-3.txt set-*.txt
    1

`set-1.txt`

    abc

    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

#### <a name="union"></a>Union

    $ setop union <set1> <set2> ... <setn>

>Displays all unique lines that are present in files `set1`, `set2`, ..., `setn` combined

For example:

    $ setop union set-1.txt set-2.txt
    abc
    def
    ghi
    $ setop union set-1.txt set-2.txt set-3.txt
    abc
    def
    ghi
    xyz
    $ setop union set-*.txt
    abc
    def
    ghi
    xyz

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

#### <a name="inter"></a>Intersection

    $ setop inter <set1> <set2>

>Displays all unique lines that are common to files `set1` and `set2`

For example:

    $ setop inter set-1.txt set-2.txt
    abc
    def
    ghi
    $ setop inter set-1.txt set-3.txt
    abc
    def
    ghi

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

#### <a name="minus"></a>Complement

    $ setop minus <set1> <set2>

>Displays all unique lines in file `set1` that are not present in file `set2`

For example:

    $ setop minus set-1.txt set-2.txt
    $ setop minus set-3.txt set-2.txt
    xyz

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

#### <a name="sym-diff"></a>Symmetric Difference

    $ setop sym-diff <set1> <set2> ... <setn>

>Displays all unique lines that are present in either files `set1` or `set2` ... or `setn`, but not in all of them

For example:

    $ setop sym-diff set-1.txt set-2.txt
    $ setop sym-diff set-1.txt set-2.txt set-3.txt
    xyz
    $ setop sym-diff set-*.txt
    xyz

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

#### <a name="product"></a>Cartesian Product

    $ setop product <set1> <set2>

>Displays the cartesian product of the unique lines from files `set1` and `set2`

For example:

    $ setop product set-1.txt set-2.txt
    ghi abc
    def abc
    abc abc
    ghi def
    def def
    abc def
    ghi ghi
    def ghi
    abc ghi
    $ setop product set-2.txt set-3.txt
    ghi abc
    def abc
    abc abc
    ghi def
    def def
    abc def
    ghi ghi
    def ghi
    abc ghi
    ghi xyz
    def xyz
    abc xyz

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

#### <a name="is-disjoint"></a>Disjoint Sets Test

    $ setop is-disjoint <set1> <set2>

>Tests whether there are any lines common to both files `set1` and `set2`

For example:

    $ setop is-disjoint set-1.txt set-2.txt
    0
    $ setop is-disjoint set-2.txt set-3.txt
    0
    $ setop is-disjoint set-3.txt set-4.txt
    1

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

`set-3.txt`

    abc
    xyz
    def
    ghi

`set-4.txt`

    mno
    pqr
    stu

#### <a name="is-empty"></a>Empty Set Test

    $ setop is-empty <set1> <set2> ... <setn>

>Tests whether there are any lines in files `set1`, `set2`, ..., `setn` combined

For example:

    $ setop is-empty set-1.txt
    0
    $ setop is-empty <(setop sym-diff set-1.txt set-2.txt)
    1

`set-1.txt`

    abc
    def
    ghi
    def

`set-2.txt`

    def
    ghi
    ghi
    abc

#### <a name="min"></a>Minimum Element

    $ setop min <set1> <set2> ... <setn>

>Displays the lexicographic minimum from among the lines in files `set1`, `set2`, ..., `setn` combined

For example:

    $ setop min set-4.txt
    mno
    $ setop min set-1.txt set-4.txt
    abc

`set-1.txt`

    abc
    def
    ghi
    def

`set-4.txt`

    mno
    pqr
    stu

#### <a name="max"></a>Maximum Element

    $ setop max <set1> <set2> ... <setn>

>Displays the lexicographic maximum from among the lines in files `set1`, `set2`, ..., `setn` combined

For example:

    $ setop min set-1.txt
    ghi
    $ setop min set-1.txt set-3.txt
    xyz

`set-1.txt`

    abc
    def
    ghi
    def

`set-3.txt`

    abc
    xyz
    def
    ghi


## Credits

Many of the bash one-liners that are part of this project were found at a [post on Peter Krumin's blog](//www.catonmat.net/blog/set-operations-in-unix-shell/). I've been using them for years and I finally decided to put it all together in one script, with easy-to-remember command names.

## Support

Please [open an issue](https://github.com/cdax/setop/issues/new) for support.
