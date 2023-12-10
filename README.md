## Hotel-Review-Sentiment-Analysis-using-Tableau-Python

Sentiment analysis involves the process of computationally identifying and categorizing opinions expressed in a piece of text, especially in order to determine whether the writer's attitude towards a particular topic, product, etc. is positive, negative, or neutral.  In this Tableau packaged workbook, we will use a number of reviews of hotels.

We will use the Vader lexicon to help us in the analysis. When we inspect the Vader lexicon, we will see that it associates a value with a large number of icons, typed emojis, and words.

Finally, we rely on the nltk package to conduct the sentiment analysis.


1. Installing and Starting TabPy:

        conda create -name virtualenv

   The machine will inform you that it wants to load a number of packages and ask you for permission:
   Click y to proceed. Now, it has loaded the packages and created a new environment:
      
2. Activate this environment:

        conda activate ame

3. Make sure to have the latest version of the command to install TabPy (pip for Python Installer Package)

        python -m pip install -U pip
    
4. Install TabPy:

        pip install tabpy

5. To start the TabPy server, just type -

        tabpy

Preparing Tableau:

Now that we have TabPy running, we need to ensure that Tableau uses it.  To do so, we need to set up an analytics extension to Tableau.  We can do this from the Help menu in Tableau.  Go to Help > Settings and Performance > Manage Analytics Extension Connection:

To configure the Analytics Extension Connection, I selected TabPy and set the server to localhost and the port to 9004, which was the port the TabPy server is running on:

We can use Test Connection button to make sure everything went right:

I loaded the dataset file into Tableau and then created a calculated value Sentiment that uses Python functions from the nltk package to assign a value of between -1 and 1 to each review.  The script is

    SCRIPT_REAL("
    import nltk
    nltk.download('vader_lexicon')
    from nltk.sentiment import SentimentIntensityAnalyzer
    text = _arg1
    scores = []
    sid = SentimentIntensityAnalyzer()
    for word in text:
        ss = sid.polarity_scores(word)
        scores.append(ss['compound'])
    return scores
     ",
     ATTR([Reviews.Text]))

The above script goes through each review, assigns a score to it and then add that score to the scores list, which it returns.

Finally, we created a report with a filter that showed only the hotels in Florida with all reviews by the name of the hotel.






