itunesterms
===========

This project contains static terminology, and related documentation,
for iTunes for use with appscript. This allows the continued use of
appscript with iTunes 10.6.3 (which has the same terminology as
10.6.1, but appscript and ASDictionary can no longer retrieve it).

If you're affected by the change in iTunes 10.6.3, please consider
filing a bug at http://bugreport.apple.com (see the OpenRadar bug
under links for an example), to increase the changes of Apple doing
something about it.

tunes.py
========

Used for py-appscript, as follows:

    import appscript
    import tunes
    itunes = appscript.app('iTunes', terms=tunes)

Of course you can also use the other constructor forms (by bundle ID, 
etc.); just add the "terms" keyword parameter to the end.

tunes.rb
========

Used for rb-appscript, as follows:

    require `rubygems`
    require `appscript`
    require `tunes`
    itunes = Appscript.app.by_name("iTunes", Tunes)
    
As with Python, you can use the other constructor forms as well.

objc-appscript/glues/ITGlue
===========================

Used for objc-appscript, following the usual directions.

applescript, objc-appscript, py-appscript, rb-appscript
=======================================================

Documentation in text, single-page html, and frame html, as generated
by ASDictionary.

Background
==========

As described in Dr. Drang's blog post "The first nail in the coffin of
Python appscript" (and blog commnents by appscript author has, among
others), iTunes 10.6.3 no longer responds properly to the ascr/gdte
event to 'get dynamic terminology', which means that, with iTunes
10.6.3:

 * py-appscript and rb-appscript can no longer find terms dynamically.
 * py-appscript and rb-appscript can no longer generate static glue.
 * ASDictionary can no longer generate static glue.
 * ASTranslate can no longer translate AppleScript to other languages.
 * AppleScript can no longer talk to iTunes remotely (via eppc).
 * ScriptingBridge can no longer talk to iTunes remotely.

There are other mechanisms for getting terminology, but they all have
problems.

OSAGetAppTerminology is deprecated, and does not work in 64 bit
(although it is used in versions of py-appscript before 0.22, which
may still work, at least on older/32-bit systems).

OSACopyScriptingDefinition (legacy, but not deprecated) and the sdef
command-line tool both retrieve terminology in "sdef" format rather
than "aete" format. This is sufficient for local AppleScript,
AppleScript Editor, Script Debugger, and ScriptingBridge connections,
and for the "sdef|sdp" method of generating static glue for
ScriptingBridge, but would require a rework of appscript to solve the
problem. 

Matt Neuburg has written a tool for Ruby called
sdefToRBAppscriptModule that can generate glue on the fly via the sdef
mechanism for rb-appscript; so far, no such tool exists for
py-appscript, or for generating objc-appscript glue, but it shouldn't
be too much harder.

In the mean time, the simplest solution is simply to get terminology
from an earlier version of iTunes. This will cause problems if iTunes
ever updates its terminology, but it's been a long time since they've
made any changes, and even longer since they've made any breaking
changes. So, the terminology from 10.6.1, or 9.2, will work fine with
10.6.3.

The terminology was generated with ASDictionary 0.13.1, py-appscript
1.0.0, and rb-appscript 0.6.1. Since the py-appscript and rb-appscript
terminology modules aren't often used by many appscript developers,
here's how to use them:

    from appscript.terminology import dump
    dump('/Applications/iTunes.app', 'tunes.py')

If you're using older versions of appscript (before 0.22 in Python,
unknown in Ruby), dump uses OSAGetAppTerminology, which means it does
not work in 64-bit builds, so you will have to run your interpreter in
32-bit mode.

Links
=====

 * http://appscript.sourceforge.net
 * http://www.leancrew.com/all-this/2012/06/the-first-nail-in-the-coffin-of-python-appscript/
 * http://www.apeth.com/rbappscript/04appobject.html
 * http://openradar.appspot.com/radar?id=1788405
