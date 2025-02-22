# Recipes

A recipe defines how to transform data in one format / location into another format / location.
The primary way people contribute to Pangeo Forge is by writing / maintaining recipes.

```{warning}
The Recipe API is still in flux and may change. Make sure the version of the documentation
you are reading matches your installed version of pangeo_forge.
```

## The Recipe Object

A Recipe is a Python object which encapsulates a workflow for transforming data.
A Recipe knows how to take a {class}`file pattern <pangeo_forge.patterns.FilePattern>`,
which descibes a collection of source files ("inputs"),
and turn it into a single analysis-ready, cloud-optimized dataset.
Creating a recipe does not actually cause any data to be read or written; the
recipe is just the _description_ of the transformation.
To actually do the work, the recipe must be {doc}`executed <execution>`.
Recipe authors (i.e. data users or data managers) can either execute their recipes
on their own computers and infrastructure, in private, or contribute their recipe
to the public Pangeo Forge {doc}`recipe_box`, where it can be executed in the cloud via
{doc}`bakeries`.

## Recipe Classes

To write a recipe, you must start from one of the existing recipe classes.
Recipe classes are based on a specific data model for the input files and target dataset format.
Right now, there is only one recipe class implemented:

### XarrayZarr Recipe

The {class}`pangeo_forge.recipes.XarrayZarrRecipe` recipe class uses
[Xarray](http://xarray.pydata.org/) to read the input files and
[Zarr](https://zarr.readthedocs.io/) as the target dataset format.
The inputs can be in any [file format Xarray can read](http://xarray.pydata.org/en/latest/user-guide/io.html),
including NetCDF, OPeNDAP, GRIB, Zarr, and, via [rasterio](https://rasterio.readthedocs.io/),
GeoTIFF and other geospatial raster formats.
The target Zarr dataset can be written to any storage location supported
by [filesystem-spec](https://filesystem-spec.readthedocs.io/); see {doc}`storage`
for more details.
The target Zarr dataset will conform to the
[Xarray Zarr encoding conventions](http://xarray.pydata.org/en/latest/internals/zarr-encoding-spec.html).

The best way to really understand how recipes work is to go through the relevant
tutorials for this recipe class. These are, in order of increasing complexity
- {doc}`tutorials/netcdf_zarr_sequential`
- {doc}`tutorials/multi_variable_recipe`
- {doc}`tutorials/terraclimate`

Below we give a very basic overview of how recipes are used.

First you must define a {doc}`file pattern <file_patterns>`.
Once you have a {class}`file pattern <pangeo_forge.patterns.FilePattern>` object,
initializing an `XarrayZarrRecipe` can be as simple as this.

```{code-block} python
recipe = XarrayZarrRecipe(file_pattern)
```

There are many other options we could pass, all covered in the API documentation
{class}`below <pangeo_forge.recipes.XarrayZarrRecipe>`.

All recipes need {doc}`storage <storage>` for the target dataset.
If you have already defined a {class}`pangeo_forge.storage.FSSpecTarget` object,
then you can either assign it when you initialize the recipe or later, e.g.

```{code-block} python
recipe.target = FSSpecTarget(fs=fs, root_path=target_path)
```

This recipe may also requires a cache, a place to store temporary
files. We can create one as follows.

```{code-block} python
recipe.input_cache = CacheFSSpecTarget(fs=fs, root_path=cache_path)
```

Once your recipe is defined and has its targets assigned, you're ready to
move on to {doc}`execution`.

The API documentation below explains all of the possible options for `XarrayZarrRecipe`.
Many of these options are explored further in the {doc}`tutorials/index`.

```{eval-rst}
.. autoclass:: pangeo_forge.recipes.XarrayZarrRecipe
```
