Data Science Delivered
======================

About: Short document summarising Ian's thoughts on successful ways (and
ways to avoid!) to ship working, maintainable and understandable data
science products. This is based on my experience, your experience may be
very different - if so, file a Bug for me in GitHub and give me
something to chew on. I've put this together based on my talking, teaching and coaching.

By: Ian Ozsvald ([*http://ianozsvald.com/*](http://ianozsvald.com/))

License: Creative Commons By Attribution

Location:
[*https://github.com/ianozsvald/data\_science\_delivered*](https://github.com/ianozsvald/data_science_delivered)[
]()

Aimed at: Existing data scientists, both for those who are engineers and those who are researchers. 

My notes
--------

-   overview of major stages in a project (conception to maintenance)

    -   outline the projects

    -   take a look at the data

        -   [*http://simplystatistics.org/2014/06/13/what-i-do-when-i-get-a-new-data-set-as-told-through-tweets/*](http://simplystatistics.org/2014/06/13/what-i-do-when-i-get-a-new-data-set-as-told-through-tweets/)

    -   determine what's feasible, define milestones, get buy-in

    -   deliver a working prototype

    -   deliver a deployable, maintainable and testable system

    -   support the solution

-   early stages

    -   sources of data

    -   discovery

    -   common sources of dirty data and how to clean

        -   cleaning data is necessary, nothing else works if the data is not clean

        -   cleaning data is an on-going process

        -   low quality data breaks everything

        -   cleaning broken text

            -   ftfy

            -   poor specification of encoding

            -   HTML entites

        -   normalising unicode variants (text, punctuation)

            -   unidecode, Turkish double-I problem

            -   variants of dash and white space, “and &”, “copyright \[symbol\]”

        -   normalising weights and measures

            -   astropy etc?

            -   no tool to recognise these?

        -   numeric outliers for normally distributed numbers

        -   checking for text-encoded numbers like NaN and INFINITY

        -   don’t patch over bad data, you forget about it and it never gets better, then you rely on the assumptions and things go bad

        -   validators that check for bad data

    -   exploration

-   delivering data products

    -   How a project evolves

        -   Hacking, scripts

        -   Reporting results

        -   Pickles

        -   Config

        -   Modules

        -   Packages

        -   Testing

        -   Data integrity tests

        -   Speed of iteration

        -   Static analysis

        -   Repeatable processes

        -   Online data

-   the cost of bad data

    -   we all have bad data, we sweep it under the carpet

    -   data projects frequently killed but to inertia from having to tackle low quality data

    -   bad data should be treated like bad logic - it’ll torpedo your project so it must be fixed

    -   bad data will keep creeping back in from new data sources and from existing data sources and from legacy data sources

    -   you have to actively monitor it, report on it and fix it

    -   make it a red-light scenario when your data goes bad and fix it else you’ll keep running slower and slower (just like you spend more time fire-fighting if you don’t bother with a good unit-test and test management process)

    -   O’Reilly’s Bad Data Handbook

    -   Some general notes on cost of bad data:

        -   [*https*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*://*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*econsultancy*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*.*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*com*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*/*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*blog*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*/64612-*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*the*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*-*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*cost*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*-*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*of*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*-*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*bad*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*-*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*data*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*-*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*stats*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)[*/*](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)

        -   [*http*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*://*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*techcrunch*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*.*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*com*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*/2015/07/01/*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*enterprises*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*dont*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*have*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*big*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*data*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*they*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*just*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*have*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*bad*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*-*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*data*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)[*/*](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)

-   solving problems with data

    -   classification

        -   diagnosis tips:

            -   Understanding what a RandomForest thinks is important [*http*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*://*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*nerds*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*.*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*airbnb*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*.*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*com*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*/*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*unboxing*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*-*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*the*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*-*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*random*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*-*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*forest*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*-*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*classifier*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)[*/*](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)

            -   Andrew Ng's notes
                [*http://cs229.stanford.edu/materials/ML-advice.pdf*](http://cs229.stanford.edu/materials/ML-advice.pdf)

            -   metrics
                [*http://www.win-vector.com/blog/2015/09/willyourmodelworkpart4/*](http://www.win-vector.com/blog/2015/09/willyourmodelworkpart4/)

            - Visualise a [confusion matrix](http://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html) for the resulting classifications.

            - Visualise a correlation matrix for your features if you have a small number (e.g. <20 features). You might also visualise the similarities as a force network graph ([photo](https://twitter.com/lc0d3r/status/654226497893453824)) using NetworkX or Gephi.

    -   regression

        -   diagnosis tips:

            -   &lt;TODO&gt;


    - Unit-testing on scikit-learn models
        - Valerio Maggio's talk at BudapestBI Forum 2015 on [Machine Learning Under Test](https://budapestbi2015.sched.org/event/02b6848e6c63be71fcc2ab2ee14a84eb#.Vh5J9JfRJyQ)

    -   natural language processing

        -   NLTK useful for ngrams and tokenisation

        -   it is faster to tokenise on whitespace yourself

        -   SpaCy looks good

        -   keep features human-readable to ease debugging

        -   tokenising a lower-cased string with whitespace tokenisation and unigrams gets you a long way

-   building useful tools

    -   starting with scripts

    -   modularising

    -   config.py - centralise file/db access to few functions

    -   adding tests

        -   py.test

        -   coverage

        -   hypothesis

        -   engarde

    -   making a package

    -   scratch and scratch\_local

    -   routes to distribution

-   delivering working products

    -   reporting

    -   delivering repeatable read-only datasets

    -   delivering read-only webservices

    -   delivering online data products

    -   KISS - explainable answers probably beat having the best score

-   engineering concerns (how stuff goes wrong)

    -   not logging the right data

    -   not automating your data-edit process for production data (you mustn’t edit this stuff by hand as you’ll have multiple environments over time)

    -   not automating the logging/scraping of data

        -   intermittent logging (causing gaps)

    -   schemas change but not using a good change approach

        -   field name changes

        -   datatype changes

    -   not having a sensible schema

        -   mixing `"" None Id(0) "none" "NOTPRESENT"` to all indicate a Null condition in the same dataset, this is a really bad idea and will lead to confusion. Fix on using only 1 Null value.

        -   having different applications (e.g. MongoDB driven by C\# and Python with [different Legacy UUIDs](http://3t.io/blog/best-practices-uuid-mongodb/)) write data in similar but incompatible ways

    -   data lakes are probably a better idea then never-finished perfect schemas

    -   lack of monitoring and lack of acting on the monitored result

        -   write data validator scripts and run them religiously

        -   when data goes wrong, make it a high priority else it’ll poison later work

    -   use testing to limit problems

-   storing data

    -   dense and sparse matrices

        -   numpy arrays have a single type

        -   scipy.sparse is excellent for sparse data

        -   you can store sparse data on disk with hdf5 and compression

    -   pickles of dicts are good for persisting python objects

        -   add a timestamp and maybe a git commit for traceability

-   useful python tools

    -   anaconda environments

    -   ipython

    -   jupyter notebook

    -   argparse and os.env to get default configurations from environment

    -   progressbar33 / tqdm

    -   ipython\_memory\_checker

    -   jc

-   higher performance python

    -   numba on numpy

    -   pypy

    -   multiprocessing
