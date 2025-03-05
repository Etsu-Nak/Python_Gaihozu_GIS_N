# はじめに
このプログラム群は、Stanford大学が公開する5万分の1の地図データをGISデータ化するためのプログラムです。
GISデータ化用のプログラムは、基本的に100枚以上を処理することを前提にしています。
現時点では、100枚処理する際に、25枚程度は手作業の処理が入ります。


## 前提　～　データをダウンロードする前にお読みください
緯度経度値の判読の補正を行う為に、Stanford大学のデータをダウンロードする際の準備が必要となります。
Stanford大学の5万分の1の地図のダウンロードサイトでは、日本全体の白地図が地図範囲を示す四角で区切られています。
その四角の一つをクリックすると、Sheet Number	NJ-53-6-3のような数値が出てきます。
このNJ-53-6-3をコピーしておき、ダウンロードする画像の名前を、NJ-53-6-3.jpgに変更します。
必要なだけの画像が溜まったら、ハイフンをアンダーバーに変換して下さい。
（フォルダ内のファイル名の一部を一括変更、でできます）

## 地図画像の分類
外邦図のうち、北海道東部と南西諸島は、大正7年の日本の経度修正の後に作成されています。従って、他の場所の地図の経度に必ず記されている10.4″の記述がありません。
北海道のうち、NL-54-16-4 EASTから	NK-54-9-14までの縦の一列は、左側は10.4″の補正値がついており、右側は補正値無し、という他の地図より10.4″経度方向が短い地図になっています。
これより東の地図群は補正無し、西は補正あり、となります。
また、南西諸島については、NH-52-8-4 EAST以南の九州の島嶼部のうち、NG-52-4-9/13、NG-52-6-13以外が補正無しにあたります。
補正無しの地図については、他の地図と別にして下さい。

# プログラムの使用用
## Preprocessing_of_Gaihozu_japan.ipynb

