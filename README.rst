=========================================
Jayaraman lab's importer code for Ovation
=========================================


This project contains Matlab [#]_ code for importing data for Vivek Jayaraman's lab into the `Ovation Scientific Data Management System <http://physionconsulting.com/web/Ovation.html>`_.

The importer is modularized into individual Matlab functions that import components of the Jayaraman data:

- ``insertXSGEpoch`` and ``appendXSG`` which insert a new ovation.Epoch for an XSG file or append an XSG file to an existing ``ovation.Epoch`` respectively
- ``appendScanImageTiff`` which appends a ScanImage TIFF's data to an existing ``ovation.Epoch``
- ``appendSeq`` which appends a .seq data file to an existing ``ovation.Epoch``

Documentation of the specification for each import module is available on the project `wiki <https://github.com/physion/jayaraman-importer/wiki>`_.

Coordinating method(s) to import an entire experiment's data have not been written yet.

Basic Usage
-----------

To use the importer:

#. Add the ``src/`` directory to the Matlab path
#. Add the ``yamlmatlab/`` directory to the Matlab path
#. Choose an Ovation ``Experiment`` object to insert data into. To create a ``Project`` and ``Experiment``::

    >> import ovation.*
    >> context = NewDataContext(<path_to_connection_file>, <username>);
    >> project = context.insertProject(<project name>, <project purpose>, <project start date>);
    >> experiment = project.insertExperiment(<expt purpose>, <expt start date>);
#. Insert an ``EpochGroup``, replacing ``<label>`` with the desired ``EpochGroup`` label and the arguments to ``datetime`` with the start time and date of the ``EpochGroup`` (see ``help ovation.datetime``)::

    >> epochGroup = experiment.insertEpochGroup(<label>, datetime(...));
    
#. Insert an Epoch::

    >> epoch = insertXSGEpoch(epochGroup,...
                        xsgPath,...     % path to XSG file
                        protocolID,...  % protocolID, e.g. 'org.hhmi.janelia.jayaraman.<my protocol name>'
                        timezone)       % timezone string (e.g. 'America/New_York')

#. Append ScanImage data::

    >> appendScanImageTiff(epoch, tifFilePath)
    
#. Append Seq data::

    >> appendSeq(epoch,...
                   seqFile,...              % Path to .seq file
                   yamlFile,...             % Path to YAML metadata file
                   timeZone)                % timezone string (e.g. 'America/New_York')

Automated tests
---------------

To run the automated test suite:

#. Add the ``jayaraman-importer/src`` folder to the Matlab path
#. Add the ``yamlmatlab`` folder to the Matlab path
#. Add ``yamlmatlab/external/snakeyaml-1.9.jar`` to your java classpath via javaaddpath
#. Add Matlab xUnit (``jayaraman-importer/matlab-xunit-doctest/xunit``) to the Matlab path
#. From within the ``jayaraman-importer/`` directory::
    
    >> runtestsuite test
    




.. [#] Matlab is a registered trademark of The Mathworks, Inc..


