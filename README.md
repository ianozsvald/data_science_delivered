Data Science Delivered
======================

About: Short document summarising Ian's thoughts on successful ways to ship working, maintainable and understandable data
science products and ways to avoid falling into dark holes of despair. This is based on my experience, your experience may be
very different - if so, file a Bug for me in GitHub and give me
something to chew on. 

Very roughly these are "notes from me to myself 10 years ago", I hope you find some of this to be useful. I've put this together based on my talking, teaching and coaching along with feedback from chats at our [PyDataLondon](http://www.meetup.com/PyData-London-Meetup/) data group.

By: Ian Ozsvald ([*http://ianozsvald.com/*](http://ianozsvald.com/)) of [ModelInsight](http://modelinsight.io/)

License: Creative Commons By Attribution

Location:
[*https://github.com/ianozsvald/data\_science\_delivered*](https://github.com/ianozsvald/data_science_delivered)

Aimed at: Existing data scientists, both for those who are engineers and those who are researchers. 

My notes
--------

-   overview of the major stages in a data project (from conception to maintenance)

    -   outline the projects

    -   take a look at the data

        -   [*http://simplystatistics.org/2014/06/13/what-i-do-when-i-get-a-new-data-set-as-told-through-tweets/*](http://simplystatistics.org/2014/06/13/what-i-do-when-i-get-a-new-data-set-as-told-through-tweets/)

    -   determine what's feasible, define milestones, get buy-in
        - keep the scope small and sane (especially if you're not confident about your data or how you'll solve the aspirational goals of the project)

    -   deliver a working prototype

    -   deliver a deployable, maintainable and testable system

    -   support the solution

-   early stages

    -   sources of data
        - don't forget external data (e.g. OpenData) and licensable data
        - joining external data to internal data might be tricky (you probably lack a decent join key)

    -   discovery - what's feasible? what's valuable? how far might this project go?

    -   dirty data

        -   cleaning data is necessary, nothing else works if the data is not clean

        -   cleaning data is an on-going process

        -   low quality data breaks everything
        -   don’t patch over bad data, you forget about it and it never gets better, then you rely on the assumptions and things go bad

        -   write validators that check for bad data - run them regularly, report exceptions to the specification, treat this as a _red flag_ event and try to get to the source of the problem (and patch up broken data before you forget about it)

    - dirty text data

        -   cleaning broken text

            -   [ftfy](https://github.com/LuminosoInsight/python-ftfy) to fix bad encodings

            -   poor specification of encoding - requires some checking on your part to make sensible guesses - often text "looks like" UTF8 but might actually be encoded in Windows [CP-1252](https://en.wikipedia.org/wiki/Windows-1252) which encodes smart-quotes differently to disk
            - [Chromium Compact Language Detector](https://pypi.python.org/pypi/chromium_compact_language_detector/) - identifies human language type

            -   HTML entity decoding (Python's [unescape](https://docs.python.org/3/library/html.html) does a sensible job for the basic entities)

        -   normalising unicode variants (text, punctuation)

            -   unidecode, Turkish double-I problem

            -   variants of dash and white space, `and &`, `copyright ©`

        - normalising synonyms
            - probably having many synonymous words for the same "thing" you're working on is a bad idea (e.g. company name variants like RBS and The Royal Bank of Scotland, and product name variants like MS VC 2000 and Microsoft Visual Studio 2000) as the variants water down the signal, you should probably map these to canonical representations to reduce the variation in labelling. This only applies if you want the aggregate signal for the "common items" rather than the exact signal for each named variation
            - [ish](https://github.com/judy2k/ish) has synonyms for common terms like "yes, yep, yup, ..."
            - [unidecode](https://pypi.python.org/pypi/Unidecode) strips "funny accents" to remove variation in your text, this is useful if people write the same thing in different ways (e.g. Société Générale and Societe Generale will both be written by regular people to mean the same company)

        -   normalising weights and measures

            -   astropy etc?

            -   no tool to recognise these?

        - encoding text categories for machine learning
            - Pandas has a [get_dummies](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.get_dummies.html) which performs naieve encoding
            - [Patsy](http://statsmodels.sourceforge.net/devel/contrasts.html) has a richer set of encoding options and tries to remove redundant encodings

    - dirty numeric data

        -   numeric outliers for normally distributed numbers - checking the values outside of a couple of standard deviations is a sane starting point
        - checking for outliers in a non-stationery dataset (e.g. a time series of prices) is trickier (READER - what's a good starting point?)

        -   check for text-encoded numbers like NaN and INFINITY, they confuse things when parsed!


    -   exploration
        - for numbers - figure out what distributions and examples you have. what are your outliers? are the outliers reasonable or the result of bad data?
        - for text - what examples do you have? how long and short are the strings? what character sets are used? do you care about case, accents, punctuation?
        - what kind of variations do you have in your data? what's normal? Seaborn's [pairplot](https://web.stanford.edu/~mwaskom/software/seaborn/generated/seaborn.pairplot.html) does a great job on small sets of numerical data

-   delivering data products

    -   How a project can evolve

        - Get your data (and expect to have to fetch more, it is fine to start with a sensible subset of the possible data - aim for Small Data that fits in RAM at the start to keep your exploration speed high)

        -   Hacking, scripts (knock out lots of ideas)

        -   Reporting results (tell a story, get someone to validate your assumptions, do a code review as a sanity check)

        -   Pickles (you'll have partial results you want to store, a `pickle` is a reasonable way to store early-stage results)

        -   Configuration (make it run on other machines e.g. for colleagues or for deployment or other configurations like test, staging and deployment)

        -   Modules (use `__init__.py` in folders to group code into logical units)

        -   Packages (make your code distributable using `setup.py`)

        -   Testing (check `unittest.py` and [py.test](http://pytest.org/latest/))

        -   Data integrity tests (make sure your data actually contains what you expect - you can easily ignore this step and easily write a solution that lies to you - _be very wary of skipping this_)

        -   Speed of iteration (it gets slower as the code gets bigger, it is also likely that more bugs slip in which occasionally will _really_ slow you down)

        -   Repeatable processes

        -   Online data (we've gone from a static lump of data to needing to update our models with new data - how do we do this and deploy it within our unique operating constraints?)

        - Big Data (maybe you really have a Big Data problem - now you've figured out how to solve the problem on a subset of the data you need to figure out how to deploy this in a cluster)

-   the cost of bad data

    -   we all have bad data, we sweep it under the carpet

    -   bad data should be treated like bad logic - it’ll torpedo your project so it must be fixed

    -   bad data will keep creeping back in from new data sources and from existing data sources and from legacy data sources, if you don't monitor for it then you won't see this (and later it'll just be painful to deal with)

    -   you have to actively monitor it, report on it and fix it

    - bad data will add a "development tax" on your work, it'll incrementally slow you down and over time it can cause your project to stall when the whole teams switches for months just to clean the data so it can be useful again

    -   make it a red-light scenario when your data goes bad and fix it else you’ll keep running slower and slower (just like you spend more time fire-fighting if you don’t bother with a good unit-test and test management process)

    -   O’Reilly’s Bad Data Handbook

    -   Some general notes on cost of bad data:

        - [The cost of bad data: stats](https://econsultancy.com/blog/64612-the-cost-of-bad-data-stats/)

        - [Enterprises Don’t Have Big Data, They Just Have Bad Data](http://techcrunch.com/2015/07/01/enterprises-dont-have-big-data-they-just-have-bad-data/)

-   solving problems with data

    - O'Reilly's notes on [Evaluating Machine Learning Models](http://www.oreilly.com/data/free/evaluating-machine-learning-models.csp)

    -   classification

        -   diagnosis tips:

            -   Andrew Ng's notes
                [*http://cs229.stanford.edu/materials/ML-advice.pdf*](http://cs229.stanford.edu/materials/ML-advice.pdf)

            -   metrics
                [*http://www.win-vector.com/blog/2015/09/willyourmodelworkpart4/*](http://www.win-vector.com/blog/2015/09/willyourmodelworkpart4/)
            - [Unboxing the Random Forest Classifier: The Threshold Distributions](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)

            - Visualise a [confusion matrix](http://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html) for the resulting classifications.

            - Visualise a correlation matrix for your features if you have a small number (e.g. <20 features). You might also visualise the similarities as a force network graph ([photo](https://twitter.com/lc0d3r/status/654226497893453824)) using NetworkX or Gephi.

            - What classifications are always wrong? Train on your training set and then use either your train or your test set to diagnose which labels in incorrectly predicts. What's missing? Poor features? Maybe the model is too simplistic? Maybe you have bad labels? 
            - Which classifications always sit on the decision boundary (e.g. items with a 50/50 probability of being in one of two classes)? Why can't the model confidently move the examples to the right class?

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

        - for two-class text classification (e.g. spam, "is user of typeX") the following configuration is a sane base-case starting point:
            - assumption - normal-human-possible classification task (i.e. one with lots of redundancy that a regular human can easily solve)
            - lowercased, uni-decoded unigram features
            - bag of words (i.e. without order or frequency counts)
            - Bernulli Naive Bayes (by default scikit learn's BNB takes care of unbalanced data sets)
            - maybe just use a dummy classifier that uses some regular expressions (i.e. driven by human insight rather than ML) for a super-easy-to-diagnose classification approach
            - don't be in a rush to go to complex models and features sets until you know why you need the complexity
            - tfidf scaling is useful for variable length documents (e.g. pages of wikipedia text) but if your text is roughly the same length (e.g. tweets) then scaling is less useful

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

    - deployment options to machines:
        - Amazon EC2 / other cloud providers, probably using Docker
        - [YHatHQ](http://www.yhathq.com/) (TODO link to @springcoil's yhat talk from pyconireland)
        - [Flask](http://flask.pocoo.org/) with Swagger documentation
        - [Django REST](http://www.django-rest-framework.org/)
        - [Eve](http://python-eve.org/) - Python + Flask and Mongo or SQL storage

- storing data
    - MySQL
        - Unicode text is 3-byte `utf8` by default (so it only encodes the Basic Multilingual Plane and not the Supplementary Planes) and so silently loses data that doesn't look "Western-like", use [`utf8mb4`](https://dev.mysql.com/doc/refman/5.5/en/charset-unicode-utf8mb4.html) instead

    - MongoDB
        - Schemaless by default (this hopefully changes in 2015), this makes it quick for iteration and rubbish for data integrity later when you have an engineering mindset (write your own validation & reporting code that checks your schema is met to avoid going crazy)

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

    -   pickles of `dicts` are good for persisting python objects

        - add a timestamp and maybe a git commit for traceability and a notes key in a `dict` along with your data (which might be `numpy` arrays, `dataframes` etc)
        - have a script that visits all your pickles, loads them in and reports the build-time, label and notes so you can quickly check what's deployed 

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
