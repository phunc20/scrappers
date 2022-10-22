`scrapy shell` for vnexpress

```python
response.css("nav").css("ul").css("li").getall()

response.xpath("//nav/ul/li")
```



## Article Page

In [8]: response.css("p.description::text").get()
Out[8]: 'Việc Hà Hồng Lệ dứt tình để yêu công tử nhà giàu khiến Trang cay cú nhưng vẫn không thể quên cô, dù đã lấy vợ.'  

response.css("p.Normal::text").getall()


## Q&A
1. Formatted texts (e.g. `<b>important</b>`, `<em>serious</em>`) in `text` are excluded; how could we keep them?
    - [x] Cf.
        - <https://stackoverflow.com/questions/753052/strip-html-from-strings-in-python>
          ```python
          from io import StringIO
          from html.parser import HTMLParser
          
          class MLStripper(HTMLParser):
              def __init__(self):
                  super().__init__()
                  self.reset()
                  self.strict = False
                  self.convert_charrefs= True
                  self.text = StringIO()
              def handle_data(self, d):
                  self.text.write(d)
              def get_data(self):
                  return self.text.getvalue()
          
          def strip_tags(html):
              s = MLStripper()
              s.feed(html)
              return s.get_data()
          ```
              - <https://stackoverflow.com/questions/9662346/python-code-to-remove-html-tags-from-a-string>
        - 
        ```python
        for paragraph in response.css("p.Normal").getall():
            clean_paragraph = strip_tags(paragraph)
        ```
2. The comment section of vnexpress articles cannot be scraped using pure scrapy
    - Due to javascript
    - [x] One solution is to use playwright to fake our crawler as a browser: <https://www.youtube.com/watch?v=0wO7K-SoUHM>
