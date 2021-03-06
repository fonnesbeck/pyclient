# Python client for All of Us Workbench Jupyter Notebooks

The client uses Python 2.7, to match FireCloud Jupyter environments.

## Local testing

### Configure

The file all_of_us_config.json contains configuration settings used during
local tests; this file is copied to notebook clusters during localization.

If you want to point at a different workspace or CDR, you can change the values;
check in your changes if you want it to be permanent.  

### Install

```Shell
virtualenv venv
. venv/bin/activate
./project.rb setup-env
```

### Regenerate the Swagger client

```Shell
pyclient$ ./project.rb swagger-regen
```

This will place generated Python files in py/aou_workbench_client/swagger_client,
and generated README files in py/swagger_docs and py/README.swagger.md.

After running this, run the following to add new files to git:

```Shell
git add py/aou_workbench_client/swagger_client
git add py/swagger_docs
```

## Run tests
```Shell
pyclient$ ./project.rb test
```

Running tests will place the test service account credentials in sa-key.json.

The test service account owns free tier billing projects for the test environment, and has
access to all workspaces created in them.

If you want to do manual testing of the client when running locally, you can run:

```Shell
export GOOGLE_APPLICATION_CREDENTIALS=<absolute path to sa-key.json>
```

## Manual Testing

- Push your topic branch to GitHub.
- From your notebook, run:

  ```
  %%bash
  PY_VERSION=2.7 # or 3.4
  MY_TOPIC_BRANCH=
  "pip${PY_VERSION}" install --user --upgrade "git+https://github.com/all-of-us/pyclient.git@${MY_TOPIC_BRANCH}#egg=aou_workbench_client&subdirectory=py"
  ```
 - You may also need to restart the kernel via the Jupyter UI: 'Kernels' -> 'Restart'

## Releases

To publish a new version, modify py/setup.py with the new version number,
merge any changes to master (including any regenerated
Swagger client changes), and then run:

```Shell
git checkout master
git pull
git tag pyclient-vX-Y-rcZ
git push --tags
```

where X is a major version, Y is a minor version, and Z is a release candidate
number.

You will then be able to use the pushed version of the client library in a notebook
by running:

```Shell
!pip install --user 'https://github.com/all-of-us/pyclient/archive/pyclient-vX-Y-rcZ.zip#egg=aou_workbench_client&subdirectory=py'
```


