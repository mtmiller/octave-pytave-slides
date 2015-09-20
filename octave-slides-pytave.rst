=================
Octave and Python
=================

:author: Mike Miller
:date: September 21, 2015

.. footer:: OctConf - September 2015

-----------
Why Python?
-----------

* Python also a dynamic language that is easily extensible
* Python also extremely popular for scientific computing
* The new symbolic package uses Python's SymPy (over a pipe)
* There are already two Python modules for calling into Octave
* Matlab has added Python support [1]_, similar to Java interface

-----
Goals
-----

* Call Octave functions from Python

  * ``>>> x, = pytave.feval (1, "magic", 4)``

* Call Python functions from Octave

  * ``>> y = uint8 (py ("os.urandom", 1024));``

* Make an integral part of Octave core
* Make the syntax look completely *native* and *seamless*

* In Python:

  * ``>>> x = octave.magic (4); lam, v = scipy.linalg.eig (x)``

* In Octave:

  * ``>> y = uint8 (py.os.urandom (1024)); plot (y);``

---------------
Starting Points
---------------

* Pytave and Oct2Py are two existing modules for calling Octave
* Oct2Py is pure Python, interacts with Octave via a pipe

  * More popular, better supported, pip-installable [2]_

* Pytave embeds the Octave interpreter as a Python extension

  * Less popular because it needs to compile and track API changes,
    breaks down over time

* Use the Pytave approach, bring it up to current API, what next?

----------------------
Pytave Progress Update
----------------------

* Compiles and runs against Octave 4.0.0 and development branch
* Works with Python 2.7 and NumPy 1.9 API and data types
* Call any Octave builtin or user function on the load path
* Automatically converts common data types

  * All numeric values handled as NumPy matrices
  * Structs map to Python dictionaries
  * Char arrays map to Python strings, but not back to strings
  * Python lists map to cell arrays, but not back to lists

* Modify user path and load packages
* Preliminary support for calling Python from Octave…

----------
Demo Break
----------

---------------------------------
Calling Python - Work in Progress
---------------------------------

* Extremely preliminary — embedding Python interpreter and calling a
  function are relatively easy
* Currently eval()ing strings, not direct calls, not passing arguments
* Only handling one return value for now
* Test branch with function handles, also not passing arguments yet,
  but this is the way to go
* Needs to be able to distinguish between values (e.g. ``sys.version``)
  and callables (e.g. ``socket.socket``)
* Needs to be able to handle unknown object types as opaque handles

-------------------------------
Overall Problems To Be Resolved
-------------------------------

* Uses boost::python, makes some tedious bits easy, but missing pieces
* Not yet compatible with Python 3 (API changes, Unicode strings, etc)
* Mismatch between Python and Octave multiple return values

  * In Python, function has *N* return values, caller must match
  * In Octave, caller implicitly indicates number of return values
  * For now, this is an extra argument when calling Octave from Python

* Considerations for matrices vs scalars vs strings (maybe)
* Hook into each language's dynamic nature, dispatch names automatically
  so ``py ("os.urandom", 1)`` can become ``py.os.urandom (1)``

-------
Summary
-------

* Reusing existing Pytave project and extending to embed Python into
  Octave
* Brought Pytave up to date to work with current versions of Octave,
  Python, and NumPy
* Most of the mechanics are straightforward, may need some thought put
  in to type conversion
* Project is out there, will continue to develop [3]_
* Please try, send feedback, ideas, and patches

----------
References
----------

.. [1] Call Python From Matlab
   `<https://www.mathworks.com/help/matlab/matlab_external/call-python-from-matlab.html>`_
.. [2] oct2py on Python Package Index
   `<https://pypi.python.org/pypi/oct2py>`_
.. [3] hg clone `<https://hg.mtmxr.com/pytave>`_
