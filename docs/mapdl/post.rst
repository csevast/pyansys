Pythonic Post-Processing
------------------------
You can post process using an active MAPDL session using the
``post_processing`` property of a ``Mapdl`` instance.  One
advantage of this approach is it integrates well with existing MAPDL
scripting or automation, but can also be carried out on result files
generated from other programs, including ANSYS Mechanical.

Ideally, you should post-process using the ``result`` property or by
reading the result file using ``pyansys.read_result``, but in some
cases it's advantageous to post-process using Mapdl. 


Examples
~~~~~~~~
Classically, one would request nodal results from MAPDL using the
``PRNSOL`` command.  For example:

.. code::

     POST1:
     PRNSOL, U, X
    
     PRINT U    NODAL SOLUTION PER NODE
    
      ***** POST1 NODAL DEGREE OF FREEDOM LISTING *****                            
     
      LOAD STEP=     1  SUBSTEP=     1                                             
       TIME=    1.0000      LOAD CASE=   0                                         
     
      THE FOLLOWING DEGREE OF FREEDOM RESULTS ARE IN THE GLOBAL COORDINATE SYSTEM  
     
        NODE       UX    
           1  0.10751E-003
           2  0.85914E-004
           3  0.57069E-004
           4  0.13913E-003
           5  0.35621E-004
           6  0.52186E-004
           7  0.30417E-004
           8  0.36139E-004
           9  0.15001E-003
     MORE (YES,NO OR CONTINUOUS)=


However, using the ``pyansys.Mapdl`` module, you can instead request the
nodal displacement with:

.. code:: python

    >>> mapdl.set(1, 1)
    >>> disp_x = mapdl.post_processing.nodal_displacement('X')
    array([1.07512979e-04, 8.59137773e-05, 5.70690047e-05, ...,
           5.70333124e-05, 8.58600402e-05, 1.07445726e-04])

You could also plot the nodal displacement with:

    >>> mapdl.post_processing.plot_nodal_displacement('X')


.. figure:: ./images/post_norm_disp.png
    :width: 300pt

    Normalized Displacement of a Cylinder from MAPDL


Selected Nodes
~~~~~~~~~~~~~~
The ANSYS database processes some results independently of if nodes or
elements are selected.  If you have subselected a certain component
and wish to also limit the result of a certain output
(i.e. ``nodal_displacement``), use the ``selected_nodes`` property to
get a mask of the currently selected nodes.

.. code::

    >>> mapdl.nsel('S', 'NODE', vmin=1, vmax=2000)
    >>> mapdl.esel('S', 'ELEM', vmin=500, vmax=2000)
    >>> mask = mapdl.post_processing.selected_nodes


Post Processing Object Methods
------------------------------
.. autoclass:: pyansys.post.PostProcessing
    :members:
