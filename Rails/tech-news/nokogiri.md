# 概要
gem nokogiri を用いた複数サイトからのスクレイピング

controller
```ruby:links_controller.rb
class LinksController < ApplicationController
  # URLにアクセスするためのライブラリの読み込み
  require "open-uri"
  # gem nokogiriの読み込み
  require "nokogiri"
  require "robotex"
  
  def index
    # スクレイピングするサイトurlの配列を作成
    urls = [
      "https://jp.techcrunch.com/",
      "https://www.gizmodo.jp/articles/",
      "https://electrek.co/",
      "https://www.tesmanian.com/",
      "https://www.macrumors.com/",
      "https://towardsdatascience.com/"
    ]

    @docs = []
    urls.each do |url|
      robotex = Robotex.new
      p robotex.allowed?(url)

      user_agent = 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/28.0.1500.63 Safari/537.36'
      charset = nil
      html = open(url, "User-Agent" => user_agent) do |f|
        charset = f.charset # 文字種別を取得
        f.read # htmlを読み込んで変数htmlに渡す
      end
      
      # htmlをパース(解析)してオブジェクトを作成
      doc = Nokogiri::HTML.parse(html, nil, charset)
      @docs << doc
    end
  end

end
```
index.html.haml
```ruby:index.html.haml
.contents
      - @docs[0].css('ul > li > div > div.block-content > span > a > img').each do |link|
        .links
          .img
            = link_to link.parent[:href] do
              = image_tag link["data-src"], class: "imageSize"
          .link
            = link_to link.
              = link[:alt]
```
