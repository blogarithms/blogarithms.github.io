---
layout: post
title: "Building Scoper"
excerpt: "My most recent project - fuzzy & semantic searching for captioned YouTube videos"
categories: [development, algorithm]
image:
    feature: 
comments: true
---

If you haven't yet had the chance to check out my most recent project, Scoper, here it is - <span style="color:blue;"><a href="https://github.com/RameshAditya/scoper">https://github.com/RameshAditya/scoper</a></span>

The tl:dr; version of Scoper is that it lets you search for a specific timestamp of a video, if you tell it what you're looking for.

A bit more formally, Scoper pulls the captions of a YouTube video whose URL you specify, and also asks you for what you're looking for in the video.

With this information, it uses NLP algorithms to determine which caption is the most similar to what you're looking for, and returns the corresponding timestamp.

I ended up breaking down the development of this project into a few steps -
- Retrieve the captions of the specified video
- Tokenize the captions and generate my set of documents
- Implement a fuzzy matching algorithm
- Implement a semantic matching algorithm
- Map the K most similar captions to their original timestamps
- Pretty print this back to the user

Retrieving the captions of the video was fairly simple using the `youtube_transcript_api` python module. I just had to plug in the YouTube URL's video ID, and the language of the captions I wanted it in.

*Note: The API throws an exception in case the video ID doesn't exist, or if the language specified does not have captions present.*

But asking the user for the video ID itself directly, didn't seem user-friendly. 

From my experience, good applications offload the responsibility from the user onto themselves wherever possible.

So I decided to find a way to parse YouTube URLs to extract an ID from a generic YouTube URL.

This would be simple, except as I found out, YouTube URLs exist in multiple formats. 

Here are a few possibilities, which all point to the same video -

```
http://www.youtube.com/v/0zM3nApSvMg?fs=1&hl=en_US&rel=0
http://www.youtube.com/embed/0zM3nApSvMg?rel=0
http://www.youtube.com/watch?v=0zM3nApSvMg&feature=feedrec_grec_index
http://www.youtube.com/watch?v=0zM3nApSvMg
http://youtu.be/0zM3nApSvMg
http://www.youtube.com/watch?v=0zM3nApSvMg#t=0m10s
```

The video ID above is `0zM3nApSvMg`, as you can probably tell. But this is about figuring out how to get my code to also be able to tell.

And some thinking and research later, we have -

**One regex to rule them all.**

```
^.*(youtu.be\\/|v\\/|e\\/|u\\/\\w+\\/|embed\\/|v=)([^#\\&\\?]*).*
```

This was implemented in code as -

```python
youtube = r'(youtu.be\/|v\/|e\/|u\/\w+\/|embed\/|v=)'
video_id = r'([^#\&\?]*).*'
https = r'^.*'

parsed_url = re.search(https + youtube + video_id, youtube_video_url)

return parsed_url[2]
```


Once this was done, I decided to use gensim's tokenize method to directly tokenize each caption from the video's captions.

Now my list of documents were ready and the next step was to implement the algorithms that compute the similarities between the query string and the captions.

------------------------------------

### Fuzzy Matching

The intent of supporting fuzzy matching was on the off-chance that the viewer might be searching for something specifically, maybe with a spelling they may not know - or a potential Google auto-captioning misspelling.

Fuzzy matching returns the caption closest to the user query string using variants of the levenshtein distance algorithm as a metric of similarity.

While I could've implemented levenshtein distance with DP efficiently, I decided to use fuzzywuzzy's `extractBest` method. (Why reinvent the wheel, right?)

```python
from fuzzywuzzy import process
similar_strings = process.extractBests(query, corpus, limit=limit)
```

### Semantic Matching

Now this one was a bit more tricky.

There's no "established" way, to determine the semantic similarity between two sentences yet.

Word-to-word similarity is pretty well explored in NLP, with solutions like Word2Vec, fastText and GloVe, but these approaches only generate word embeddings.

I needed sentence embeddings to make my work simpler.

So I started doing some research, and I encountered the word-mover distance algorithm, but noticed that its performance worked best when comparing sentences of similar lengths.

I looked at other approaches as well, like averaging the word embeddings of each sentence and then finding the cosine similarity of the resultant vectors, or adding the word vectors and then computing the similarities, but they didn't seem to be too intuitive for me.

Ultimately, I decided to implement my own variant of the word-mover distance, where I decided to map each word embedding of sentence A to its nearest match in sentence B, and for every generated pairing, mark both these words as "used", and repeating the matching process for the next token in A while only selecting from unused tokens in B.

```python
sentence_1 = list(tokenize(sentence_1))
sentence_2 = list(tokenize(sentence_2))

visited_1 = [0]*len(sentence_1)
visited_2 = [0]*len(sentence_2)

similarity = 0

# The algorithm used below is a modified word-mover's distance algorithm.
# It is asymmetric, and for each word in sentence_1, we find the closest
# un-mapped word in sentence_2 and add this similarity.
for idx_a, word_a in enumerate(sentence_1):
    if self.vocabulary[word_a] == 1:
        visited_1[idx_a] = 1
        closest_distance = 1e18
        idx_chosen = -1
        for idx_b, word_b in enumerate(sentence_2):
            if visited_2[idx_b] == 0 and self.vocabulary[word_b] == 1:
                current_distance = (1 - self.model.similarity(word_a, word_b))
                if idx_chosen == -1 or current_distance < closest_distance:
                    closest_distance = min(closest_distance, current_distance)
                    idx_chosen = idx_b

        if idx_chosen != -1:
            visited_2[idx_chosen] = 1

        similarity += closest_distance

# Divide by len(sentence_1) to normalize by length
return similarity/len(sentence_1)
```


While I'm certain there exist more complex and accurate sentence similarity approaches, what's cool about this method is that it's computationally lightweight.

This coupled with the fact that the average user of Scoper would be someone who may not necessarily have dedicated hardware to support deep learning/neural network applications etc, further solidified this approach.

----------------------------------------------------------------------------------

Now that I could compute similarities, all that was left was to return the K most-similar ones, and correspondingly, pretty-print them to the user.

After having done this, I refactored the code and added some documentation to improve readability.

And that was it! 

Feel free to check it out, and you can find it here - <span style="color:blue;"><a href="https://github.com/RameshAditya/scoper">https://github.com/RameshAditya/scoper</a></span>

----------------------------------------------------------------------------------------

For more of my work, follow me on:

LinkedIn: <span style="color:blue">[https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)</span>

GitHub: <span style="color:blue">[https://github.com/RameshAditya/](https://github.com/RameshAditya/)</span>

Twitter: <span style="color:blue">[https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)</span>