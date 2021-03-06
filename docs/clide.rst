Getting data out of CLIDE
=========================

Nicolas Fauchereau has written several helper functions in python, with
the aim of mimicking the functions that are available in R from sourcing
the ``common/clidesc.r`` file

These functions are contained in the file ``clidesc/common/clide.py``
which (for now and until we create a conda package and host it on
`binstar <https://binstar.org/nicolasfauchereau>`_) needs to be copied
over to the same ``common`` directory.

At the beginning of each Python script, one must **import these
functions** (the equivalent of calling source() in a R script)

This is done by entering these lines:

::

    import sys, os

    Local = False # set to True for testing purposes

    if local: 
        sys.path.append('../common') 
        from clide import * 
    else: 
        source_path = os.path.dirname(os.path.realpath(sys.argv[0]))
        sys.path.append(os.path.join(source_path, '../common'))
        from clide import * 
        conn = clide_open(base_path)

which will have for effect to **i)** make all the functions implemented
in clide.py available for calling from within the python script, **ii)**
parse the command line arguments and **iii)** establish the connection
to the CLIDE database (which will be contained in the conn object).

These functions, their call sign and outputs are briefly described
below: note that when returning a table (e.g. the result of a SQL query
to the clide database) this table is a `Pandas
DataFrame <http://pandas.pydata.org/pandas-docs/dev/dsintro.html#dataframe>`_
object, which is similar to (but more efficient than!) a R dataframe,
and makes slicing, columns or row selection, resampling etc extremely
convenient. When time-series are returned, the index of the dataframe
(i.e. the ‘rows’ identifier) is a `Pandas datetime
index <http://pandas.pydata.org/pandas-docs/dev/generated/pandas.DatetimeIndex.html>`_
object resulting from the conversion of the ``lsd`` field to a python
datetime object and setting it as the index for the DataFrame. The name
of the index is invariably ``timestamp``.
