RE2 is a fast, safe, thread-friendly alternative to backtracking regular expression engines like those used in PCRE, Perl, and Python.  It is a C++ library.

Backtracking engines are typically full of features and convenient syntactic sugar but can be forced into taking exponential amounts of time on even small inputs.  RE2 uses [automata theory](http://swtch.com/~rsc/regexp/regexp1.html) to guarantee that regular expression searches run in time linear in the size of the input.  RE2 implements memory limits, so that searches can be constrained to a fixed amount of memory.
RE2 is engineered to use a small fixed C++ stack footprint no matter what inputs or regular expressions it must process; thus RE2 is useful in multithreaded environments where thread stacks cannot grow arbitrarily large.

On large inputs, RE2 is often much faster than backtracking engines; its use of automata theory lets it apply optimizations that the others cannot.

Unlike most automata-based engines, RE2 implements almost all the common Perl and PCRE features and syntactic sugars.  It also finds the leftmost-first match, the same match that Perl would, and can return submatch information.  The one significant exception is that RE2 drops support for backreferences¹ and generalized zero-width assertions, because they cannot be implemented efficiently.  The [syntax page](Syntax.md) gives full details.

For those who want a simpler syntax, RE2 has a POSIX mode that accepts only the POSIX egrep operators and implements leftmost-longest overall matching.

To get started writing programs that use RE2, see the <a href='http://code.google.com/p/re2/wiki/CplusplusAPI'>C++ API description</a>.
For details about the implementation, see “<a href='http://swtch.com/~rsc/regexp/regexp3.html'>Regular Expression Matching in the Wild</a>.”



<br><br>
<table cellpadding='0' width='611' cellspacing='0'><tr><td width='100%'>
<a href='http://xkcd.com/208/'><img src='http://re2.googlecode.com/hg/doc/xkcd.png?r=4816e3cbb232eec1747dceb6c72da4ca8534b14e' /></a>
</td></tr>
<tr><td align='right'>
(Image and logo above are from <a href='http://xkcd.com/208/'><a href='http://xkcd.com/208/'>http://xkcd.com/208/</a></a>.)<br>
</td></tr>
</table>
<br><br>

¹ Technical note: there's a difference between submatches and backreferences.  Submatches let you find out what certain subexpressions matched after the match is over, so that you can find out, after matching <code>dogcat</code> against <code>(cat|dog)(cat|dog)</code>, that <code>\1</code> is <code>dog</code> and <code>\2</code> is <code>cat</code>.  Backreferences let you use those subexpressions <i>during</i> the match, so that <code>(cat|dog)\1</code> matches <code>catcat</code> and <code>dogdog</code> but not <code>catdog</code> or <code>dogcat</code>.<br>
<br>
RE2 supports submatch extraction, but not backreferences.<br>
<br>
If you absolutely need backreferences and generalized assertions, then RE2 is not for you, but you might be interested in <a href='http://blog.chromium.org/2009/02/irregexp-google-chromes-new-regexp.html'>irregexp</a>, Google Chrome's regular expression engine.