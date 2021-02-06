# Intake Zenodo Fetcher: _Fetch data from Zenodo based on Intake catalog entries_

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/ESM-VFC/intake_zenodo_fetcher/main?filepath=examples/fetch_based_on_zenodo_doi.ipynb)

## Why?

To improve reproducibility of workflows that use data archived on Zenodo, `intake_zenodo_fetcher` simplifies downloading a local copy of the data belonging to a Zenodo DOI.
It offers functions to fetch _all_ data baloginging to a Zenodo DOI and it allows for restricting the download to just those parts of the Zenodo record that belong to a catalog entry in an Intake catalog.

See [examples/fetch_based_on_zenodo_doi.ipynb](https://nbviewer.jupyter.org/github/ESM-VFC/intake_zenodo_fetcher/blob/main/examples/fetch_based_on_zenodo_doi.ipynb) for a demo on pre-fetching the data based on a Zenodo DOI or [try it live on `mybinder.org`](https://mybinder.org/v2/gh/ESM-VFC/intake_zenodo_fetcher/main?filepath=examples/fetch_based_on_zenodo_doi.ipynb).

## Installation

Install with:
```python
python -m pip install git+https://github.com/ESM-VFC/intake_zenodo_fetcher.git
```

## Usage

`intake_zenodo_fetcher` populates the storage location indicate in the intake catalog with data from Zenodo, if the catalog entries have a `metadata.zenodo_doi` key.

For the following example entry `"FESOM2_sample"` pointing to <https://zenodo.org/record/3865567>
```yaml
metadata:
  version: 1

plugins:
  source:
      - module: intake_xarray

sources:
  FESOM2_sample:
    driver: netcdf
    description: 'FESOM2 pi mesh Sample dataset'
    metadata:
      zenodo_doi: "10.5281/zenodo.3865567"
    args:
      urlpath: "{{ CATALOG_DIR }}/FESOM2_PI_MESH/*.fesom.1948.nc"
      xarray_kwargs:
        decode_cf: False
        combine: 'by_coords'

```
running
```python
intake_zenodo_fetcher.download_zenodo_files_for_entry(
    cat['FESOM2_sample']
)
```
will make sure all files matching the glob pattern `"*.fesom.1948.nc"` are downloaded to `./FESOM2_PI_MESH/` in the directory that also contains the catalog.
