## Lessons Learnt Mining 120 GB Data in R
&nbsp;
### ZHANG Ruda
### Dec 2016

--

## Overview

1. Measure data size
1. Data version control
1. Code Management
1. Numerical Operations
1. Visualization

<div id="right">

<p>
- You can place two graphs on a slide
- Or two columns of text
- These are all created with div elements
</p>

</div>


--

Two types of research: data heavy, algorithm heavy (simulation / scientific computation)
(When computing all all data is affordable, don't use statistics.)
Knowledge discovery as data compression: raw data (start from CSV), compressed data, data abstraction, visualization, statistical models.

## Measure data size

Exchange format:
CSV is a compact exchange format for tabular data; extra: human-readable, suitable for command line processing.
(communication format: Apache Arrow, Parquet) much faster but less support.

Raw dump (level 0):
2009 trip_9.csv (14 columns)
14626748 * 208 = 3042363584 (3037060168; 2.9G)
hash (32) * 2 + categorical (1 * 2 + 3) + timestamp (19) * 2 + integer (1 + 4) + floating point (18 unsigned, 19 * signed) * 5 + column delimiter (14) = 218
2009 fare_9.csv (11 columns)
14626748 * 116 = 1696702768 (1681547968; 1.6G)
hash (32) * 2 + categorical (3 * 2) + timestamp (19) + floating point (3/4 + 1 + 3 + ~3 + 1 + 3/4) + column delimiter (11) = 114
2009-2013
(2.9G + 1.6G) * 12 * 5 = 270G (94G GZipped)

GZIP compression ratio: 0.28~0.38
<!--image: compression ratio & speed pareto front -->

Cleaned CSV (level 1):
(22 columns)
176897200 * 140 = 24765608000 (24705588672; 24G)
ID (4 + 6/7 + 8/9) + timestamps (10) * 2 + categorical (3 + 1 + 4/6 + 2/4/5) + integer (3/4 + 1) + floating point (3/6 * 3 + 3 * 2 + 3/4 + 1; unsigned 11 * 2, signed 12 * 2) + column delimiter (22) = 148
2009-2013
8.5E8 * 140 = 1.19E11 (114G; 32G GZipped)

In-memory (level 1):
8.5E8 * 140 = 1.19E11 (114G)
int/factor (4) * 9 + double/datetime (8) * 13 = 140

Storage:

1. In-memory (data frame, e.g. R, Python): as long as your data fits; AWS EC2 r3.8xlarge (250GB RAM), x1.16xlarge (1TB RAM), x1.32xlarge (2TB RAM);
1. External storage (RDBMS, e.g. PostgreSQL): cheap but seriously slow, even with SSD; outdated solution for data analytics.
1. Distributed systems (Hadoop, Spark, etc.): complex, not as efficient; for TB and larger data.

data representation:
data.frame (tabular data): unordered records of heterogeneous variables.
data.table benchmarks against other data frame solutions:
<!--1e9 row, 50GB benchmark bench-grouping-1e9.png-->
<!--1e8 rows, 1GB benchmark https://github.com/szilard/benchm-databases#results-->
performance, space, storage;
I/O: fread, fwrite;
<!--image-->
<!--http://blog.h2o.ai/wp-content/uploads/2016/04/Results.png fwrite-hightlighed.png-->
reference semantics (in-place modification, low memory footprint);
radix sort and physical index
compact syntax exploiting lazy evaluation and chaining: `DT[i, j, keyby]`


## Numerical Operations

- Sort. (key and physical indices)
- Trees are fast, thus popular. (bbox, L^infty vs L^2 neighborhood; B-tree, R-tree)
- Create simple new features. (mark column, Manhattan grid coordinate system vs point in polygon)

## Visualization

Design graphics by pixels.
(pixel arithmetic)

--

## Data version control

Data processing levels:

- Level 0 data are raw data dumped from (TLC) database. At higher levels, the data are converted into more useful variables and formats.
- Level 1 data is the most fundamental record with significant scientific utility.
- Level 2 data is the first directly usable data for most scientific applications. Derived variables include trip type, driver profit, etc.
- Level 3 data is smaller and have regular spatial (discrete network) and temporal organization.
- Level 4 data is coarser derived variables, including profit rate, vacancy rate, etc.

```sh
head -n 20 trip_9.csv | sed -n '1,3 !p'
tree ~/data/nyctaxi
```

## Code Management

Keep things in one place, or lose them. (/R, /man, /vignette, small objects and data tables)
Separate reusable code from scripts.
functions & (small objects and data tables) data in library

(RStudio demo: Files)

--

## Takeaway Lessons

1. Use data.table. <!-- .element: class="fragment highlight-red" -->
1. Process data in a hierarchy.
1. Sort.
1. Trees are fast, thus popular. <!-- .element: class="fragment highlight-blue" -->
1. Create simple new features.
1. Design graphics by pixels.
1. Separate reusable code from scripts.
1. Keep things in one place, or lose them.
1. etc.
1. etc.
1. etc.
1. etc.
