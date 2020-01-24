Usage per version
=================

`Common Options amongst versions`_

`cadcdata - 1.3.1 (Current)`_
  `Usage for GET`_

  `Usage for PUT`_

  `Usage for INFO`_

`cadcdata - 2.0.0 (Proposed)`_
    `Usage for CP`_

    `Usage for DELETE`_


Common Options amongst versions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. table:: Common Optional Arguments

    =============================================== =============================================
    Option                                          Description
    =============================================== =============================================
    ``[-h | --help]``                               Show the help message
    ``[-d | --debug]``                              Enable DEBUG messages
    ``[-v | --verbose]``                            Verbose messages
    ``[-q | --quiet]``                              Silent all output
    ``[--cert CERT]``                               Location of your X509 certificate to use for authentication (unencrypted, in PEM format)
    ``[--host HOST]``                               Base hostname for services - used mainly for testing but useful for specifying alternate Registry (default: www.cadc-ccda.hia-iha.nrc-cnrc.gc.ca).  Uses ``https``.
    ``[--resource-id RESOURCE_ID]``                 Resource service identifier to lookup service to use (default ivo://cadc.nrc.ca/data)
    ``[-n]``                                        Use .netrc in $HOME for authentication
    ``[--netrc-file NETRC_FILE]``                   NetRC file to use for authentication
    ``[(-u | --user) USER]``                        Name of user to authenticate. Note: application prompts for the corresponding password!
    =============================================== =============================================

cadcdata - 1.3.1 (Current)
==========================

.. image:: https://img.shields.io/pypi/v/cadcdata.svg   
    :target: https://pypi.python.org/pypi/cadcdata

Canadian Astronomy Data Centre - data access

Access library and client for astronomical data hosted at the Canadian Astronomy Data Centre


Usage for ``GET``
~~~~~~~~~~~~~~~~~
Retrieve files from a CADC archive

usage:  ``cadc-data get COMMON-OPTIONS GET-OPTIONS archive filename``

.. table:: GET Positional Arguments

   ================= =============================================
   \                 Description
   ================= =============================================
   ``archive``       CADC archive
   ``filename``      The name of the file in the archive
   ================= =============================================


.. table:: GET Optional Arguments (``GET-OPTIONS``)

    ========================================= =============================================
    Option                                    Description
    ========================================= =============================================
    ``[[-o | --output] OUTPUT``               Space-separated list of destination files (quotes required for multiple elements)
    ``[--cutout [CUTOUT [CUTOUT ...]]]``      Specify one or multiple extension and/or pixel range cutout operations to be performed. Use cfitsio syntax
    ``[--nomd5]``                             Do not perform md5 check at the end of transfer
    ``[-z | --decompress]``                   Decompress the data (gzip only)
    ``[--wcs]``                               Return the World Coordinate System (WCS) information
    ``[--fhead]``                             Return the FITS header information
    ========================================= =============================================

Examples
^^^^^^^^

- Anonymously getting a public file:
    ``cadc-data get -v GEMINI 00aug02_002.fits``
- Use certificate to get a cutout and save it to a file:
    ``cadc-data get --cert ~/.ssl/cadcproxy.pem -o /tmp/700000o-cutout.fits --cutout [1] CFHT 700000o``
- Use default netrc file ($HOME/.netrc) to get FITS header of a file:
    ``cadc-data get -v -n --fhead GEMINI 00aug02_002.fits``
- Use a different netrc file to download wcs information:
    ``cadc-data get -d --netrc ~/mynetrc -o /tmp/700000o-wcs.fits --wcs CFHT 700000o``
- Connect as user to download two files and uncompress them (prompt for password if user not in $HOME/.netrc):
    ``cadc-data get -v -u auser -z GEMINI 00aug02_002.fits 00aug02_003.fits``


Usage for ``PUT``
~~~~~~~~~~~~~~~~~
Upload files into a CADC archive

Usage:  ``cadc-data put COMMON-OPTIONS PUT-OPTIONS archive source``

.. table:: PUT Positional Arguments

    ================= =============================================
    \                 Description
    ================= =============================================
    ``archive``       CADC archive
    ``source``        File or directory containing the files to be put
    ================= =============================================


.. table:: PUT Optional Arguments (``PUT-OPTIONS``)

    =========================================== ====================================================
    Option                                      Description
    =========================================== ====================================================
    ``[--nomd5]``                               Do not perform md5 check at the end of transfer
    ``[-t | --type]``                           MIME type to set.  Deduced by default
    ``[-e |  --encoding]``                      MIME encoding to set.  Deduced by default
    ``[-s | --archive-stream] ARCHIVE_STREAM``  Specific archive stream to add the file to (DEPRECATED)
    ``[-i | --input] INPUT``                    Space-separated list of input name to use in archive - overrides the actual file names in source. (quotes required for multiple elements) (DEPRECATED)
    =========================================== ====================================================

Examples
^^^^^^^^
- Use certificate to put a file in an archive stream under a different name :
    ``cadc-data put --cert ~/.ssl/cadcproxy.pem -as default -t "application/gzip" -i newfilename.fits.gz TEST myfile.fits.gz``
- Use default netrc file ($HOME/.netrc) to put two files:
    ``cadc-data put -v -n TEST myfile1.fits.gz myfile2.fits.gz``
- Use a different netrc file to put files from a directory:
    ``cadc-data put -d --netrc ~/mynetrc TEST dir``
- Connect as user to put files from multiple sources (prompt for password if user not in $HOME/.netrc):
    ``cadc-data put -v -u auser TEST myfile.fits.gz dir1 dir2``

Usage for ``INFO``
~~~~~~~~~~~~~~~~~~
Get information regarding files in a CADC archive in the form:

File:
^^^^^
	``-name``
	``-size``
	``-md5sum``
	``-encoding``
	``-type``
	``-usize``
	``-umd5sum``
	``-lastmod``

Usage:  ``cadc-data info COMMON-OPTIONS archive filename``

.. table:: INFO Positional Arguments

    ================= =============================================
    \                 Description
    ================= =============================================
    ``archive``       CADC archive
    ``filename``      The name of the file in the archive
    ================= =============================================


Examples
^^^^^^^^
- Anonymously getting information about a public file:
    ``cadc-data info GEMINI 00aug02_002.fits``
- Use certificate to get information about a file:
    ``cadc-data info --cert ~/.ssl/cadcproxy.pem CFHT 700000o``
- Use default netrc file ($HOME/.netrc) to get information about a file:
    ``cadc-data info -n GEMINI 00aug02_002.fits``
- Use a different netrc file to get information about a file:
    ``cadc-data info --netrc ~/mynetrc CFHT 700000o``
- Connect as user to get information about two files (prompt for password if user not in $HOME/.netrc):
    ``cadc-data info -u auser GEMINI 00aug02_002.fits 00aug02_003.fits``


cadcdata - 2.0.0 (Proposed)
===========================


Usage for ``CP``
~~~~~~~~~~~~~~~~
Retrieve files from the Storage System.

usage:  ``cadc-data cp COMMON-OPTIONS CP-OPTIONS source destination``


** **Note**: First version of ``cp`` is limited to operating on a single file.


.. table:: Positional Arguments

   ================= =============================================
   \                 Description
   ================= =============================================
   ``source``        [file] The source of the data copy to PUT
   ``source``        [uri] The URI of the file to GET
   ``destination``   [uri] The destination URI of the PUT
   ``destination``   [file | directory] The destination of the file to GET
   ================= =============================================

.. table:: Optional Arguments (``CP-OPTIONS``)

    ========================================= =============================================
    Option                                    Description
    ========================================= =============================================
    ``[--cutout [CUTOUT [CUTOUT ...]]]``      Specify one or multiple extension and/or pixel range cutout operations to be performed. Use cfitsio syntax
    ``[--nomd5]``                             Do not perform md5 check at the end of transfer
    ``[-z | --decompress]``                   Decompress the data (gzip only)
    ``[--wcs]``                               Return the World Coordinate System (WCS) information
    ``[--fhead]``                             Return the FITS header information
    ``[-t | --type]``                         MIME type to set.  Deduced by default
    ``[-e |  --encoding]``                    MIME encoding to set.  Deduced by default
    ========================================= =============================================

Examples
^^^^^^^^
- Anonymously GETting a public file: 
    ``cadc-data cp -v cadc:GEMINI/00aug02_002.fits .``

- Use certificate to GET the first extension and save it to a file:
    ``cadc-data cp --cert ~/.ssl/cadcproxy.pem --cutout [1] cadc:CFHT/700000o.fits.fz /tmp/700000o-cutout.fits``

- Use default netrc file ($HOME/.netrc) to GET FITS header of a file in the home directory:
    ``cadc-data cp -v -n --fhead cadc:GEMINI/00aug02_002.fits ~/``

- Use a different netrc file to download wcs information:
    ``cadc-data cp -d --netrc ~/mynetrc --wcs cadc:CFHT/700000o.fits.fz /tmp/700000o-wcs.fits``

- Connect as user to download a file and uncompress it (prompt for password if user not in $HOME/.netrc):
    ``cadc-data cp -v -u auser -z cadc:GEMINI/00aug02_002.fits.gz /tmp/``

- Anonymously GETting a public file: 
    ``cadc-data cp -v cadc:GEMINI/00aug02_002.fits ./``

- Use default netrc file ($HOME/.netrc):
    ``cadc-data cp -v -n cadc:GEMINI/00aug02_002.fits ./``

- Use a different netrc file to upload to the CFHT namespace bucket:
    ``cadc-data cp -d --netrc ~/mynetrc /tmp/700000o-wcs.fits cadc:CFHT/``
    ``cadc-data cp -d --netrc ~/mynetrc /tmp/700000o-wcs.fits cadc:CFHT/mynewfile.700000o.wcs.fits``

- Connect as user to upload (PUT) a file (prompt for password if user not in $HOME/.netrc):
    ``cadc-data cp -v -u auser 00aug02_002.fits cadc:GEMINI/00aug02_003.fits``

- Upload a file using a certificate for authentication:
    ``cadc-data cp --cert ~/.ssl/proxycert.pem /mnt/processed/scuba-2.fits cadc:JCMT/scuba2.fits``


Usage for ``RM``
~~~~~~~~~~~~~~~~~~~~

Remove files from the Storage System.

usage:  ``cadc-data rm COMMON-OPTIONS source [source... ]``

** **Note**:  One of ``--cert``, ``-u | --user``, ``-n``, or ``--netrc-file`` is required for delete.

.. table:: Positional Arguments

   ================= =============================================
   \                 Description
   ================= =============================================
   ``source``        [uri] The URI of the entities to delete
   ================= =============================================


Examples
^^^^^^^^

- Use certificate to DELETE the file for the given URI:
    ``cadc-data rm --cert ~/.ssl/cadcproxy.pem cadc:CFHT/700000o.fits.fz``

- Use default netrc file ($HOME/.netrc) to DELETE two files:
    ``cadc-data rm -v -n cadc:GEMINI/00aug02_002.fits cadc:GEMINI/00aug02_001.fits``
