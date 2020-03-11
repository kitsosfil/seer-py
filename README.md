# seer-py

Python SDK for the Seer data platform, which handles authenticating a user, downloading channel data, and uploading labels/annotations.

## Install

To install, simply clone or download this repository, then type `pip install .` which will install all the dependencies.

### Epilepsy Ecosystem Data

For users attempting to download data for the [Epilepsy Ecosystem](https://www.epilepsyecosystem.org/howitworks/), please download the [latest release](https://github.com/seermedical/seer-py/releases/latest) instead of cloning the repository or downloading the master branch. Then open the script `ContestDataDownloader.py` in `Examples` and it will guide you through the download process (you will need to change a few things in this script including the path to download the data to).

## Requirements

This library currently requires Python 3, and it if you don't currently have a Python 3 installation, we recommend you use the Anaconda distribution for its simplicity, support, stability and extensibility. It can be downloaded here: https://www.anaconda.com/download

The install instructions above will install all the required dependencies, however, if you wish to install them yourself, here's what you'll need:

- [`gql`](https://github.com/graphql-python/gql): a GraphQL python library is required to query the Seer platform. To install, simply run: `pip install gql`
- Pandas, numpy, and matplotlib are also required. Some of these installs can be tricky, so Anaconda is recommended, however, they can be installed separately. Please see these guides for more detailed information:
  - https://scipy.org/install.html
  - https://matplotlib.org/users/installing.html
  - https://pandas.pydata.org/pandas-docs/stable/install.html

To run the Jupyter notebook example (optional, included in Anaconda): `pip install notebook`

## Getting Started

Check out the [Example](Examples/Example.ipynb) for a step-by-step example of how to use the SDK to access data on the Seer platform.

To start a Jupyter notebook, run `jupyter notebook` in a command/bash window. Further instructions on Jupyter can be found here: https://github.com/jupyter/notebook

## Authenticating

To access data from the Seer platform you must first register for an account at 
https://app.seermedical.com/. You can then use seer-py to authenticate with the API.

Create an instance of the `SeerAuth` class (defined in _auth.py_) and provide
your email address and password using one of the following options:

1. Pass them as arguments when instantiating a `SeerAuth` object.
2. In your local user directory, create a _.seerpy/_ folder with a file named
  _credentials_ in it. Save your email address on the first line and your
  password on the second line with no other text.
3. If you don't use one of the previous options, you will be prompted to enter
  your email address and password in the terminal or Jupyter notebook before
  you can retrieve any data.

## Running with other API endpoints

To run seer-py against other environments (for instance against a development API server), perform the following steps:

1. Run the `seer-api` server following the relevant documentation in the API repository
2. Run a `seer-graphiql` server (which includes the required proxies), ensuring that `HTTPS` is set to `OFF` in the startup script,
3. Call `SeerConnect` with the alternative endpoint, and setting `dev` to `True`. For example:

```python
client = SeerConnect(api_url='http://localhost:3090/api/development', dev=True)
```

## Troubleshooting

### Downloading hangs on Windows

There is a known issue with using python's multiprocessing module on Windows with spyder. The function `getLinks` uses `multiprocessing.Pool` to run multiple downloads simultaneously, which can cause the process to run indefinitely. The workaround for this is to ensure that the current working directory is set to the directory containing your script. Running the script from a command window will also solve this problem. Alternatively, setting `threads=1` in the `getLinks` function will stop in from using `multiprocessing` altogether.
