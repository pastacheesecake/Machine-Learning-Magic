# Machine-Learning-Magic
NLTK - Tensorflow (soon)

Writing poetry and narrative texts using statistical-demon-summoning (cause human inspiration is overrated I guess)

# Machine Learning Narrative - Poetry

## Requirements

* __Python 3__ (I suggest Anaconda 3)
* NumPy, SciPy, Stats and the like (come with Anaconda)
* Twython, won't do Twitter magic but `nltk.vader` needs it (╯°益°)╯彡┻━┻	
* __NLTK__, you should download all the corpus and associated modules [NLTK](https://www.nltk.org) (either `pip install nltk` or `conda install nltk` )
* TensorFlow for Python [TensorFlow Python3 API](https://www.tensorflow.org/api_docs/python/)
* Pickle and JSON if wanna get fancy [̲̅$̲̅(̲̅ ͡° ͜ʖ ͡°̲̅)̲̅$̲̅]	

## Poetry

It all depends on what you want to write, my personal method is quite simple:

1. Get a plain-text library of books you wanna cut-up from and save them in your project folder. *Can get PDFs but then you'd need to extract the text and PDF text-extraction in Python 3 is... not the best tbh) I think there's a Gutenberg Library out there or something that has plain-text books and whatnot* There's some Python 2 modules for this task, also C/C++ are a much better choice.
2. Tokenize the texts using `nltk.tokenize()` and dump those results somewhere *(cause we don't wanna repeat the process every time we write something)* i.e

```python
from nltk import tokenize
folder_path = 'poetry' #Directory where our data is
container = [] # Contains tokenized books to be pickled into file, probably list ain't a good idea though
for filename in glob.glob(os.path.join(folder_path, '*')):  #Finds all files in  folder_path
  with open(filename, 'r',encoding='utf-8') as f: # utf-8 magic
    text = ' '.join(f.read().splitlines()) #strips newlines
    lines_list = tokenize.sent_tokenize(text) #nltk tokenization
    container.append(lines_list)
    container = [item for sublist in container for item in sublist] #flattens the list of lists, maybe slow... fix it yourself
with open('results_poetry.txt','wb') as results:
    pickle.dump(container,results)
    results.close()
    
    
```

3. Having all your words tokenized, do cut-up magic a-la *Burroughs* or *Dada*, here's my method
* Randomly select words from your pickle dump/text file using python's random module or maybe select specific words and whatnot. *since some of you were asking...* Yet again, either pickle this or dump in a JSON file or whatever you feel like doing if wanna keep the results.
* *Optional: If you wannna make actual sentences, use the tags generated by `nltk.tokenize()` so pick in a sensible manner i.e [Chomsky Hierarchy](https://en.wikipedia.org/wiki/Chomsky_hierarchy) (this will come in handy when writing stories)*
* Filter by number of syllables, use either [Pyphen](http://pyphen.org), `nltk.cmudict` or code your own syllable-counting function like a big boy ( ಠ ͜ʖ ಠ)	*pyphen is the best imo*
* Apply sentiment analysis (either pre-trained `nltk.sentiment.vader` [Documentation](https://www.nltk.org/_modules/nltk/sentiment/vader.html) or train your own TensorFlow neural network like a good hee-ho *more on this later I guess*), yet again filter or do as you please

```python

'''
Simple sentiment analysis module, takes a pickle file and returns basic data.
Up to you to take more stats and whatnot. 
Not up for Bayesian hocus pocus atm
٩(ఠ益ఠ)۶

'''


from nltk.sentiment.vader import SentimentIntensityAnalyzer
from nltk import tokenize
import pickle
import statistics as stats #Not necessary
import numpy as np   # Not necessary

negativity = []
positivity = []
neutral = []
compound = []
data = [positivity,negativity,neutral] # There's probably a better container out there, possibly numpy array or something idk
with open('results.txt','rb') as fp: #Our previous -pickled- results
    database = pickle.load(fp)
sid = SentimentIntensityAnalyzer() #Instances VADER

def analyse(sentences):
#Accepts list of sentences and performs sentiment intensity analysis
    for i in sentences:
        # print(i) uncomment if you actually want the sentence
        ss = sid.polarity_scores(i)
        negativity.append(ss['neg'])
        positivity.append(ss['pos'])
        neutral.append(ss['neu'])
        for k in sorted(ss):
            print('{0}: {1},'.format(k, ss[k]), end='\n')  #For each sentence it prints a polarity index.

analyse(database)

```
* *Profit?*

**Note: It's completely possible to do all this in a big script and cut all the pickle fairy dust but if you're dealing with lots of books, it's gonna take a lot of time so up to you bb**

## Narrative

To be fair, there's no straight-up method to write narrative texts using NLTK or plain Python, there's several TensorFlow implementations though, but they're all equally as convoluted and nonsensical as Infinite Jest. 

1. Train your own neural network using books/texts you want to replicate (needs time, a good machine and several hours or Google Cloud if you're loaded)
2. Using a pre-trained neural network (won't help you with this one bb)
3. Cheating using NLTK tokens and Chomsky Hierarchy

### TensorFlow implementation (thanks [Sung Kim](https://github.com/hunkim))

1. Install Python3 TensorFlow API [Documentation](https://www.tensorflow.org/api_docs/)
2. Download [Multi-layer Recurrent Neural Networks package](https://github.com/hunkim/word-rnn-tensorflow)
3. Replace the `input.txt` file in the TinyShakespeare folder of MRNNP with your training text
4. Run `train.py` (this will take several hours/computing power, check CPU/GPU temperature and whatnot)
5. Parse in sample.py [For more details](https://github.com/hunkim/word-rnn-tensorflow#beam-search)

### NLTK-Chomsky Cheating featuring Dante from the Devil May Cry Series
 
According to context-free generative grammar, a sentence is -more or less- a specific (terminal or leaf) node of an abstract *generator of sentences* or root node (don't quote me on this).

So, in that regard, from a grammatical point of view a sentence like *John eats a pineapple* is no different than *Dante kills demons*
. Of course the meaning is very different, but that's semantics and not grammar, duh.

Our goal is to parse our text in such a way that we can correctly write sentences following a series of predetermined grammatical  structures or rules using a parse tree such as:

![Hm](http://2.bp.blogspot.com/-0OviR_gBISo/U1JJAi67DaI/AAAAAAAAAnE/BSGxu60uDbo/s1600/tree+why_graphs002.png "Chomsky Tree")

So, in simpler words:
* We want to create a repeating sentence structure that we can organize our sentences with.
* Use the same subjects in a cohesive way such that the whole text makes sense (this one is tricky and to be honest I'm not sure)
* Add variation to the sentence structure so our text won't be the same phrase repeated 10 times with different subjects.

In this context, we would like an algorithm that avoids texts like:
 
 *Dante is a demon hunter. Dante likes to kill demons. This text features Dante. Dante is from the Devil May Cry Series. I love Dante. Dante loves Dante.*
 
A way to fix this would be pre-classifying verbs/adjectives/nouns we would like our text to feature in a JSON file, then pick at random. Even when this defeats the whole purpose of machine learning, let's try tackling this as easy as possible:

* Select an appropiate NLTK corpus (a collection of tagged words and texts) *this won't be a problem since as I suggested, you downloaded the whole NLTK packag*, for literature I recommend either Brown or Gutenberg. 
* Tag all the words you already tokenized from your training texts
* Select at random according to previously determined criteria such as topic, subject, etc
* Order and re-write according to grammatical structure following `nltk.tag` tag convention

Common NLTK tags

Tag| meaning | examples
--- | --- | ---
ADJ | adjective | cool, bad, great
ADV | adverb | really, early
CONJ | Conjunction | but, though, because
NP | Proper name | Dante, Africa, Toto
N | Common name | book, duck, demon
VERB | Verb | hunt, kill, talk
PRON | Pronoun | he, she, it, they (reeee)
DET | Determinant | The, a, some

The classic NLTK tagging convention is a tuple as follows ('Word','tag')



As you can probably tell, there are some combinations that make readable sentences, and some that don't; simple English sentence structures would be:

DET | N | VERB  *The dog walks*

N | VERB | ADJ *Dante is cool*

As you can also probably tell, there's infinite combinations that either add or substract a plethora of elements that are optional in a sentence, such as adverbs or determinants. [More info](https://en.wikipedia.org/wiki/Phrase_structure_rules). 



```python
from nltk.corpus import brown


```



### Further reading

[Universal Tagset](http://www.lrec-conf.org/proceedings/lrec2012/pdf/274_Paper.pdf)

[NLTK Tagging Chapter](https://www.nltk.org/book/ch05.html)

