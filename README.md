# Stand-up Comedy Recommender

This standup-comedy recommender is 100% powered by Natural Language Processing (NLP) techniques & unsupervised
machine learning algorithms. None of the topics assigned to each comedy special were hard-coded; they were
algorithmically generated. <br><br>This project was created over a 2-week span during my time at the
<a href="https://www.thisismetis.com/" target="_blank">Metis</a> data science program.

<h4>Technical Details</h5>

<h5>Data Acquisition & Storage</h5>
<p>
    The initial data source is a <a href=" https://scrapsfromtheloft.com/stand-up-comedy-scripts/" target="_blank">set of stand-up comedy transcripts</a>
    published on "Scraps from the Loft". I used <a href="https://www.crummy.com/software/BeautifulSoup/bs4/doc/" target="_blank">Beautiful Soup</a> to
    scrape the transcripts. The raw text was then processed and stored in a
    <a href="https://www.mongodb.com/" target="_blank">Mongo</a> database hosted
    remotely on an <a href="https://aws.amazon.com/ec2/" target="_blank">AWS EC2</a> instance.
</p>

<h5>NLP Processing Pipeline</h5>
<p>
    Each raw comedy transcript was processed with the following pipeline:
    <ul>
        <li>Initial baseline cleaning of text to remove punctuation and non-meaningful anomalies.</li>
        <li>
            Removal of stop words (including scikit-learn's predefined set of
            <a href="https://scikit-learn.org/stable/modules/feature_extraction.html#stop-words" target="_blank">stop words</a>
            as well as a manually defined list of words.
        </li>
        <li>
            Lemmatization using <a href="https://www.nltk.org/_modules/nltk/stem/wordnet.html" target="_blank">NLTK</a>
        </li>
        <li>
            Vectorized using scikit-learn's <a href="https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html" target="_blank">TFIDF Vectorizer</a>.
        </li>
    </ul>
</p>

<h5>Machine Learning Algorithms (Topic Modeling & Recommender)</h5>
<p>
    The filter topics were generated by reducing the dimensions of the TFIDF-Vectorized data using an
    <a href="https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.NMF.html" target="_blank">NMF decomposition</a>.
    The topic names were assigned by manually reviewing the top words associated with each topic.

The search/recommender feature takes the user-provided text, puts it in the same TFIDF variable space as the original corpus using the pre-trained
NLP Pipeline described above. Then, the text is transformed into the same topic space as the corpus using the pre-trained NMF model.
Finally,
<a href="https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html">cosine similarity</a>
is calculated between the search term topic space and the topic space for every document in the corpus. Those similarities are ranked, and
the top 10 most similar comedy specials are displayed as a recommendation.
</p>

#### Data Sources
* Comedy Transcripts - [Scraps From The Loft](https://scrapsfromtheloft.com/stand-up-comedy-scripts/)
* Comedy Special Poster Images - [OMDB API](https://www.omdbapi.com/)

#### File Contents
* The `analysis` directory contains scripts that were used to acquire data, set up a Mongo database, process the raw
  text, and fit machine learning models.
* The `app` directory contains files used by the Flask application. `app.py` is the main file for the Flask app.
* `presentation.pptx` contains the mock business presentation I made for the Metis course.

#### Dependencies

Install the dependencies by running the following in your terminal:

`pip install -r requirements.txt`