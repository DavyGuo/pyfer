# Pyfer

|Name |Github |
|-|-|
|Gabriel Bogo|[@GabrielBogo](https://github.com/GabrielBogo)|
|Yuwei Liu |[@liuyuwei169](https://github.com/liuyuwei169)|
|Weifeng Davy Guo |[@DavyGuo](https://github.com/DavyGuo)|
|Mohamad Makkaoui |[@makka3](https://github.com/makka3)|


Python implementation of the `infer` R package, that offers a tidy way of developing statistical inference built on top of Pandas.

The `infer` package in R streamlines the process of reshuffling and bootstrapping of samples, calculating summary statistics and confidence intervals, and performing hypothesis tests for statistical inference. It does this using a combination of functions that are built with the emphasis on clear expressive code and using correct statistical grammar that explains the way the values are calculated and the tests are evaluated in statistical inference.

With this package as the inspiration, `pyfer` will have four main functions (`specify`,`generate`,`calculate`,`get_ci`) for the first iteration. These functions will, given a data frame and the specified response variable; calculate summary statistics and confidence intervals for the response variable. Further details follow in the description of the functions below.

##### Where does `pyfer` fit into the Python ecosystem?

Currently, there isn't a package in Python's ecosystem (according to our not so thorough research) that does a great job at replicating the `infer` package's functionality and easy-of-use. Therefore, we hope that this package will provide the basic tools to perform statistical inference using expressive code in Python.

## Functions

**specify(data, response, explanatory)**  

    Choose specific columns to feed the subsequent pipeline.

    Parameters:
    ---------------
    data: pd.DataFrame
    response: string
        One column of the dataframe to be the response variable.
    explanatory: string or list of strings
        Columns to be the explanatory variables

    Returns:
    ---------------
    pd.DataFrame:
        Dataframe containing one column for response variable and zero or more columns for the explanatory variables. The first column is always the response.

**generate(data, n_samples, type="boostrap")**  

    Generate bootstrap resamples and permutations

    Parameters:
    ---------------
    data: pd.DataFrame
        A Dataframe containing a response column (the first one) and zero or more explanatory columns. Usually the output of a specify function.
    n_samples: integer
        Number of resamples.
    type: string
        "Bootstrap" (default), or "Permutation".

    Returns:
    ---------------
    pd.DataFrame:
        Dataframe containing all resamples stacked vertically. Will keep all columns from the input data and an additional sample_id column to identify individual resamples.


**calculate(data, stat="mean")**  


    Calculate a summarizing statistic for each bootstrap sample.

    Parameters:
    ---------------
    data: pd.DataFrame
        A Dataframe generated from `generate` function with columns: response, sample_id and zero or more explanatory variables.
    stat: string
        "mean" (default) or "median".

    Returns:
    ---------------
    pd.DataFrame:
        Summarized data. Each row contains the summary statistic for a given resample.


**get_ci(data, alpha=0.05, point_estimate=None)**  

    Return the bootstrap confidence interval for a point estimate.

    Parameters:
    ---------------
    data: pd.DataFrame
        A Dataframe containing summarizing statistics from multiple resamples. Typically it's the output of a `calculate` function.
    interval: float
        Significance level of the confidence interval. Example: 0.05 (default) represents the 95% confidence interval.

    Returns:
    ---------------
    pd.DataFrame
        Dataframe containing 1 row and columns for Statistic (Point Estimate), significance level, Lower Bound and Upper Bound.
