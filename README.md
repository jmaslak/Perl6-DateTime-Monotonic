[![Build Status](https://travis-ci.org/jmaslak/Perl6-DateTime-Monotonic.svg?branch=master)](https://travis-ci.org/jmaslak/Perl6-DateTime-Monotonic)

NAME
====

DateTime::Monotonic - is a Never-Decreasing Time-Elapsed Counter

SYNOPSIS
========

    use DateTime::Monotonic;

    my $tm = DateTime::Monotonic.new;

    my $start = $tm.seconds;
    # Do something that takes time here
    my $end = $tm.seconds;

    say "Processing took at least {$end - $start} seconds";

DESCRIPTION
===========

DateTime::Monotonic is A Never-Decreasing Time-Elapsed Counter.

This means that for any given instance of `DateTime::Monotonic`, the time will always increment, never decrement, even if the system clock is adjusted.

On Linux, this will use a monotonic second counter that is independent of the time-of-day clock. This allows reasonably accurate time measurements independent of the system clock being changed.

On non-Linux hosts, this will simulate a monotonic second counter by treating negative time shifts between successive calls to `seconds` as if no time elapsed. It will continue to be impacted by time shifts forward.

ATTRIBUTES
==========

has-monotonic-support
---------------------

This is `True` on systems with monotonic counters that are supported by this module (currently just Linux). If true, you can measure time elapsed reasonably accurately even if the system clock is adjusted.

If this is `False`, the monotonic support is emulated, subject to the limitations described below under `seconds`.

METHODS
=======

seconds(-->Numeric:D)
---------------------

Returns relative time between this `seconds` call and the first `seconds` call of this instance of `DateTime::Monotonic`.

On the first call for an instance of `DateTime::Monotonic`, will return "time zero" which is the value `0`. Following calls to `seconds` will return the time elapsed (in seconds, including fractional seconds) since this "time zero".

On Linux systems, this will provide reasonably accurate time regardless of system clock adjustement.

On Non-Linux systems, this will provide an always-incrementing number which will not be accurate with adjustments. If the time-of-day clock would indicate time went backwards between a `seconds` call and the previous `seconds` call, this method will return the previous result (I.E. the number returned will be the same as if no time elapsed between calls). If time is adjusted forward between calls, this will return a value that appears to have caused more time to elapse than actually has elapsed - but it will always be in a forward direction.

This method is thread safe.

BUGS
====

While there are no known bugs, this module does not yet support non-Linux OSes fully. It will provide non-reversing time on those systems, but the module could be improved by adding additional OS support.

EXPRESSING APPRECIATION
=======================

If this module makes your life easier, or helps make you (or your workplace) a ton of money, I always enjoy hearing about it! My response when I hear that someone uses my module is to go back to that module and spend a little time on it if I think there's something to improve - it's motivating when you hear someone appreciates your work!

I don't seek any money for this - I do this work because I enjoy it. That said, should you want to show appreciation financially, few things would make me smile more than knowing that you sent a donation to the Gender Identity Center of Colorado (See [http://giccolorado.org/](http://giccolorado.org/). This organization understands TIMTOWTDI in life and, in line with that understanding, provides life-saving support to the transgender community.

If you make any size donation to the Gender Identity Center, I'll add your name to "MODULE PATRONS" in this documentation!

AUTHOR
======

Joelle Maslak <jmaslak@antelope.net>

COPYRIGHT AND LICENSE
=====================

Copyright © 2018 Joelle Maslak

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.

