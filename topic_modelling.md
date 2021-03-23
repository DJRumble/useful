# An example of topic modelling

In this example with have a sample of customer feedback from the retail experience along with the TNPS scores. 

We apply a date range, TNPS range and additional filters to reduce to a focused list of verbatums.

Some cleaning functions for the data.

```
# Functions for text manipulation

def sent_to_words(sentences):
    '''Tokenise each sentence into a list of words, removing punctuations'''
    for sentence in sentences:
        yield(gensim.utils.simple_preprocess(str(sentence), deacc=True))  # deacc=True removes punctuations
        
def remove_stopwords(texts):
    return [[word for word in simple_preprocess(str(doc)) if word not in stop_words] for doc in texts]

def make_bigrams(texts):
    return [bigram_mod[doc] for doc in texts]

def make_trigrams(texts):
    return [trigram_mod[bigram_mod[doc]] for doc in texts]

def lemmatization(texts, allowed_postags=['NOUN', 'ADJ', 'VERB', 'ADV']):
    """https://spacy.io/api/annotation"""
    texts_out = []
    for sent in texts:
        doc = nlp(" ".join(sent)) 
        texts_out.append([token.lemma_ for token in doc if token.pos_ in allowed_postags])
    return texts_out
```	