# Context-sensitive-Spelling-Correction
implementation of context-sensitive spelling correction. The input of the code will be a set of text lines and the output will be the same lines with spelling mistakes fixed.
# Justification of solution
## N-grams
The primary limitation in Nodvig's solution was the use of a small dataset. To address this, I leveraged much larger datasets from the NLTK library, which provided a more robust base for calculating word probabilities and correcting spelling errors. Additionally, Nodvig's method relied on unigrams, which is less effective for context-dependent words that are related to others in the sentence. 


Additionally, I integrated trigrams into the solution, though I found that they can slow down the algorithm. The impact of trigrams, while noticeable, is not always critical, as the evaluation results with and without trigrams were very similar. Therefore, I left the use of trigrams as an optional feature. If accuracy is the priority, trigrams are valuable, but they can be omitted for speedier performance.

## Keyboard Map and Weights
One important improvement I made was the inclusion of weighted errors. Many common spelling mistakes are caused by the keyboard layout, and so I assigned different weights to various types of errors based on their likelihood of occurrence. These weights were carefully chosen and tested, and here’s how they work:
- Keyboard mistakes (keyboard_replaces): 0.95 (very common errors, such as mistyped adjacent keys)
- Replacements (replaces): 0.8 (frequent but less common than keyboard-related mistakes)
- Transpositions (transposes): 0.5 (less frequent, such as switching adjacent letters)
- Insertions (inserts): 0.3 (rare, when extra characters are typed)
- Deletions (deletes): 0.2 (also rare, where characters are omitted)

This weighted approach allows for better prioritization during the search for potential corrections. It ensures that more frequent errors, such as those arising from adjacent key presses, are given higher priority.

## Improve Speed
Another notable improvement I made was to optimize the edit2 generation process using heapq. Previously, generating all possible candidate edits for the second iteration (edit2) could be computationally expensive. By using a heap, I ensured that only the best candidates were prioritized, and I reduced the impact of low-weighted corrections. 

Also, I implemented caching to speed up the code. All computations are now much faster, as results of frequently repeated operations (such as unigram, bigram, and trigram probabilities) are cached. This avoids redundant calculations and significantly improves performance.

## Best Candidate Function
When selecting the best candidates, I implemented a maximization function, which can be quite large and complex due to the many variables at play. To avoid the issue of probabilities becoming excessively small, I employed logarithms. This ensures that the values stay manageable and helps stabilize the calculation process. Additionally, I adjusted the weights for bigrams and trigrams to give more importance to these models, recognizing their higher contextual value.

## Evaluation Results
The difference between the results is not extremely significant, but my implementation shows much better results for Character Accuracy. I believe this improvement is largely due to the incorporation of bigram and trigram models, which capture more context and help correct mistakes at the character level. Additionally, the speed improvement is quite noticeable, thanks to optimizations like caching and more efficient handling of edit candidates. These changes contribute to both better performance and faster processing.
