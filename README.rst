
powerlaw: A Python Package for Analysis of Heavy-Tailed Distributions
=======

``powerlaw`` is a toolbox using the statistical methods developed in
`Clauset et al. 2007`__ and `Klaus et al. 2011`__ to determine if a
probability distribution fits a power law. Academics, please cite as:

Jeff Alstott, Ed Bullmore, Dietmar Plenz. (2014). powerlaw: a Python package
for analysis of heavy-tailed distributions. `PLoS ONE 9(1): e85777`__

Also available at `arXiv:1305.0215 [physics.data-an]`__

__ http://arxiv.org/abs/0706.1062 
__ http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0019779
__ http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0085777
__ http://arxiv.org/abs/1305.0215

Basic Usage
-----------------
For the simplest, typical use cases, this tells you everything you need to
know.::

    import powerlaw
    data = array([1.7, 3.2 ...]) # data can be list or numpy array
    results = powerlaw.Fit(data)
    print results.power_law.alpha
    print results.power_law.xmin
    R, p = results.distribution_compare('power_law', 'lognormal')

For more explanation, understanding, and figures, see the working paper,
which illustrates all of powerlaw's features. For details of the math, 
see Clauset et al. 2007, which developed these methods.

Quick Links
-----------------
`Installation`__

`Paper illustrating all of powerlaw's features, with figures`__

`Code examples from manuscript, as an IPython Notebook`__
Note: Some results involving lognormals will now be different from the
manuscript, as the lognormal fitting has been improved to allow for
greater numerical precision.

`Documentation`__

`Known Issues`__

`Update Notifications, Mailing List, and Contacts`__

This code was developed and tested for Python 2.x with the 
`Enthought Python Distribution`__,  and later amended to be
compatible with 3.x. The full version of Enthought is 
`available for free for academic use`__.

__ http://code.google.com/p/powerlaw/wiki/Installation
__ http://arxiv.org/abs/1305.0215 
__ http://nbviewer.ipython.org/github/jeffalstott/powerlaw/blob/master/manuscript/Manuscript_Code.ipynb
__ http://pythonhosted.org/powerlaw/
__ https://code.google.com/p/powerlaw/wiki/KnownIssues
__ http://code.google.com/p/powerlaw/wiki/Interact
__ http://www.enthought.com/products/epd.php
__ http://www.enthought.com/products/edudownload.php 

Power Laws vs. Lognormals and powerlaw's 'lognormal_positive' option
-----------------
When fitting a power law to a data set, one should compare the goodness of fit to that of a `lognormal distribution`__. This is done because lognormal distributions are another heavy-tailed distribution, but they can be generated by a very simple process: multiplying random variables together. The lognormal is exactly like the normal distribution, which can be created by adding random variables together; in fact, the log of a lognormal distribution is a normal distribution (hence the name). In contrast, creating a power law generally requires fancy or exotic generative mechanisms (this is probably why you're looking for a power law to begin with; they're sexy). So, even though the power law has only one parameter (``alpha``: the slope) and the lognormal has two (``mu``: the mean of the random variables and ``sigma``: the standard deviation of the underlying normal distribution), we typically consider the lognormal to be a simpler explanation for observed data, as long as the distribution fits the data just as well. For most data sets, a power law is actually a worse fit than a lognormal distribution, or perhaps equally good, but rarely better. This fact was one of the central empirical results of the paper `Clauset et al. 2007`__, which developed the statistical methods that ``powerlaw`` implements. 

__ https://en.wikipedia.org/wiki/Lognormal_distribution
__ http://arxiv.org/abs/0706.1062 

However, for many data sets, the superior lognormal fit is only possible if one allows the fitted parameter ``mu`` to go negative. If one assumes the lognormal distribution is generated by multiply random variables (which is why one considers the lognormal distribution to be a simpler explanation), then having a negative ``mu`` implicitly assumes that the random variables can take negative values. In fact, it assumes that the random variables are *typically* negative. For some physical systems, this is perfectly possible. For the data you're studying, though, it's probably a weird assumption. All of the data points you're fitting to are positive by definition, since power laws must have positive values and ``powerlaw`` throws out 0s or negative values. So why would those data be generated by a process that multiplies together negative values? The resulting distribution of data should have negative data points as well, but you don't have them (or you threw them out). 

One possible solution is ``lognormal_positive``. This is just a regular lognormal distribution, except `mu` must be positive. You can compare a power law to this distribution in the normal way shown above::

    R, p = results.distribution_compare('power_law', 'lognormal_positive')
    
You may find that a lognormal where ``mu`` must be positive gives a much worse fit to your data, and that leaves the power law looking like the best explanation of the data. Before concluding that the data is in fact power law distributed, consider carefully whether a more likely explanation is that the data is a lognormal distribution, generated by random variables that can have negative values, and thus have a negative ``mu``.


Further Development
-----------------
``powerlaw`` is open for further development. If there's a feature you'd like to see in ``powerlaw``, `submit an issue <https://github.com/jeffalstott/powerlaw/issues>`_. 
Pull requests and offers for expansion or inclusion in other projects are welcomed and encouraged. The original author of `powerlaw`, Jeff Alstott, is now only writing minor tweaks, so contributions are very helpful.

Acknowledgements
-----------------
Many thanks to Andreas Klaus, Mika Rubinov and Shan Yu for helpful
discussions. Thanks also to `Andreas Klaus <http://neuroscience.nih.gov/Fellows/Fellow.asp?People_ID=2709>`_,
`Aaron Clauset, Cosma Shalizi <http://tuvalu.santafe.edu/~aaronc/powerlaws/>`_,
and `Adam Ginsburg <http://code.google.com/p/agpy/wiki/PowerLaw>`_ for making 
their code available. Their implementations were a critical starting point for
making powerlaw.
