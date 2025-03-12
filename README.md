# はじめに
このプログラム群は、Stanford大学が公開する5万分の1の地図データをGISデータ化するためのプログラムです。  
GISデータ化用のプログラムは、基本的に100枚以上を処理することを前提にしています。  



## 前提　～　データをダウンロードする前にお読みください
緯度経度値の判読の補正を行う為に、Stanford大学のデータをダウンロードする際の準備が必要となります。  
Stanford大学の5万分の1の地図のダウンロードサイトでは、日本全体の白地図が地図範囲を示す四角で区切られています。
その四角の一つをクリックすると、Sheet Number	NJ-53-6-3のような数値が出てきます。  
このNJ-53-6-3をコピーしておき、ダウンロードする画像の名前を、NJ-53-6-3.jpgに変更します（名前はダウンロードする矩形ごとに変わります）。  
必要なだけの画像が溜まったら、ハイフンをアンダーバーに変換して下さい。
（フォルダ内のファイル名の一部を一括変更、でできます） 
この作業を行わないと、Lat_lon_interpolation_for_Stanford_Gaihozuプログラムが使えません。

## 地図画像の分類
　外邦図のうち、北海道東部と南西諸島は、大正7年の日本の経度修正の後に作成されています。従って、他の場所の地図の経度に必ず記されている10.4″の記述がありません。 
北海道のうち、NL-54-16-4 EASTから	NK-54-9-14までの縦の一列は、左側は10.4″の補正値がついており、右側は補正値無し、という他の地図より10.4″経度方向が短い地図になっています。
これより東の地図群は補正無し、西は補正あり、となります。 
また、南西諸島については、NH-52-8-4 EAST以南の九州の島嶼部のうち、NG-52-4-9/13、NG-52-6-13以外が補正無しにあたります。  
補正無しの地図については、他の地図と別にして下さい。

# プログラムの概要
 Preprocessing_of_Gaihozu_japan.ipynbとLat_lon_interpolation_for_Stanford_Gaihozu.ipynbは、jupyter notebookで作成しております。また、それ以降のプログラムはGoogle Colabでの使用を前提にしています。
## Preprocessing_of_Gaihozu_japan.ipynb
　このプログラムの使用には、同リポジトリに入っている8つのjpgファイル（temp～.jpg）が必要です。jpgファイルが入っているフォルダ（デフォルトですとannaconda実行フォルダ直下のdataフォルダ）に移動してから、それ以下のプログラムを実行して下さい。  
また、プログラムを実行する前に、Tesseract-OCRをインストールする必要があります。インストールする際には、日本語を追加しておいて下さい。 
セル11の『OCRによる判読プログラム』の部分で使用します。 
　また、このプログラムでは、3つのフォルダを用意する必要があります。セル11『OCRによる判読プログラム』の、文字判読用画像を入れるフォルダ『Moji』と一番最後のセル19にある、imageファイルと四隅情報を正答したcsvファイルを入れるフォルダである『result1』と、四隅情報がどこかで間違えているcsvファイルを入れる『result2』を、先に作成する必要があります。   
 　もう一つ、地図画像の分類の項で述べた10.4″の補正がある場合と無い場合で、セル16の最後の行が異なります。デフォルトが補正ありの場合で、補正が無い場合は、その下の行を使用して下さい。ただしこの時点ではまだ、10.4″補正があるフォルダと無いフォルダは分けておいて下さい（補正ありと補正無しで分けて、それぞれに3つのフォルダを作って下さい）  
  作成されるのは、result1フォルダ内に、緯度経度枠の外側それぞれ100画素で切り取った画像（例：res_NI_52_2_4.jpg）と、緯度経度の判定が正しければ対応するcsvファイル（例：coodNI_52_2_4.csv）、Mojiフォルダ内にjpgファイル（例：figNI_52_2_4.jpg）です。緯度経度判定に問題がある場合は、csvファイルだけがresult2フォルダに入ります。 

## 次のプログラムに行く前に
　フォルダMoji内の画像を目視でチェックします。 
 文字の一部が範囲から飛び出している、もしくは全く見えない場合、その四隅の検出は失敗しています。一か所でもそういった画像があれば、対応する画像（res_****.jpg）とcsvファイル（cood****.csv）は別フォルダに入れてください。 
 四隅判読失敗の画像については、  
 １．単純な検出失敗（四隅は切り取った後の画像に入っている）  
 ２．単純な検出失敗（四隅のうちの最低一つが切り取った後の画像に含まれていない）  
 ３。地図の一部が矩形からはみ出している  
の3種類が考えられます。   
１については、お絵描きソフトで正しい緯度経度点の位置を判読し、 csvファイルを修正する必要があります。   
２については、切り取った画像そのものを修正する必要があります。全体から手で切り取り直してcsvファイルの四隅の位置を修正するか、画像の一部をつぎはぎして、その点だけをcsvファイル上で修正するかになります。　  
３については、諦めて、手動で緯度経度を付与します。このように一部がはみ出した場合は、画像の切り出しの範囲も手動で決める必要があります。この作業は以下、また別のプログラムで行いますが、地図の線通りに切ると。例えば半島の一部の画像が消えます。諦めて、QGIS等で緯度経度処理をします。   
ここまで処理したcsvファイルはresult2のフォルダに、jpgファイルはresult1のファイルに入れます。  
（日本全体で数枚だけでしたが、四隅の緯度経度は検出できているのに、地図の一部が切り取られてしまうことがありました。地図の囲み線の途中で陸地の一部が外部にはみ出しているもので、これは島嶼部に生じていました）  
現時点では、100枚処理する際に、25枚程度は1，2，3のどれかに相当しており、手作業の処理が必要となっています。

## Lat_lon_interpolation_for_Stanford_Gaihozu.ipynb
　これは、半自動プログラムです。   
 result1のフォルダから、どれか一つのcsvファイルを選びます。この名前はcood**_**_**_**.csvと、3本のアンダーバーで繋がれています。この、最後の部分だけを*で示したのが、セル2のファイル名です。以下、ファイル名はこれを入れる必要が出ます（ので、半自動です）。   
 セル5は、『判読成功』の場合も間違いがないかどうかの確認です。判読成功、と割り振られる中に、緯度経度の度の部分が間違えているものがあります。これが無いかどうかをチェックするのがこのセルです。間違えているものがあれば、result2のフォルダに移します。   
 セル8では、間違いのなかったcsvファイルから一つを選びます。そこで選ぶのは、説明にもある通り、『緯度経度値判読失敗csvファイルが入るフォルダ』と、『それを修正したファイルを入れるフォルダ』です。後者を新しく作って下さい。  

## 次のプログラム群に行く前に
　ここまでで作成されるのは、１。地図部分全体より一回り外側で切り取られたjpgファイル　２。jpgファイル内の緯度経度点の位置座標と、対応する緯度経度値が書かれたcsvファイルです。  
　この二つを対にして、以下のプログラムで処理していきます。  
 以下のプログラムは、Google Colabで動かしますので、これらのファイルをGoogle Driveに入れる必要があります。その際、あまり一気に入れようとしないで下さい。  
 R1to4_Latlon_forGaihozuプログラムを動かすと、最大で16～17GBのファイルが出来てしまいます。個人用のGoogleドライブは15GBなので、注意しながら動かす必要があります。  
 ただ、作成されたファイルは、いわば作業途中のファイルなので、次の作業に必要な段階のものだけ残して消すことができます。名前の末尾で区別できますので、調整しつつ処理して下さい。  
 多分、GoogleドライブをPCにマウントするか同期するかして、データの移動を行うほうが良いとは思います。  
 

 

