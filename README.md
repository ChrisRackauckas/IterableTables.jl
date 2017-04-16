# IterableTables

[![Project Status: Active - The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)
[![](https://img.shields.io/badge/docs-stable-blue.svg)](https://davidanthoff.github.io/IterableTables.jl/stable)
[![Build Status](https://travis-ci.org/davidanthoff/IterableTables.jl.svg?branch=master)](https://travis-ci.org/davidanthoff/IterableTables.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/uv9ybxa17e8581pr/branch/master?svg=true)](https://ci.appveyor.com/project/davidanthoff/iterabletables-jl/branch/master)
[![Query](http://pkg.julialang.org/badges/IterableTables_0.5.svg)](http://pkg.julialang.org/?pkg=IterableTables)
[![codecov.io](http://codecov.io/github/davidanthoff/IterableTables.jl/coverage.svg?branch=master)](http://codecov.io/github/davidanthoff/IterableTables.jl?branch=master)

## Overview

IterableTables defines a  generic interface for tabular data.

The package currently has support for the following data sources:
[DataFrames](https://github.com/JuliaStats/DataFrames.jl),
[DataStreams](https://github.com/JuliaData/DataStreams.jl)
(including [CSV](https://github.com/JuliaData/CSV.jl),
[Feather](https://github.com/JuliaStats/Feather.jl),
[SQLite](https://github.com/JuliaDB/SQLite.jl),
[ODBC](https://github.com/JuliaDB/ODBC.jl)),
[DataTables](https://github.com/JuliaData/DataTables.jl),
[IndexedTables](https://github.com/JuliaComputing/IndexedTables.jl),
[TimeSeries](https://github.com/JuliaStats/TimeSeries.jl),
[TypedTables](https://github.com/FugroRoames/TypedTables.jl) and
any iterator who produces elements of type
[NamedTuple](https://github.com/blackrock/NamedTuples.jl).

The following data sinks are currently supported:
[DataFrames](https://github.com/JuliaStats/DataFrames.jl) (including things
like ``ModelFrame`` etc.),
[DataTables](https://github.com/JuliaData/DataTables.jl),
[IndexedTables](https://github.com/JuliaComputing/IndexedTables.jl),
[TimeSeries](https://github.com/JuliaStats/TimeSeries.jl),
[TypedTables](https://github.com/FugroRoames/TypedTables.jl),
[StatsModels](https://github.com/JuliaStats/StatsModels.jl),
[Gadfly](https://github.com/GiovineItalia/Gadfly.jl) and
[VegaLite](https://github.com/fredo-dedup/VegaLite.jl).

## Installation

This package only works on julia 0.5 and newer. You can add it with:
```julia
Pkg.clone("https://github.com/davidanthoff/IterableTables.jl.git")
```

## Getting started

``IterableTables`` makes it easy to conver between different table types in julia. It also makes it possible to use any table type in situations where packages traditionally expected a ``DataFrame``.

For example, if you have a ``DataFrame``
````julia
using DataFrames

df = DataFrame(Name=["John", "Sally", "Jim"], Age=[34.,25.,67.], Children=[2,0,3])
````

you can easily convert this into any of the supported data sink types by simply constructing a new table type and passing your source ``df``:
````julia
using DataTables, TypedTables, IterableTables

# Convert to a DataTable
dt = DataTable(df)

# Convert to a TypedTable
tt = Table(df)

````
These conversions work in pretty much any direction. For example you can convert a ``TypedTable`` into a ``DataFrame``:
````julia
new_df = DataFrame(tt)
````
Or you can convert it to a ``DataTable``:
````julia
new_dt = DataTable(tt)
````
The general rule is that you can convert any sink into any source.

``IterableTables`` also adds methods to a number of packages that have traditionally only worked with ``DataFrame``s that make these packages work with any data source type defined in ``IterableTables``.

For example, you can run a regression on any of the source types:
````julia
using GLM, DataFrames

# Run a regression on a TypedTable
lm(@formula(Children~Age),tt)

# Run a regression on a DataTable
lm(@formula(Children~Age),dt)
````
Or you can plot any of these data sources with ``Gadfly``:
````julia
using Gadfly

# Plot a TypedTable
plot(tt, x=:Age, y=:Children, Geom.line)

# Plot a DataTable
plot(dt, x=:Age, y=:Children, Geom.line)
````
Again, this will work with any of the data sources listed above.
