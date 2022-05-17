# Description
The Gods of Mars is a science fantasy novel by American writer Edgar Rice Burroughs and the second of Burroughs' Barsoom series. It features the characters of John Carter and Carter's wife Dejah Thoris. It was first published in The All-Story as a five-part serial in the issues for January–May 1913. It was later published as a complete novel by A. C. McClurg in September, 1918 and in many editions subsequently.

This project aims to analyze the first two paragraphs, which is about 1080 lines including blank lines from the whole document.

## Part 1 Analysis Preparation
For this analysis, we start by loading the document “The Gods of Mars”. We use VCorpus function to load the entire document into memory, while ignore case for the longest words and sentences analysis. The book contains 9313 lines and 443,046 characters of text in total.

After creating a new folder named ‘chapters’, I apply the which() function to identify the starting points for Chapters 1, 2, and 3. Next, the write.table() is used to save my newly sliced chapters into the 'chapters' folder. The total number of lines in Chapter 1&2 is 917 (445 lines in Chapter 1 and 472 lines in Chapter 2) including blank lines. Note that I didn’t take the content before Chapter 1 into consideration, like Title, Author, Foreword and Contents, which contains 161 lines in total. The titles of each Chapter, like ‘CHAPTER I’ and ‘CHAPTER II’, were also omitted. Therefore, the number of lines before ‘CHAPTER III’ should be 1080 including blank lines.

## Part 2 Find the Longest Words and Sentences
I created two functions to get the 10 longest words (Table 1) and the 10 longest sentences (Table 2) before removing the punctuation.

![1652822367(1)](https://user-images.githubusercontent.com/90291484/168911892-8e341dae-e9b1-4018-85b7-194de3411639.png)

![1652822564(1)](https://user-images.githubusercontent.com/90291484/168912348-e3651790-fd32-44d3-bd9b-82f88f01e7da.png)

## Part 3 Apply Natural Language processing (NLP) Techniques
I firstly compute document term matrix (DTM) and term document matrix (TDM), as well as examine the structure and inspect the data. The output signifies that the two documents contain 2263 terms which have appeared at least once. 1766 cells in frequencies are 0, 2760 cells have non-zero values. The sparsity is 39%, which means that 39% (=1766/(2760+1766)) of all cells in the matrix are zero. Some sample words with high frequency are listed, like ‘the’, which appears 328 times in chapter 1, and 309 times in chapter 2.

Then I apply functions to wrangle data: First removing numbers and punctuations, then converting every character into lower case. After data wrangling, the number of terms reduced by 390 (=2263-1873), andtThe sparsity reduced from 39% to 37%.

Stop words are considered uninformative to my analysis, so I want to remove them. This can be done easily using the tm package. The package provides a list of stop words in English that can be easily filtered out using removeWords. Then I inspect the TDM again and draw dendrograms to check the distribution of clusters (Figure 1). 

![1652822945(1)](https://user-images.githubusercontent.com/90291484/168913172-f05a3674-6218-460d-99a4-cca49381cc46.png)

The resulting dendrogram is quite cluttered, so I need to eliminate more words to get a good dendrogram. I choosing some additional terms manually by looking through the most frequent terms and identifying those that were uninformative. The result dendrograms is shown as Figure 2. I remove 86 (=1873-1787) words from the documents and get a new TDM as well as increasing the sparsity from 37% to 39%; After removing the new stop words, the most frequent terms in both chapters seem to contribute more to the understanding the theme of the book. For example, ‘Tarkas Tars’ might be a name of a man, since the two words appear the same number of times in chapters 1 and 2.The new dendrogram is much better compared with the old one, but it is still too difficult to interpret.

![1652823246(1)](https://user-images.githubusercontent.com/90291484/168913845-44bfb9c6-2041-449a-85ca-065883cb1070.png)

To quickly perceive the most prominent terms, and try to get and understanding of each chapter from them, I can create word clouds. The result is shown in Figure 3.

![1652823362(1)](https://user-images.githubusercontent.com/90291484/168914122-81702603-d8d8-49e3-be38-9bdd6365461d.png)

I apply ‘quanteda’ package and compute the term frequency-inverse document frequency (tf-idf) score, with full control over options (Figure 4). There are 361 lines with 1071 features in chapter 1 and 384 lines with 1068 features in chapter 2 (with blank lines removed). For interpreting the tf-idf score, the larger the score, the more useless the word. So, by calling dfm_tfidf function, I can filter the noise using these scores. Like in chapter 2, ‘tars’ and ‘tarkas’ are more important than ‘forest’ or ‘battle’.

![1652824804(1)](https://user-images.githubusercontent.com/90291484/168917114-443d29e0-2d56-4943-b75b-06ab73ef9a39.png)

Then I apply ‘syuzhet’ package for sentimental analysis. By calling get_sentiment function, I can get an idea about the sentiment of each sentence in documents. The larger and more positive number is, the more positive sentiment the sentence is. For example, the last sentence of chapter 1 may have the most positive sentiment in the whole chapter. Both chapters are in negative sentiment with negative sum and mean value of sentiment vector, while chapter 2 seeming especially negative.

Finally, I apply get_percentage_values function to compare the shape of one trajectory to another and draw the plot (Figure 5). From the plots, I can get the distribution of emotions as story goes by. There’s a positive emotional intensity at the first 5% to 10% of chapter 1, but after that, sentiment becomes negative. Chapter 2 appears to contain more negative sentiment than positive, which confirms my results from earlier.

![1652825262(1)](https://user-images.githubusercontent.com/90291484/168918506-329e0562-8017-4845-b54b-a506706dfe46.png)

## Part 5 Conclusion
For this project, I loaded the document “The Gods of Mars” and separated first two chapters into a VCorpus for text analysis. By calling ‘str’ and ‘inspect’ functions, I can easily get the basic idea of the structure and content inside the corpus. 

I wrote functions to find 10 longest words and sentences, which developed my skills on manipulating text file. Document term matrices (DTM) and term document matrices (TDM) are two ways to express the text mathematically. I practiced with creating and reading both of these.

For the data wrangling section, removing numbers and punctuations is often necessary, as well as making sure that everything is in lower case. An important indicator ‘sparsity’ refers to how many of the values in the matrix are zero. Meanwhile, I should try to reduce the noise in the text, i.e., useless but frequently appearing words, by removing stop words. World clouds help me to quickly perceive the most prominent terms and locate the relatively prominent themes. There are many ways to choose stop words, depending on the situation, but the packages used provide easy functionalities. The dendrogram is a good method to check if I removed enough stop words. These methods in data cleaning for text analysis are valuable skills that I have practiced.

I also applied natural language processing (NLP) techniques, like text analysis and sentiment analysis with the ‘quanteda’ and ‘syuzhet’ packages respectively. For text analysis, tf-idf score can provide me with a clear sense of which words are most important. The larger the score, the more useless the word. As for sentiment analysis, I can have a feeling about the emotions in a document according to the values after applying a sentiment dictionary. Large, positive number indicate a high amount of positive sentiment. Both the two analyses help me to have a better understanding of the theme and content of the book. These methods can easily be applied to future analysis as well. 
