# Usage Guide

This guide provides information on the methods available within the Open Food Facts Python SDK.

## API

The SDK can be used to access Open Food Facts API.

First, instantiate an API object:

```python
from openfoodfacts import API, APIVersion, Country, Environment, Flavor

api = API(
    user_agent="<application name>",
    username=None,
    password=None,
    country=Country.world,
    flavor=Flavor.off,
    version=APIVersion.v2,
    environment=Environment.org,
)
```

All parameters are optional with the exception of user_agent, but here is a description of the parameters you can tweak:

- `username` and `password` are used to provide authentication (required for write requests)
- `country` is used to specify the country, which is used by the API to return product specific to the country or to infer which language to use by default. `world` (all products) is the default value
- `flavor`: the Open*Facts project you want to interact with: `off` (Open Food Facts, default), `obf` (Open Beauty Facts),...
- `version`: API version (v2 is the default)
- `environment`: either `org` for production environment (openfoodfacts.org) or `net` for staging (openfoodfacts.net)


*Get information about a product*

```python
code = "3017620422003"
api.product.get(code)
```

*Perform text search*

```python
results = api.product.text_search("mineral water")
```

*Create a new product or update an existing one*

```python
results = api.product.update(CODE, body)
```

with `CODE` the product barcode and `body` the update body.

## Using the dataset

If you're planning to perform data analysis on Open Food Facts, the easiest way is to download and use the Open Food Facts dataset dump.
Fortunately it can be done really easily using the SDK:

```python
from openfoodfacts import ProductDataset

dataset = ProductDataset("csv")

for product in dataset:
    print(product["product_name"])
```

With `dataset = ProductDataset("csv")`, we automatically download (and cache) the dataset. We can then iterate over it to get information about products.

Two dataset types are available `csv` and `jsonl`. The `jsonl` dataset contains all the Open Food Facts database information but takes much more storage (>5 GB), while the `csv` dataset is much ligher (~700 MB) but only contains the most important fields.

The `jsonl` dataset type is used by default.
