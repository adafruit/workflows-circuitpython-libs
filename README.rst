.. image:: https://raw.githubusercontent.com/adafruit/Adafruit_CircuitPython_Bundle/main/badges/adafruit_discord.svg
    :target: https://adafru.it/discord
    :alt: Discord

.. image:: https://github.com/circuitpython/circuitpython-unified-build-ci/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_VEML7700/actions/
    :alt: Build Status

workflows-circuitpython-libs
============================

A collection of CI workflowss for CircuitPython libraries

Instructions
------------

These CI exist to both provide a uniform CI across all the libraries, as well as provide an
easy way to modify and update them as changes need to be made.  Each CI workflow is broken
out into its own composite action that can be called within a workflow, such as:

.. code-block:: yaml

    - name: Run GitHub Release CI workflow
      uses: adafruit/workflows-circuitpython-libs/release-gh@main
      with:
        github-token: ${{ secrets.GITHUB_TOKEN }}

Build CI
--------

The build CI is the workflow that runs for every push.  It is additonally run for pull
requests to check that there are no issues regarding linting, documentation, etc.  It
requires no inputs and is invoked using:

.. code-block:: yaml

    - name: Run Build CI workflow
      uses: adafruit/workflows-circuitpython-libs/build@main

GitHub Release CI
-----------------

The GitHub release CI runs whenever a release is made for a library.  It's main job
to attach bundles to the releases.  It takes the following arguments:

* ``github-token``: A valid GitHub token with authorization scope to upload the file
  to the release
* ``upload-url`` The upload URL where the release assets can be uploaded; should be
  that of the release

It can be invoked using:

.. code-block:: yaml

    - name: Run GitHub Release CI workflow
      uses: adafruit/workflows-circuitpython-libs/release-gh@main
      with:
        github-token: ${{ secrets.GITHUB_TOKEN }}
        upload-url: ${{ github.event.release.upload_url }}

PyPI Release CI
---------------

The PyPI release CI also runs whenever a release is made for a library.  It updates
the version string with the GitHub release tag and then builds and uploads the
library to PyPI for download on SBC and other Blinka-based targets.  It takes the
following inputs:

* ``pypi-username``: The username for the PyPI account, which twine requires
* ``pypi-password``: The password for the PyPI account, which twine requires

It can be invoked using:

.. code-block:: yaml

    - name: Run PyPI Release CI workflow
      uses: adafruit/workflows-circuitpython-libs/release-pypi@main
      with:
        pypi-username: ${{ secrets.pypi_username }}
        pypi-password: ${{ secrets.pypi_password }}
