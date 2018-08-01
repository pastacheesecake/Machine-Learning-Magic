# Machine-Learning-Magic
NLTK - Tensorflow (soon)


# Machine Learning Narrative - Poetry

## Requirements

* __Python 3__ (I suggest Anaconda 3)
* NumPy, SciPy, Stats and the like (come with Anaconda)
* __NLTK__, you should download all the corpus and associated modules [NLTK](https://www.nltk.org)
* TensorFlow for Python (for future features) [TensorFlow Python3 API](https://www.tensorflow.org/api_docs/python/)
* Pickle and JSON if wanna get fancy [̲̅$̲̅(̲̅ ͡° ͜ʖ ͡°̲̅)̲̅$̲̅]	

## Poetry

It all depends on what you want to write, my personal method is quite simple:

1. Get a plain-text library of books you wanna cut-up from and save them in your project folder. *Can get PDFs but then you'd need to extract the text and PDF text-extraction in Python 3 is... not the best tbh)*
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
wb.close()
    
    
```

3. Having all your words tokenized, do cut-up magic a-la *Burroughs* or *Dada*, here's my method:
* Randomly select words from your pickle dump/text file using python's random module 
* Filter by number of syllables (use either [Pyphen](http://pyphen.org), `nltk.cmudict` or code your own syllable-counting function like a big boy ( ಠ ͜ʖ ಠ)	
* Apply sentiment analysis (either pre-trained `nltk.sentiment.vader` [Docu](https://www.nltk.org/_modules/nltk/sentiment/vader.html) or train your own TensorFlow neural network like a good hee-ho *more on this later I guess*), yet again filter or do as you please
* *Profit?*
