# Word-cloud-displays-of-film-reviews-crawling-from-Douban-
Crawl the film reviews from Douban to display the word cloud
# Crawl data from Douban
urllib.request and BeautifulSoup are the two most basic packages in the Python crawler, and BeautifulSoup allows us to easily extract the contents of HTML tags and crawl the specified data.<br>
The decoding method is based on the Utf-8<br>

## See what the latest release movies
```
from urllib import request
resp = request.urlopen('https://movie.douban.com/nowplaying/wuhan/')                   
html_data = resp.read().decode('utf-8')
from bs4 import BeautifulSoup as bs
soup = bs(html_data, 'html.parser')    
nowplaying_movie = soup.find_all('div', id='nowplaying')
nowplaying_movie_list = nowplaying_movie[0].find_all('li', class_='list-item')
```
Though this we can get the list of the film being released, take the nowplaying_movie_list[0]<br>
the latest movie is the Mission: Impossible - Fallout<br>

## View Short Reviews
```
resp1 = request.urlopen('https://movie.douban.com/subject/26336252/comments?status=P')
html_data1 = resp1.read().decode('utf-8')
```
## Perform label parsing
Display the original contents of the page HTML and we can see that the short comment item is in the<div class=""comment""><br>
So we perform the label parsing with BeautifulSoup pakage
```
soup = bs(html_data, 'html.parser') 
comment_div_lits = soup.find_all('div', class_='comment')
```
## Extracting child elements
First find <span>the element of class= ' short ' and continue to get the text inside the element internally
```
eachCommentList = []
for each in comment_div_lits:
    item = each.find_all('span',class_='short')
    for eachs in item:
        txtstr = eachs.string
        eachCommentList.append(txtstr)
```
## The above is a brief data crawl work. Next, we need to clean the crawled data.
## Data cleaning
Remove line breaks and spaces<br>
To facilitate the cleaning of the data, we place the data in the obtained list in an array of strings for easy character processing
```
comments = ''
for n in range(len(eachCommentList)):
comments = comments + (str(eachCommentList[n])).strip()
```
## Regular expression clears the punctuation
```
import re
pattern = re.compile(r'[\u4e00-\u9fa5]+')#至少匹配一个汉字
filterdata = re.findall(pattern,comments)
cleaned_comments = ' '.join(filterdata)
```
## Remove spaces between words
```
cleaned_comments=cleaned_comments.replace(' ','')
```
<br>
## Word Frequency statistics
```
segment = jieba.lcut(cleaned_comments)
words_df=pd.DataFrame({'segment':segment})
```
![word
