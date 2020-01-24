cadcdata (1.4.0) 
========

.. image:: https://img.shields.io/pypi/v/cadcdata.svg   
    :target: https://pypi.python.org/pypi/cadcdata

Canadian Astronomy Data Centre - data access

Access library and client for astronomical data hosted at the Canadian Astronomy Data Centre



Common Options (``COMMON-OPTIONS``) for ``GET/PUT/DELETE``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
    ``[--resource-service-id REGISTRY_SERVICE_ID]`` Resource service identifier (default ivo://cadc.nrc.ca/minoc)
    ``[-n]``                                        Use .netrc in $HOME for authentication
    ``[--netrc-file NETRC_FILE]``                   NetRC file to use for authentication
    ``[[-u | --user] USER]``                        Name of user to authenticate. Note: application prompts for the corresponding password!
    =============================================== =============================================


Usage for ``GET``
~~~~~~~~~~~~~~~~~
Retrieve files from the Storage System.

usage:  ``cadc-data get COMMON-OPTIONS GET-OPTIONS resource-id [resource-id ...]``


.. table:: GET Positional Arguments

   ================= =============================================
   \                 Description
   ================= =============================================
   ``resource-id``   The URI of the resource(s) to PUT/GET/DELETE
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



**Examples**:

- Anonymously getting a public file: 
    ``cadc-data get -v cadc:GEMINI/00aug02_002.fits``

- Use certificate to get a cutout and save it to a file:
    ``cadc-data get --cert ~/.ssl/cadcproxy.pem -o /tmp/700000o-cutout.fits --cutout [1] cadc:CFHT/700000o``

- Use default netrc file ($HOME/.netrc) to get FITS header of a file:
    ``cadc-data get -v -n --fhead cadc:GEMINI/00aug02_002.fits``

- Use a different netrc file to download wcs information:
    ``cadc-data get -d --netrc ~/mynetrc -o /tmp/700000o-wcs.fits --wcs cadc:CFHT/700000o``

- Connect as user to download two files and uncompress them (prompt for password if user not in $HOME/.netrc):
    ``cadc-data get -v -u auser -z cadc:GEMINI/00aug02_002.fits cadc:GEMINI/00aug02_003.fits``


Usage for ``PUT``:
~~~~~~~~~~~~~~~~~~
Upload files to the Storage System.

usage:  ``cadc-data put COMMON-OPTIONS PUT-OPTIONS base-resource-id file [file ...]``


.. table:: PUT Positional Arguments

   ========================================= =============================================
   \                                         Description
   ========================================= =============================================
   ``base-resource-id``                      Base resource ID to prepend to file names
   ``file``                                  The file(s) to upload.
   ========================================= =============================================
   
.. table:: PUT Optional Arguments (``PUT-OPTIONS``)

    ========================================= ====================================================
    Option                                    Description
    ========================================= ====================================================
    ``[--nomd5]``                             Do not perform md5 check at the end of transfer
    ``[-t | --type]``                         MIME type to set.  Deduced by default
    ``[-e |  --encoding]``                    MIME encoding to set.  Deduced by default
    ========================================= ====================================================



**Examples**:

- Anonymously getting a public file: 
    ``cadc-data put -v cadc:GEMINI/00aug02_002.fits``

- Use default netrc file ($HOME/.netrc):
    ``cadc-data put -v -n cadc:GEMINI/00aug02_002.fits``

- Use a different netrc file to upload:
    ``cadc-data put -d --netrc ~/mynetrc cadc:CFHT /tmp/700000o-wcs.fits``

- Connect as user to upload two files (prompt for password if user not in $HOME/.netrc):
    ``cadc-data put -v -u auser cadc:GEMINI 00aug02_002.fits 00aug02_003.fits``

- Upload a file using a certificate for authentication:
    ``cadc-data put --cert ~/.ssl/proxycert.pem cadc:JCMT scuba2.fits``

