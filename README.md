% 近況報告@tokushima.app-6
% ka ([kaosfield](http://www.kaosfield.net))
% 2015-09-06

# Agenda

- プリキュアハッカソン
    - rubicure_fuzzy_match
        - fuzy_match
        - test-unit
- Elixir
    - Programming Elixir
    - Phoenix
- Make gems
    - color-scheme-validator
    - conoha
- デスクトップ改善

# プリキュアハッカソンに参加してきました

[プリキュアハッカソン 3 - connpass](http://connpass.com/event/17280/)

<img src="precure-hackathon-3.png" height="300px">

プリキュアの映画を見ながらテンションを高めることにより進捗を得る

**最高感ある**

# rubicure_fuzzy_match

Rubicureというgemに曖昧検索機能を追加するgemを作った

本体を改造するのではなく後からメソッドを追加する方針

それを読み込むだけでrubicureもそのまま使えるし追加機能も使える

`rspec-rails`とか`factory_girl_rails`のノリ

- リポジトリ: [kaosf/rubicure_fuzzy_match](https://github.com/kaosf/rubicure_fuzzy_match)
- スライド: [Rubicureをあいまい検索対応強化してみた](http://kaosf.github.io/rubicure-fuzzy-match-slide)

# かなり適当な検索で作品を取ってこれる

```ruby
Rubicure::Seriese.fuzzy_find 'ss'
#=> {:series_name=>"splash_star", :title=>"ふたりはプリキュア Splash☆Star", :started_date=>Sun, 05 Feb 2006, :ended_date=>Sun, 28 Jan 2007, :girls=>["cure_bloom", "cure_egret"]}

Rubicure::Seriese.fuzzy_find 'ゴプリ'
#=> {:series_name=>"go_princess", :title=>"Go!プリンセスプリキュア", :started_date=>Sun, 01 Feb 2015, :girls=>["cure_flora", "cure_mermaid", "cure_twinkle", "cure_scarlett"]}

Rubicure::Seriese.fuzzy_find 'ハト'
#=> {:series_name=>"heart_catch", :title=>"ハートキャッチプリキュア！", :started_date=>Sun, 07 Feb 2010, :ended_date=>Sun, 30 Jan 2011, :girls=>["cure_blossom", "cure_marine", "cure_sunshine", "cure_moonlight"]}

Rubicure::Seriese.fuzzy_find '姫プリ'
#=> {:series_name=>"go_princess", :title=>"Go!プリンセスプリキュア", :started_date=>Sun, 01 Feb 2015, :girls=>["cure_flora", "cure_mermaid", "cure_twinkle", "cure_scarlett"]}
```

# 作品名の正規化

`Rubicure::Seriese.regularize` メソッドで正規化が出来る

```ruby
Rubicure::Seriese.regularize 'splashstar' #=> "ふたりはプリキュア Splash☆Star"
Rubicure::Seriese.regularize 'スマプリ' #=> "スマイルプリキュア！"
Rubicure::Seriese.regularize 'スマイプリ' #=> "スマイルプリキュア！"
Rubicure::Seriese.regularize 'ハピチャ' #=> "ハピネスチャージプリキュア！"
```

# fuzzy_matchという面白そうなgem

あいまい検索が出来るようになる

[seamusabshere/fuzzy_match](https://github.com/seamusabshere/fuzzy_match)

```ruby
require 'fuzzy_match'

fm = FuzzyMatch.new ['プリキュア', 'ナージャ']
fm.find 'ナンジャ' #=> "ナージャ"
fm.find 'プリプリ' #=> "プリキュア"
fm.find 'どれみ'   #=> nil
```

# test-unit

RSpecを無理して使う時代ではもう無い

test-unitを使ってみている


[Ruby - Test::Unitでテストを書く - Qiita](http://qiita.com/repeatedly/items/727b08599d87af7fa671)

# データ駆動テスト出来る

```ruby
class TestRubicureFuzzyMatch < Test::Unit::TestCase
  data(
    "ふたり"      => ["ふたり"     , "ふたりはプリキュア"            ],
    "初代"        => ["初代"       , "ふたりはプリキュア"            ],
    ...
    "姫プリ"      => ["姫プリ"     , "Go!プリンセスプリキュア"       ])
  test '.regularize' do |data|
    assert_equal data[1], Rubicure::Seriese.regularize(data[0])
  end
  ...
end
```

# Elixir

Erlang VMの上で動くプログラムを書くためのRuby風の文法を持った言語

# 書籍

[The Pragmatic Bookshelf | Programming Elixir](https://pragprog.com/book/elixir/programming-elixir)

<img src="elixir.jpg" height="300px">

洋書だけど頑張って読んでる

(RubyとClojureとOCamlとHaskellの経験のおかげでコードは割と楽に読めている)

# Phoenix

[Phoenix](http://www.phoenixframework.org/)

- 1.0.0が8月の末に出たばっかりのWeb Application Framework
- ユーザ認証まわりとかライブラリを調べないと感
- <img src="korekaramainichi.png" height="300px">

# Make gems

最近いろいろgemを作っている

gemはどんどんカジュアルに作っていって良い

# color-scheme-validator

リポジトリ: [kaosf/color-scheme-validator](https://github.com/kaosf/color-scheme-validator)

rubygems.org: [color-scheme-validator | RubyGems.org | your community gem host](https://rubygems.org/gems/color-scheme-validator)

Rails で Color Scheme (#FF0000 とか #00ffff とか) のバリデーションを行う

カスタムバリデータ(と言うらしい)

# 使い方

```ruby
class Item < ActiveRecord::Base
  validates :name, presence: true

  validates :color, color_scueme: true
end
```

# rails g も調子に乗って出来るようにしてみた

```sh
rails g colorscheme install ja
```

自動で `config/locales/colorscheme.ja.yml` が作られる

# conoha

リポジトリ: [kaosf/conoha](https://github.com/kaosf/conoha)

ConoHa の VPS で以下のことが簡単に出来るように

- VPS一覧表示
- VPSを作る
- VPSを破棄する
- bootする
- shutdownする
- イメージを保存する(backup)
- イメージからマシンを作る(restore)
- ssh起動する
- などなど

# 使い方

作る

```sh
conoha create ubuntu g-1gb
```

一覧確認

```sh
conoha vpslist
```

バックアップ

```sh
conoha imagecreate 01234567-89ab-cdef-0123-456789abcdef ubuntu-backup
```

復元

```sh
conoha createfromimage fedcba98-7654-3210-fedc-ba9876543210 g-1gb
```

# API

`Conoha`クラスに各種機能を実装してある

先ほどの`conoha`コマンドは単にそれを実行するだけ

なので仮にクラウドサービスを作るとしてそのバックエンドを担うことが出来る

```ruby
Conoha.init!
Conoha.create "01234567-89ab-cdef-0123-456789abcdef", "g-1gb"
```

# まだまだ作成中

betaバージョンなので誰か使って下さい

機能は必要最小限あるがhelp messageが無かったりエラーへの対処が弱かったり

# デスクトップ改善

作業環境を改善した

# 買ったもの

[Amazon.co.jp： ユニットコム　凄腕　UNI-LCD-ARM-VDUAL　液晶モニタアームアーム　VESA規格75/100mm対応 縦DUALモニタ クランプ固定式: パソコン・周辺機器](http://www.amazon.co.jp/dp/B00B7YMHIG)

<img src="arm1.jpg" height="300px"> <img src="arm2.jpg" height="300px">

# 夢のモニタ二台縦置き

<div>
  <img src="2-vertical-monitors.jpg" height="450px" style="float:left">
  <ul class="incremental" style="float:left">
  <li>上部にテレビを映せる</li>
  <li>下部がPCのサブモニタ</li>
  <li>**実況が捗る！！**</li>
  </ul>
</div>

# 高い位置のモニタを使うメリット

- 椅子の背もたれに頭を完全にあずけて上を仰ぎ見る形でも難なく作業が出来る
- リクライニングを倒した時に丁度目線の位置にモニタがある
- **最高感ある**

# 更によかったこと

- なんとこのモニタアーム6000円でした
- 実際安い
- <img src="http://www.tendertown.net/mt/pollyanna/pollyanna001chan.gif" height="300px"> ([愛少女ポリアンナ物語（世界名作劇場）ファンサイト by 名作アニメの杜](http://www.tendertown.net/mt/pollyanna.html))

#

いろいろあった8月と9月初頭でした

#

おわり
