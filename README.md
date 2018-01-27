
# crawling with nokogiri gem_rails
+ require 'open-uri'를 컨트롤러 위에 추가
```
+ url="http://finance.naver.com/marketindex/?tabSel=exchange#tab_section"
+ doc = Nokogiri::HTML(open(url))
```
+ 파싱은 html값을 가져오는것, 크롤링은 특정값을 가져오는것
+ https://github.com/sparklemotion/nokogiri 참고
+ doc.css('span.class')
+ doc.css('span#id')
+ ex)
+ currency=doc.css('div.head_info > span.value')
+ currency[0].text로 값접근

## 컨트롤러

```ruby
  def index
    url="http://finance.naver.com/marketindex/?tabSel=exchange#tab_section"
    doc = Nokogiri::HTML(open(url), nil,'euc-kr')
    currency = doc.css('div.head_info > span.value')
    @new_currency = currency.map {|cur| cur.text}
    currency_name= doc.css('h3.h_lst')
    @new_currency_name =currency_name.map {|cur_n| cur_n.text}
  end
```

## view

```ruby
<% @new_currency.each_with_index do |currency,index| %>
<p><%=index%>: <%= index %><%=@new_currency_name[index] %> <%=currency%></p>
<% end %>
```