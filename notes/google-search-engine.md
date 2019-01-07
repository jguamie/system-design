# Google Search Engine
These notes are for the 1998 prototype of the Google large-scale search engine. Google's name is derived from the common spelling of googol--the large number 10<sup>100</sup>.
## History
In 1998, Yahoo was the leading search engine. To have high quality indices, Yahoo used a manual approach to do so. Popular topics were subjective, expensive to build, slow to update, and did not cover obscure topics. 

Automated search engines returned low quality matches. Advertisers were able to game the system to mislead these engines. Users were accustomed to having to look through at least ten results to find at least one relevent result.
## Design Goals
1. **Web Scale.** At the rate the Web was growing, fast crawling is required to be able to process hundreds of gigabytes efficiently. Search queries should have low latency at a throughput of thousands of queries per second.
1. **Search Quality**. Users want to see only the most relevant results.
1. **Academic Research**. Current search engines catered to commercial entities and advertisers. Researchers need a means to process large portions of the Web to produce meaningful, academic results.
## PageRank
PageRank utilizes link structure and anchor text (the visible, clickable text in a hyperlink) for relevance and quality filtering. PageRank is a model of user behavior. The quality and importance of a page can be measured primarily by counting the number of links to it from other pages. Pages that have a high PageRank can be considered to have higher quality links.
### Anchor Text
Research has found that anchor text provides the most accurate descriptions of Web pages, more so than the Web pages' description of itself. Using anchor text allows for higher quality categorizations of Web pages.

Anchor text allows for categorization of non-text content such as images, videos, programs, and databases. Non-text content cannot be indexed by a standard text-based search engine. Anchor text from other pages opens up that possibility.

Anchor text expands the search coverage of downloaded documents. In Google's crawl of 24 million pages, more than 259 million anchors were indexed--about 10x more coverage.
## Google Architecture
<img src="https://github.com/jguamie/system-design/blob/master/images/google-architecture.png" align="middle" width="45%">

Most of Google is implemented using C/C++ in a Linux environment.
1. **Crawlers**. Several distributed crawlers are responsible for downloading Web pages.
1. **URL Server**. The URL server sends lists of URLs to be fetched to the crawlers. Other systems refer to this as the URL Frontier.
1. **Store Server**. The store server compresses and stores the Web pages into a repository. Each Web page is assigned a docID.
1. **Indexer.** 
# References
1. Brin, Sergey, and Lawrence Page. "The Anatomy of a Large-Scale Hypertextual Web Search Engine." 1998. Accessed January 07, 2019. http://infolab.stanford.edu/~backrub/google.html.
