1. Python中中文标点处理

   ```html
   http://www.cnblogs.com/arkenstone/p/6092255.html
   https://www.zhihu.com/question/37720196/answer/115870233
   ```

   ```python
   #中文标点转英文标点
   import unicodedata
   t = u'中国，中文，标点符号！你好？１２３４５＠＃【】+=-（）'
   t2 = unicodedata.normalize('NFKC', t)
   print t2
   #中国,中文,标点符号!你好?12345@#【】+=-()
   ```

   ```python
   # 将中英文标点替换为 _ 
   import string
   import re
   from zhon.hanzi import punctuation
   import unicodedata
   pun_cn = set(string.punctuation) # 中文标点
   pun_en = set(punctuation) # 英文标点
   str_pun =  ''.join(pun_cn | pun_en)
   str = ""
   # 1 去掉html标签 - 正则
   str_no_html = re.sub(r'</?\w+[^>]*>','',str)
   # 2 替换中英文标点
   re_pun = re.compile('[%s]' % re.escape(str_pun))
   str_no_pun = re_pun.sub('_', str_no_html)
   # 3 去掉空格
   str_no_space = str_no_pun.replace(" ","")
   print(str_no_space)

   ```

   ​

2. Python中过滤HTML标签的函数

   ```html
   https://gist.github.com/dervn/859717/15b69ef75a04489f3a517b3d4f70c7e97b39d2ec
   ```

   ```python

   #用正则简单过滤html的<>标签
   import re
   str = "<img /><a>srcd</a>hello</br><br/>"
   str = re.sub(r'</?\w+[^>]*>','',str)
   print str
   ```

   ```python
   #用了HTMLParser，有更简单的方式吗？正则？
   def strip_tags(html):
       """
       Python中过滤HTML标签的函数
       >>> str_text=strip_tags("<font color=red>hello</font>")
       >>> print str_text
       hello
       """
       from HTMLParser import HTMLParser
       html = html.strip()
       html = html.strip("\n")
       result = []
       parser = HTMLParser()
       parser.handle_data = result.append
       parser.feed(html)
       parser.close()
       return ''.join(result)
   ```

   ```python
   #更深层次的过滤，类似instapaper或者readitlater这种服务，很有意思的研究课题

   # -*- coding: utf-8-*-
   import re
   ##过滤HTML中的标签
   #将HTML中标签等信息去掉
   #@param htmlstr HTML字符串.
   def filter_tags(htmlstr):
       #先过滤CDATA
       re_cdata=re.compile('//<!\[CDATA\[[^>]*//\]\]>',re.I) #匹配CDATA
       re_script=re.compile('<\s*script[^>]*>[^<]*<\s*/\s*script\s*>',re.I)#Script
       re_style=re.compile('<\s*style[^>]*>[^<]*<\s*/\s*style\s*>',re.I)#style
       re_br=re.compile('<br\s*?/?>')#处理换行
       re_h=re.compile('</?\w+[^>]*>')#HTML标签
       re_comment=re.compile('<!--[^>]*-->')#HTML注释
       s=re_cdata.sub('',htmlstr)#去掉CDATA
       s=re_script.sub('',s) #去掉SCRIPT
       s=re_style.sub('',s)#去掉style
       s=re_br.sub('\n',s)#将br转换为换行
       s=re_h.sub('',s) #去掉HTML 标签
       s=re_comment.sub('',s)#去掉HTML注释
       #去掉多余的空行
       blank_line=re.compile('\n+')
       s=blank_line.sub('\n',s)
       s=replaceCharEntity(s)#替换实体
       return s

   ##替换常用HTML字符实体.
   #使用正常的字符替换HTML中特殊的字符实体.
   #你可以添加新的实体字符到CHAR_ENTITIES中,处理更多HTML字符实体.
   #@param htmlstr HTML字符串.
   def replaceCharEntity(htmlstr):
       CHAR_ENTITIES={'nbsp':' ','160':' ',
                   'lt':'<','60':'<',
                   'gt':'>','62':'>',
                   'amp':'&','38':'&',
                   'quot':'"','34':'"',}
       
       re_charEntity=re.compile(r'&#?(?P<name>\w+);')
       sz=re_charEntity.search(htmlstr)
       while sz:
           entity=sz.group()#entity全称，如&gt;
           key=sz.group('name')#去除&;后entity,如&gt;为gt
           try:
               htmlstr=re_charEntity.sub(CHAR_ENTITIES[key],htmlstr,1)
               sz=re_charEntity.search(htmlstr)
           except KeyError:
               #以空串代替
               htmlstr=re_charEntity.sub('',htmlstr,1)
               sz=re_charEntity.search(htmlstr)
       return htmlstr

   def repalce(s,re_exp,repl_string):
       return re_exp.sub(repl_string,s)

   if __name__=='__main__':
       s=file('Google.htm').read()
       news=filter_tags(s)
       print news
   ```

3. TOADD
