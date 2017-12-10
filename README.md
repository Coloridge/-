#  爬虫
import requests
from requests.exceptions import RequestException
import re
import json

def get_one_page(url):
    try:
        response = requests.get(url)
        if response.status_code == 200:
            return response.text
        return ''
    except RequestException:
        return ''

def parse_one_page(html):
    pattern = re.compile('<div.*?info-desc">(.*?)</div>',re.S)
    items = re.findall(pattern,html)
    for item in items:
        yield {
               'introduce':item.strip()
               }
def write_to_file(content):
    with open('G:\\result.text','a',encoding='utf-8') as f:
        f.write(json.dumps(content,ensure_ascii = False) + '\n')
        f.close()
        
def main(num):
    url = 'https://bangumi.bilibili.com/anime/' + str(num)
    html = get_one_page(url)
    for item in parse_one_page(html):
        print(item)
        write_to_file(item)
   

if __name__ == '__main__':
    for i in range(3000):
        main(i)
