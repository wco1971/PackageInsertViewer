# GS1CodeSymbolReader
S1コードシンボルを読み取り、医療機器医薬品の電子化された添付文書および関連情報のそれぞれのページにアクセスし閲覧検索機能を持つiPhone, IiPad用アプリ（電子化された添付文書および関連情報ページはそれぞれPMDAが用意するサイトにアクセスして閲覧する）
 
## 概要
Swiftuiを用いた、外部ライブラリを使用しないiOS, iPadOS用の医薬品医療機器のGS1コードシンボルリーダとPMDAの電子化された添付文書サイトおよび関連情報サイトへの接続、表示、検索を行うアプリのコードです。AIは現在設定されている（2024年12月１日現在）番号に対応していますが、解読に必要な対応表がGS1以外の規格で設定されている場合などは、一部対応しておらず数字（本来なら国名が表示されるはずです）がそのまま表示されます。
 
## 開発環境
Xcodeで使用するプロジェクトとしています。PCはSonoma14.3、xcodeは15.3で確認しています。アプリの対応OSはiOSと iPadOSそれぞれ 17が確認済みです。コードにiOS, iPadOS16用のコードが埋め込まれていますが、未確認の部分や17では実装済みの機能が省かれています。特にAIについては、swiftでのGS1コードの読み取り機能が16と17+では異なっており、16では機能が制限されています。
 
## 使い方
Xcodeを使ったUSBもしくはwifi経由（端末側の設定必要）、またtestflight経由でお手元のiPhone, iPad（iOSは17が確認済み。今のところ17＋なら機能すると思います）に転送して使用できます。
iPhone, iPadでアプリを起動後、カメラで医療機器、医薬品のGS1コード（データバー限定型、GS1 128, GS1 データマトリックスに対応。データバー二層型、データバー限定型合成シンボル、データバー二層型合成シンボルには対応していません）にピントがあうようにiPhone, iPadを動かすとGS1コードシンボルを読み取り、対応する電子化された添付文書を表示します。
添付文書を表示している画面でメニューを選択し関連情報のページを閲覧、検索することができます。iPadの場合、16+が使えるハードウェアであればmulti taskingにより本アプリを２個立ち上げることができます（本来マルチタスキング時にカメラが使用可能となるのはiOS17+と対応したハードウェアが必要なので、多分バグです）。
OSでマルチタスキングが可能な場合に本アプリを複数起動しハードウェアの向きを変えると、ライブ映像の向きがUIのそれと合わなくなります。同一アプリでのマルチタスキングを行う場合の処理について理解が不十分なためです。
大抵の場合後から立ち上げた画面のカメラが生きていますので、multi taskingをやめる場合、カメラの動いていない（静止画を表示している）taskを終了してください。
ライブ映像で読み取りをおこなっているので、デバックしてもらうとわかりますが同じシンボルに対して複数回同じ処理を行なっています（メモリリークは確認した範囲では生じません）。

GS1 Data Matrixの読み取りテストでは符号化のうち、ASCIIとC40は確認しましたがTEXT、X12、EDIFACT、BASE256は使えるデータがなく未確認です。
GS1 Data MatrixをSwiftで読み取ったデータのバイナリ値に埋め草（ASCII符号化の129）以降にもコード値が存在する場合がありますが、埋め草の意味を考えて埋め草以降を削除しています。
データコード値が255を超えた場合、データコードと誤り訂正コードの配置について理解が不十分でありかつswiftの読み取り情報のバイナリに含まれる内容の理解も不十分なため、処理はデータコード値が255個までを前提にしています。

 macOSについてはシングル、マルチタスキング共に未確認ですが、swiftの設計思想上、少しの変更でつかえるようになると思っています。お試しください。
