#Support Vector Machines (SVM) Lab

ref:  [http://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html](http://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)

If you do not already have a text editor, try [Sublime Text 3](http://www.sublimetext.com/3).

###Terminal Tips for Mac OS

In your terminal, navigate to a directory.

To open a Finder window in the current directory, type:

	open .

Suppose a file is inside the directory, such as an image. To open the file using the default program for that extension (e.g. an image previewer), type:

	open image.png


### a. iris data

In your text editor open the file `svm_plot_iris.py`

- take a look at the file structure (similar to ipython commands)
- what is the expected output from running this file?

Run svm_plot_iris.py from your terminal:

    $ python svm_plot_iris.py
    
Take a look at the output file `svm_iris.png`

#### command line arguments
Back in your text editor uncomment lines 29-31.  Now we can pass arguments from the command line.  Try running the file again with these arguments:

    $ python svm_plot_iris.py .02 1.0 svm_iris.png
    
It looks like our file is now catching our command line arguments, so let's actually use them.  Uncomment lines 33-46 and run again:

    $ python svm_plot_iris.py .02 1.0 svm_iris.png

Experiment with some different values for the `step size` and `regularization parameter`:

    $ python svm_plot_iris.py .05 2.0 svm_iris.png

    $ python svm_plot_iris.py .1 5.0 svm_iris.png

Notice that matplotlib / pyplot can also generate pdfs (just change the file extension:

    $ python svm_plot_iris.py .1 5.0 svm_iris.pdf

### b. sheep and wolves dataset

Open the file `svm_plot_animals.py` in your text editor, and take a look at the code.  Then start a new python shell in your terminal:

    $ ipython
    
Import our svm_plot_animals file:

    >>> import svm_plot_animals as spa
    
    >>> dir(spa)   # look at contents
    
Notice that numpy, pylab, etc. are imported under `spa`:

    >>> a = np.array([1, 2, 3])   # note this doesn't work - see the next line
    
    >>> a = spa.np.array([1, 2, 3])
    
    >>> type(a)
    
    >>> import numpy as np
    
    # now we can use the normal syntax
    
    >>> b = np.array([4, 5, 6])

Take a look at the version variable:

    >>> spa.version
    
    # Ah!  Forgot to uncomment it - do that now:
    
    >>> spa.version   # note that after editing a file, it is not auto-reloaded
    
    >>> reload(spa)
    
    >>> spa.version
    
Simple add function:

    >>> spa.add_vals(1, 2)
    
    >>> spa.add_vals(1)   # uses default for `b` argument
    
Dictionary of classifiers:

    >>> spa.clfs

#### reading in the datafile(s)
Use the `survey_animals` function to read in animal 'locations' from the text file:

    >>> spa.survey_animals('animals1.txt')
    
    # notice that a dataframe object is returned
    
    >>> df = spa.survey_animals('animals1.txt')
    
    >>> df
    
This is our familiar pandas dataframe object.

    >>> df['animal'].values
    
    >>> df['animal_type'].values

Of course, we can also access through the notebook as usual (assuming files are in the right places):

import svm_lab.svm_plot_animals as spa
df = spa.survey_animals('animals1.txt')
df

Back to terminal - lets fit and plot:

    >>> clf_name = "SVM"
    >>> clf = spa.clfs[clf_name]
    >>> train_cols = ["x", "y"]
    >>> clf.fit(df[train_cols], df.animal_type)
    >>> spa.plot_results(clf, clf_name, df, 1, plot=True)
    
Note:  close out of the graph window to return your python shell prompt.

Finally, use the convenience function to fit and plot all three classifiers in `clfs`:

    >>> spa.plot_all(spa.clfs, 'animals1.txt')