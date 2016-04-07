# gcc-makedepend

This standalone Perl script provides the functionality of
[_makedepend_](https://en.wikipedia.org/wiki/Makedepend)
on base of the `-MM` option of _gcc_.

## Synopsis

__gcc-makedepend__ {__-p__ _prefix_} [_gcc or g++ options_] {_source_}

Or, within a _makefile_, allowing you to update it with the
command `make depend`:

    .PHONY:         depend
    depend: ;       gcc-makedepend $(CPPFLAGS) $(wildcard *.c)

## Description

_gcc-makedepend_ works like _makedepend_ but is based upon the
`-MM` option of _gcc_. This has the advantage that all standard
include directories are considered including those which are not
known to _makedepend_.

_gcc-makedepend_ updates either _makefile_ or _Makefile_,
whatever is found first, by adding or updating the actual
list of header file dependencies of all given sources.

Like _makedepend_, _gcc-makedepend_ generates and honors the

    # DO NOT DELETE

comment line within the to-be-updated makefile, i.e. if this
line is found, all makefile lines behind it are considered to
be a to-be-updated list of dependencies, and, if this line
is not yet present, it will be generated together with the
newly generated dependencies. In consequence, neither this
line nor any makefile contents beyond this line should be touched
except by _gcc-makedepend_.

The option `-p` allows to specify a prefix that will be
prepended to all dependency lines output by _gcc_. This
is useful if the target directory for output files is not
the current directory. If multiple prefix options are given,
each output line by _gcc_ will be repeated for each prefix
with the individual prefix applied.

## History

This is not the first attempt to develop a _gcc_-based replacement of
_makedepend_. Another solution named
[makedependgcc](http://www.coppit.org/code/makedependgcc)
was previously developed by
[David Coppit](http://www.coppit.org)
which, however, diverts from the original
_makedepend_ invocation line, provides no error recovery with the danger
of corrupting the to-be-updated makefile, and which does not come with
a manual page. _makedependgcc_ gave, however, the insight that the
`-M` option of _gcc_ can be used to avoid _makedepend_.
