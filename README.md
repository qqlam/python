# python
import requests
from bs4 import  BeautifulSoup
url="http://news.sina.com.cn/"
resurl=requests.get(url)
resurl.encoding='utf-8'
soutop=BeautifulSoup(resurl.text,'html.parser')
for leis in soutop.select('.cNav2'): #新闻分类
    title=[]
    titleurl=[]
    for leibie in leis.select('a'):
        title.append(leibie.select('span')[0].text)
        titleurl.append(leibie['href'])
    #print(title,titleurl)
    n=0
    for newstitle in title:
        newstitle=title[n]
        newsurl=titleurl[n]
        n += 1
        print(newstitle)
        res=requests.get(newsurl) #准备爬大类里的小类
        res.encoding='utf-8'
        soup=BeautifulSoup(res.text,'html.parser')
        for news in soup.select('.news-item'):
            if len(news.select('h2'))>0:
                h2=news.select('h2')[0].text
                a=news.select('a')[0]['href']
                time=news.select('.time')[0].text
                resa = requests.get(a) #爬具体内容
                resa.encoding = 'utf-8'
                soupa = BeautifulSoup(resa.text, 'html.parser')
                anewstitle = soupa.select('#artibodyTitle')[0].text
                anewstime = soupa.select('.time-source')[0].contents[0].strip()
                newsbody = []
                for p in soupa.select('#artibody p')[:-1]:
                    newsbody.append(p.text.strip())
                for body in newsbody:
                    print(body)
