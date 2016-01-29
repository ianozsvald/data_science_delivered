Data Science Delivered
======================

About: Short document summarising Ian's thoughts on successful ways to ship working, maintainable and understandable data
science products and ways to avoid falling into dark holes of despair. This is based on my experience, your experience may be
very different - if so, file a Bug for me in GitHub and give me
something to chew on. 

Very roughly these are "notes from me to my-younger-self", I hope you find some of this to be useful. I've put this together based on my talking, teaching and coaching along with feedback from chats at our [PyDataLondon](http://www.meetup.com/PyData-London-Meetup/) monthly meetup. 

Put your email in [here for updates](http://ianozsvald.com/building-python-data-science-products/), I'll only mail about updates to this doc.

By: Ian Ozsvald ([*http://ianozsvald.com/*](http://ianozsvald.com/)) of [ModelInsight](http://modelinsight.io/) (do get in contact if consulting and coaching might be useful)

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
        - it is quite usual to iterate on the data+prototype stage several times, once you've made a prototyped you'll probably discover that the data has problems, as you address those data problems you'll realise you had poor assumptions in the prototype, so you'll iterate. you might get it right first time (and make sure a colleague validates this) but several iterations aren't uncommon.

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



    -   exploration
        - for numbers - figure out what distributions and examples you have. what are your outliers? are the outliers reasonable or the result of bad data?
        - for text - what examples do you have? how long and short are the strings? what character sets are used? do you care about case, accents, punctuation?
        - what kind of variations do you have in your data? what's normal? Seaborn's [pairplot](https://web.stanford.edu/~mwaskom/software/seaborn/generated/seaborn.pairplot.html) does a great job on small sets of numerical data

-   delivering data products

    -   How a project can evolve

        - Get your data (and expect to have to fetch more, it is fine to start with a sensible subset of the possible data - aim for Small Data that fits in RAM at the start to keep your exploration speed high)

        -   Hacking, scripts (knock out lots of ideas)

        -   Reporting results (tell a story, get someone to validate your assumptions, do a code review as a sanity check)

        - Using source-control (e.g. github.com), don't be slow to keep a history of edits and to sync off-site

        -   Pickles (you'll have partial results you want to store, a `pickle` is a reasonable way to store early-stage results)

        -   Configuration (make it run on other machines e.g. for colleagues or for deployment or other configurations like test, staging and deployment)

        -   Modules (use `__init__.py` in folders to group code into logical units)
            - See my [template](https://github.com/ianozsvald/python_template_with_config) for examples of a working module with *tests* and a *config*

        -   Packages (make your code distributable using `setup.py`)

        -   Testing (check `unittest.py` and [py.test](http://pytest.org/latest/))
            - You'll have more confidence in your code
            - When refactoring you'll know if your code still does what it was supposed to
            - When you upgrade libraries (e.g. new Python libraries or major Python versions) you'll be confident that your code still does the same job that it did before
            -   [py.test](http://pytest.org/latest/) to write cleaner tests than with Python's built-in `unittest.py` tests
                - you can run `py.test -s` to see `stdout` (which otherwise is hidden unless the tests fail) and `py.test -s -pdb` to drop into the Python debugger on a failure (so you get dropped in to [pdb](https://docs.python.org/3/library/pdb.html) to do live debugging)
            -   [coverage](https://pypi.python.org/pypi/coverage) to figure out if you're actually testing all of your code (so you can figure out which bits need the urgent addition of tests!)
            -   [hypothesis](https://github.com/DRMacIver/hypothesis) for fuzzing your unit tests and finding blind spots you missed (use this in _addition_ to your regular tests, not as a replacement for them)


        -   Data integrity tests (make sure your data actually contains what you expect - you can easily ignore this step and easily write a solution that lies to you - _be very wary of skipping this_)
            -   [engarde](https://github.com/TomAugspurger/engarde) to add constraints to your DataFrames

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

- dealing with disparate or hard-to-access data
    - it is common for an organically evolved system to have many datastores (e.g. a MongoDB, a MySQL, some SQLITEs, an Adobe Omniture, Google Analytics) which are loosely coupled where you have to work hard to get data from several datasets
    - commonly the schemas evolve independently and historic information can be bad at different times for different stores (and this domain info can be stored in an individual's head)
    - this sort of mix of data causes an *inertia* on research and reporting
    - possible solutions include
        - rewriting the storage layers
            - massive effort, lots of line-systems might require updating
            - if on-line data products need this view of the data (e.g. for live decisions on live datasets) then this might be the best option
        - building a unified approach to extracting all the data and storing it in a new system in a clean and consistent way (e.g. putting it into a Data Warehouse or a Big Data system)
            - less effort, but more cruft is left in the line-systems
            - data products can be built off of the new system but remember that this clean view of the data is only available in this data store (not in the live line-system data stores)

- dealing with dirty data
    - what clean data might look like (this is very problem specific!)
        - no pure duplicates
        - few or no near-duplicates (e.g. repeated records with typos or minor changes)
        - values that are normalised w.r.t. your problem
            - e.g. if you're working on a data-augmentation problem you'll need some dirty data but if you're regressing then you'll want clean data so you can solve your regression challenge
        - a sane schema
            - reasonable value ranges
            - sensible between-table relationships are represented
            - no (or few) lies in the data!
            - sensible use of `null` and `0` to represent missing and zero values

    - R&D on dirty data
        - transform the raw dirty data into a cleaned research-ready dataset
        - you want clean data when you solve your challenge
        - you can robustify your research code for deployment by taking the relevant cleaning code and integrating it into your production system

    - talks on dirty data
        - ["Dirty Data"](http://www.slideshare.net/godatadriven/dirty-data-by-friso-van-vollenhoven) by Friso van Vollenhoven (notes the Icelandic Thorn issue, bad date encodings)
        - ["Cleaning Confused Collections of Characters"](http://ianozsvald.com/2015/04/03/pydataparis-2015-and-cleaning-confused-collections-of-characters/) my thoughts on ways through extracting and cleaning text
    - dirty text data

        -   cleaning broken text

            -   [ftfy](https://github.com/LuminosoInsight/python-ftfy) to fix bad encodings

            -   poor specification of encoding - requires some checking on your part to make sensible guesses - often text "looks like" UTF8 but might actually be encoded in Windows [CP-1252](https://en.wikipedia.org/wiki/Windows-1252) which encodes smart-quotes differently to disk
            - [Chromium Compact Language Detector](https://pypi.python.org/pypi/chromium_compact_language_detector/) - identifies human language type

            -   HTML entity decoding (Python's [unescape](https://docs.python.org/3/library/html.html) does a sensible job for the basic entities)

        -   normalising unicode variants (text, punctuation)

            - problems for decoding non-English alphabets like the [Turkish Double I](https://en.wikipedia.org/wiki/Dotted_and_dotless_I) and the (horrible) associated stories of murder on that page
            -   variants of dash (e.g. 40 variants are [listed](https://en.wikipedia.org/wiki/Dash)) and white space (including non-breaking whitespace, [examples](https://en.wikipedia.org/wiki/Whitespace_character)), `and &`, `copyright ©` etc

        - normalising synonyms
            - probably having many synonymous words for the same "thing" you're working on is a bad idea (e.g. company name variants like RBS and The Royal Bank of Scotland, and product name variants like MS VC 2000 and Microsoft Visual Studio 2000) as the variants water down the signal, you should probably map these to canonical representations to reduce the variation in labelling. This only applies if you want the aggregate signal for the "common items" rather than the exact signal for each named variation
            - [ish](https://github.com/judy2k/ish) has synonyms for common terms like "yes, yep, yup, ..."
            - [unidecode](https://pypi.python.org/pypi/Unidecode) strips "funny accents" to remove variation in your text, this is useful if people write the same thing in different ways (e.g. Société Générale and Societe Generale will both be written by regular people to mean the same company)

        - normalising text terms
            - often names can be written in different orders e.g. "Ian Ozsvald" and (in Hungary) "Ozsvald Ian", with no other indication of the reversed encoding

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

-   solving problems with data

    - O'Reilly's notes on [Evaluating Machine Learning Models](http://www.oreilly.com/data/free/evaluating-machine-learning-models.csp)

    - a rough process for machine learning:
        - it is fine to focus on your training data only at first (before you have a useful model) - if you can't (over)fit your training data then you'll have no joy with your test data
        - identify what your model can't fit in your data, don't guess, diagnose it (at the start this is a great way to spot broken data or missing features that you took for granted!)
        - switch to using 10-fold Cross Validation once you can over-fit your training data, CV will give you an idea about how well you generalise for predicting the real world
        - now start to diagnose what the model gets wrong on your test data

    -   classification

        -   diagnosis tips:

            -   Andrew Ng's notes
                [*http://cs229.stanford.edu/materials/ML-advice.pdf*](http://cs229.stanford.edu/materials/ML-advice.pdf)

            - ["A Few Useful Things to Know about Machine Learning"](https://homes.cs.washington.edu/~pedrod/papers/cacm12.pdf), Pedro Domingos - really useful paper to clear up some ML assumptions

            -   metrics
                [*http://www.win-vector.com/blog/2015/09/willyourmodelworkpart4/*](http://www.win-vector.com/blog/2015/09/willyourmodelworkpart4/)
            - [Unboxing the Random Forest Classifier: The Threshold Distributions](http://nerds.airbnb.com/unboxing-the-random-forest-classifier/)

            - Visualise a [confusion matrix](http://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html) for the resulting classifications.

            - Visualise a correlation matrix for your features if you have a small number (e.g. <20 features). You might also visualise the similarities as a force network graph ([photo](https://twitter.com/lc0d3r/status/654226497893453824)) using NetworkX or Gephi.

            - Use a [Dummy classifier](http://scikit-learn.org/stable/modules/generated/sklearn.dummy.DummyClassifier.html) which classifies based on e.g. the most frequent class seen in the training set, this should reflect the underlying distribution of your classes and any classifier and features you build *must* outperform this (else they're not exploiting any information that may exist!)

            - What classifications are always wrong? Train on your training set and then use either your train or your test set to diagnose which labels in incorrectly predicts (e.g. for a binary classification task take a highly confident wrong class answer from your test set). What's missing? Poor features? Maybe the model is too simplistic? Maybe you have bad labels? 
                - [Example for misclassification diagnosis on the Digits dataset]()

            - Which classifications always sit on the decision boundary (e.g. items with a 50/50 probability of being in one of two classes)? Why can't the model confidently move the examples to the right class?

            - With many training runs you can plot the coefficients inside a classifier like Logistic Regression (using a boxplot per feature) to see the distribution of the weights. This should show if e.g. some of your feature data has a poor distribution (e.g. if it is non-Normal) and how variable the weights can be.

    -   regression

        -   diagnosis tips:

            - OLS is surprisingly robust, it is a great starting point
            - After training - predict on the training data, take the worst errors and try to diagnose those
            - Make a histogram of the values of the response value - is it Normal-like? If it is heavily skewed then maybe you should try to transform it (using a `sqrt` or `log`). With a heavy skew OLS can be skewed in favour of trying to fit the few outliers rather than the body of mostly-not-skewed values. Remember to measure your training error on the un-transformed data (else you can't compare it to the error on the non-transformed previous version)

    - feature diagnosis
        - TODO



    - (Unit) Testing on scikit-learn models
        - Valerio Maggio's talk at BudapestBI Forum 2015 on [Machine Learning Under Test](https://speakerdeck.com/valeriomaggio/machine-learning-under-test-at-biforum2015) from [BudapestBIForum 2015](https://budapestbi2015.sched.org/event/02b6848e6c63be71fcc2ab2ee14a84eb#.Vh5J9JfRJyQ) covers numeric precision (e.g. almost-equal, unit least precision), measures of generalisation error (e.g. cross validation, accuracy measures), hyperparameters

    -   natural language processing

        -   NLTK useful for ngrams and tokenisation

        -   it is faster to tokenise on whitespace yourself

        -   SpaCy looks good

        -   keep features human-readable to ease debugging

        -   tokenising a lower-cased string with whitespace tokenisation and unigrams gets you a long way

        - for two-class text classification (e.g. spam, "is user of typeX") the following configuration is a sane base-case starting point (this is a Pattern that you might want to follow):
            - assumption - normal-human-possible classification task (i.e. one with lots of redundancy that a regular human can easily solve)
            - lowercased, uni-decoded unigram features
            - bag of words (i.e. without order or frequency counts)
            - Bernulli Naive Bayes (by default scikit learn's BNB takes care of unbalanced data sets)
            - Use a dummy classifier that uses some regular expressions (i.e. driven by human insight rather than ML) for a super-easy-to-diagnose classification approach
            - don't be in a rush to go to complex models and features sets until you know why you need the complexity
            - tfidf scaling is useful for variable length documents (e.g. pages of wikipedia text) but if your text is roughly the same length (e.g. tweets) then scaling is less useful


-   delivering working products

    -   reporting

    -   delivering repeatable read-only datasets

    -   delivering read-only webservices

    -   delivering online data products

    -   KISS - explainable answers probably beat having the best score

    - deployment options to machines:
        - Amazon EC2 / other cloud providers, probably using Docker
        - [YHatHQ](http://www.yhathq.com/) @springcoil [has notes](https://speakerdeck.com/springcoil/putting-data-science-models-into-production)
    
    - deployment technologies:
        - use github to pull onto a target machine - useful at first for small scale deployments, not useful for automation
        - docker - useful for fully-built and reproducible builds

    - building a web-based API:
        - [Featherweight](https://github.com/ianozsvald/featherweight_web_api) super lightweight server for publishing R&D Python functions as JSON web functions
        - [Flask](http://flask.pocoo.org/) with Swagger documentation
        - [Django REST](http://www.django-rest-framework.org/)
        - [Eve](http://python-eve.org/) - Python + Flask and Mongo or SQL storage

- storing data
    - add constraints to your datastore whenever possible, probably they're granular (e.g. text only with N characters, ints-only and Nulls are allowed) with the wrong level of granularity (you might want lower-case hex-like ASCII UUID strings only or positive numbers for addresses within a certain range), but some constraints are *much better* than no constraints. Constraints between key fields in tables are also very sensible.
    -   pickles of `dicts` are good for persisting python objects

        - add a timestamp and maybe a git commit for traceability and a notes key in a `dict` along with your data (which might be `numpy` arrays, `dataframes` etc)
        - have a script that visits all your pickles, loads them in and reports the build-time, label and notes so you can quickly check what's deployed 
    - MySQL
        - Unicode text is 3-byte `utf8` by default (so it only encodes the Basic Multilingual Plane and not the Supplementary Planes) and so silently loses data that doesn't look "Western-like", use [`utf8mb4`](https://dev.mysql.com/doc/refman/5.5/en/charset-unicode-utf8mb4.html) instead

    - MongoDB
        - Schemaless by default (this hopefully changes in 2015), this makes it quick for iteration and rubbish for data integrity later when you have an engineering mindset (write your own validation & reporting code that checks your schema is met to avoid going crazy)
        - [Monary](https://monary.readthedocs.org/) lets you read MongoDB data->Python around 10* faster ([talk](https://github.com/braz/pycon2015_talk))
    -   dense and sparse matrices

        -   numpy arrays have a single type
        -   [scipy.sparse](https://docs.scipy.org/doc/scipy/reference/sparse.html) is excellent for sparse data - if you have a dense matrix that's larger than RAM then definitely check if you have many (e.g. >30%) zero-based entries, if so you should test the RAM usage of a sparse array
        -   you can store sparse data on disk with hdf5 and compression


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

- coding sensible practices
    - don't copy/paste magic numbers around your code, instead use the constant-convention with an upper-cased variable like `OFFSET=53.9` and use `OFFSET` throughout your code (rather than `53.9`). This is especially useful if you have two different magic numbers that mean differnets things (e.g. `10` used in two different contexts), having a spelt-out variable makes the intent much clearer when you return to this code months later.
    - if you find yourself copy/pasting a block of code (e.g. a few lines that make a new database connection or do a common data manipulation) then strongly think about refactoring this into a function, it'll make support easier and your code will get shorter (so there's fewer lines to hide bugs)
    
-   useful python tools

    -   [anaconda](https://www.continuum.io/why-anaconda) environments

    -   [ipython](https://ipython.org/) - interactive Python shell

    -   [jupyter](http://jupyter.org/) notebook (and consider `%connect_info` (sort-of [example](https://ipython.org/ipython-doc/2/parallel/magics.html#engines-as-kernels) to connect a Notebook's kernel with a console interface)

    -   argparse and os.env to get default configurations from environment

    -   [progressbar33](https://github.com/germangh/python-progressbar) / [tqdm](https://github.com/noamraph/tqdm)

    -   [ipython_memory_usage](https://github.com/ianozsvald/ipython_memory_usage) - diagnose line-by-line RAM usage in an IPython session

    -   [jq](https://stedolan.github.io/jq/) - excellent JSON parser

- IPython Notebook working practices

    - IPython at the console (`ipython`) is great for rapid R&D, typically I use an IPython shell and a VIM terminal as my only editor (VIM tends to exist on all platforms and over the wire and I like being old-skool)
    - The IPython/Jupyter Notebook is great for sharing content with colleagues and making a historic document for review, it isn't great as an IDE
    - In the IPython Notebook you can add `%qtconsole` (e.g. `%qtconsole --style=linux` for unix colouring) and it'll open a QTConsole in a new window which shares the same kernel, so you can prototype (and get tooltips and tab completions) in the shell, then copy/paste the useful lines into your Notebook. Plots will work in both the shell and the Notebook - see [details for QTconsole](https://ipython.readthedocs.org/en/stable/interactive/qtconsole.html?highlight=qtconsole)
    - When plotting you can use `%matplotlib inline` to get static inline plots in the Notebook (as PNGs) or `%matplotlib notebook` to get interactive graphics (e.g. tooltips, zooms) but note that each interactive graphic holds a heavyweight object, by default the Notebook limits to 20 (IIRC) plots before it gives you warnings
    - Quantopian's [QGrid](https://github.com/quantopian/qgrid) gives an Excel-like interactive grid for Pandas dataframes including sorting and filtering

- useful data cleaning tools

    - [dedupe](https://github.com/datamade/dedupe) DataMade's data dedupe and merge process with online learning
    - [usa address](https://github.com/datamade/usaddress) DataMade's process to convert USA unstructured addresses into structured data
    - [probable people](https://github.com/datamade/probablepeople) DataMade's process to convert unstructured Western people names into structured data
    - [parserator](https://github.com/datamade/parserator) DataMade's tool to make probabilistic parsers
    - [Data Making Guidelines](https://github.com/datamade/data-making-guidelines) DataMade's guide to a repeatable ETL process

-   higher performance python

    -   [Numba](http://numba.pydata.org/) compiles Python `numpy` operations (particularly for-loops on `numpy` arrays) using LLVM, this is a very sane first thing to try
    -   [PyPy](http://pypy.org/) works great on non-numpy code, if you have pure-Python CPU-bound code then definitely try PyPY
    -   `multiprocessing` lets you parallelise stuff on a single machine 
    - don't be in a rush to go to a cluster-solution (e.g. Spark) unless you really need it, it is fine to rent an Amazon EC2 machine with 32 cores for a couple of dollars an hour using `multiprocessing` and the exact same code will run on your laptop which eases your dev/debug cycle
    - Robert's [line_profiler](https://github.com/rkern/line_profiler) is my go-to profiling tool for speed issues
    - Fabian's [memory_profiler](https://pypi.python.org/pypi/memory_profiler) is my go-to tool for memory-based profiling
    - [High Performance Python](http://shop.oreilly.com/product/0636920028963.do) - my book covers a lot of this stuff


- Building data teams
    - ["The Emerging Role of Data Scientists on Software Development Teams"](http://research.microsoft.com/apps/pubs/default.aspx?id=242286) by Microsoft Research categorises how different team-types might work and how career progression might work

- The business side of delivering data science products
    - ["Delivering Value Throughout the Analytical Process"](http://blog.applied.ai/delivering-value-throughout-the-analytical-process/) by Jon Sedar gives a nice overview of how to talk whilst wearing your business hat
    - ["From Lab to Factory"](http://slides.com/springcoil/dataproducts-11#/) by Peadar Coyle lists a couple of ways of delivering working products
