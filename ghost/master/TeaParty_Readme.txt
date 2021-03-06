＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃
＃
＃　　「奏でる日常の音色」お茶会イベント
＃　　ゴースト作者さん向け対応マニュアル
＃
＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃＃


このファイルは、「奏でる日常の音色」のお茶会イベントの仕様について記述したファイルです。
初めてご覧になった方は、更新履歴を飛ばして「目次」からご覧ください。

このファイルは仕様書的な意味合いが強いので、
苦手な方は、「里々」での対応例サンプルの「TeaParty_Sample.txt」をご覧ください。

-------------------------------------------------------------------

最新情報:2012.11.11
新しいこのお茶会の仕様に完全に移行しました。



2012.10.30
誤字の修正をしました。
OnKanadeTeaPartyInfomationRequestイベントのサンプルコードで、
「お茶会対応バージョン」と「お茶会バージョン」の２つの単語がありましたが、誤字です。
両方同じ単語で「お茶会対応バージョン」の間違いです。申し訳ありません。

2012.10.16
このテキストを公開しました。
まだお茶会本体は「奏でる日常の音色」には搭載されていません。
現在は「dic_TeaParty.txt」には新仕様のテストが出来るしくみが、
「dic_TeaTime.txt」には旧仕様のお茶会が入っています。
土曜日に普通にお茶会を実行しますと、旧仕様のお茶会がはじまってしまいます。

また、暫定仕様として、新仕様のお茶会に対応していないゴースト向けに、
「OnKanadeTeaTime」イベントも新仕様のお茶会イベントと同時に全ゴーストに送信する予定ですので、
「OnKanadeTeaTime」イベントに反応しないようにしておいてください。
(移行用にOnKanadeTeaTimeのReference2、Reference3に「1」という数値を格納するので弾くような設定にするのがおすすめです。)

・移行時期について（旧仕様対応の作者さん向け）
１１月１１日〜１１月１６日のできるだけ早い日に新仕様を完全実装したバージョンを公開し、
１１月１７日以降にユーザさんが新仕様のお茶会が実行できるようになる予定です。

そのため１１月１１日〜１１月１６日、もしくは１１月１７日以降に新仕様に対応したゴーストを公開するのがおすすめです。
また、上記のように「OnKanadeTeaTime」と「OnKanadeTeaParty」の両方に対応して、
OnKanadeTeaTimeのReference3に「1」があれば弾くという仕組みを導入すれば、移行時期を気にすることなく移行ができます。


------------------------------------------------------------------------

目次
１．お茶会イベントについて
２．動作環境
３．お茶会の仕様（対応に必須となる処理）
４．デバッグツールの紹介
５．旧仕様（OnKanadeTeaTime）との違いと移行について
６．お茶会の内容一覧
７．その他に発生するイベント


１．お茶会イベントについて
このイベントは、「奏でる日常の音色」が主催する「お茶会」で
他のゴーストさんたちとお茶やお菓子を食べるという設定で、
トークしてもらうというイベントです。
あなたのゴーストさんも、いかがですか？

下に紹介する仕様を読んで「さっぱりわからん！」という方は遠慮無く相談ください。
可能な限り実装に向けてお手伝いいたします。
「里々」であればほぼ確実に大丈夫です。


２．動作環境
このお茶会機能は「SSP 2.2.91」で作成・動作確認しています。
これ以前のバージョンまたは別のベースウェアでは正常に動作しない可能性があります。
また、このゴーストではSHIORIに「里々」を使用していますが、SHIORIには依存しません。


３．お茶会の仕様（対応に必須となる処理）
お茶会では、大きく分けて２回の情報交換が必要になります。
やりとりの方法はこちらで指定していますが、
トーク内容はゴーストそれぞれ自由に記述することができます。

・情報交換ステップ
「奏でる日常の音色」が他の起動しているゴーストすべてに「奏でる日常の音色」の
お茶会のバージョン情報を含むイベントを送信します。
イベントを受け取ったお茶会に参加するゴースト（以下、クライアント）は、
自分の対応しているお茶会バージョンと比べ、どの程度対応しているかの内容を返信します。
「奏でる日常の音色」は受け取った対応情報を確認して、開催するお茶会の内容を決定します。
データのやり取りは、\![raiseother]タグを使います。


イベント仕様

イベント名:OnKanadeTeaPartyInfomationRequest
概要:「奏でる日常の音色」からクライアントに対してバージョン情報の送信をリクエストします。
　このイベントを受け取ったクライアントは可能な限り即座に返信イベントを送る必要があります。
　このイベントでトークをするのは推奨しません。

Reference0:「奏でる日常の音色」のお茶会バージョン番号
Reference1:返信イベントの送信先ゴースト（「かなで」に固定）
Reference2:返信イベント名

イベント名:可変（OnKanadeTeaPartyInfomationRequest の Reference2）
概要:OnKanadeTeaPartyInfomationRequestを受け取ったクライアントが、
　「奏でる日常の音色」に情報を送信するための返信イベントです。

Reference0:クライアントの対応するお茶会バージョン番号
Reference1:クライアントの\0(Sakura)名（descript.txtに設定したものを正確に）
Reference2:「奏でる日常の音色」のお茶会バージョンに対する対応情報

Reference0について…詳細は「６．お茶会の内容一覧」で説明しています。
　まずはそういう数値が必要だと考えておいてください。

Reference2について…次の４つのうち当てはまるものを格納してください。（【】は不要）
　返信イベントを送らなかった場合・これ以外の対応情報を返送した場合は、
　自動的に【固定対応】とみなされます。

【対応】
「奏でる日常の音色」のバージョンよりもクライアントのバージョンが高いまたは同じなため、
この「奏でる日常の音色」が提供するお茶会イベントに完全に対応しています。

【非対応】
「奏でる日常の音色」のバージョンよりもクライアントのバージョンが低いため、
クライアントのバージョンよりも高いバージョンのお茶会イベントは処理することができません。

【自動対応】
「奏でる日常の音色」のバージョンよりもクライアントのバージョンが低いですが、
クライアントのバージョンよりも高いバージョンのお茶会イベントについて、
単語を当てはめたりなどトークの内容を変化させて自動的に対応することができます。

【固定対応】
「奏でる日常の音色」が提供するお茶会の内容によって対応は変化しません。
返答イベントのReference0はクライアントの制作者が確認しているお茶会バージョンです。
この対応情報を返送したクライアントについては、お茶会の内容選択を考慮しません。



イベント解説…サンプルコードは里々

「奏でる日常の音色」によって、バージョン情報の送信をリクエストする
「OnKanadeTeaPartyInfomationRequest」イベントが呼び出されます。

このイベントを受け取ったクライアントは、自分のお茶会バージョン以下かどうか判定し、
返送するイベントの内容を決定します。

イベントの返送は\![raiseother]タグを使います。
\![raiseother]タグの内容は、\![raiseother,ゴースト名,イベント名,Reference0...]となり、
ゴースト名にはReference1が、イベント名にはReference2が当てはまります。
続けて送信するイベントのReference0にはクライアントの対応するバージョン、
Reference1にはゴースト名、Reference2には対応情報がはいります。


＊OnKanadeTeaPartyInfomationRequest
＞お茶会バージョンに対応している	（Ｒ０）<=（お茶会バージョン）
＞お茶会バージョンに対応していない

＊お茶会バージョンに対応している
：\![raiseother,（Ｒ１）,（Ｒ２）,（お茶会バージョン）,ゴースト名,対応]

＊お茶会バージョンに対応していない
：\![raiseother,（Ｒ１）,（Ｒ２）,（お茶会バージョン）,ゴースト名,非対応]

＠お茶会バージョン
１


・お茶会ステップ
お茶会がはじまると、出されたお菓子や飲み物の内容を含むイベントが「奏でる日常の音色」から
起動している他のゴーストにイベントが送信されます。
このイベントを受け取ったクライアントは、送信された内容について自由にトークすることができます。
このイベントを受け取った時点で「奏でる日常の音色」とのやり取りは終了します。
返送の必要はありません。

イベント仕様

イベント名:OnKanadeTeaParty
概要:お茶会のイベント本体です。クライアントはこのイベントの内容を元にトークします。
　このイベントは、OnKanadeTeaPartyInfomationRequestイベントの返送に対応情報に、
　【非対応】を返送したり返送自体しなかった場合でも、対応していないバージョンの
　お茶会イベントが送信されることがあるため、誤作動をしないようにしてください。

Reference0:Reference1以降の組み合わせが実装されたお茶会のバージョン
Reference1:お飲み物
Reference2:お菓子
Reference3以降:拡張情報

Referenceの組み合わせは、「お茶会の内容一覧」に記載してあります。
内容によってトークを分岐する使い方をする場合は、Reference0とReference2の両方でチェックすることをおすすめします。
Reference1はバージョン１では３種類ですが、お菓子の内容だけでなくこちらも新しい種類ができるかもしれません。

イベント解説…サンプルコードは里々

お茶会が始まると、お茶会の内容を通知する「OnKanadeTeaParty」イベントが呼び出されます。
このイベントについて、クライアントに要求される操作はありません。
「OnGhostChanged」「OnMinuteChange」のようにReferenceの内容を使ったりして、
クライアントが自由にトークすることができます。

サンプルコードでは、まずイベントでReference0が自分のバージョン以下であることを確認し、
次にその内容についてトークしています。

＊OnKanadeTeaParty【タブ】（Ｒ０）<	（お茶会バージョン）
：今日は（Ｒ１）と（Ｒ２）だね。

＠お茶会バージョン
１


４．デバッグツールの紹介

基本的には現在「お茶会イベント」は土曜日にしか実行することができませんが、
ゴースト作者さん向けにひと通りの内容をテストするデバッグツールを用意しています。
デバッグツールなため呼び出し方法は特殊です。

起動方法…「奏でる日常の音色」の「OnKanadeTeaPartyDebug」イベントを呼び出してください。
簡単な方法…右クリックメニューの「機能」＞「開発用パレット」＞「スクリプト入力」
　出たウインドウに「\![raise,OnKanadeTeaPartyDebug]」と入力して送信してください。

デバッグツールでは、お茶会の内容を呼び出して動作を確認したり、
バージョンチェックがきちんとできているかどうかの確認をすることができます。


５．旧仕様（OnKanadeTeaTime）との違いと移行について

「お茶会の内容を増やして欲しい」などのリクエストを受けたため、
将来的に内容を変更しても対応していないゴーストとの間で不具合が発生しにくい仕様に変更することとなりました。

基本的には「TeaTime」が「TeaParty」に置き換わります。
以下に対応が必須となる、あるいは名称が変更されるなどしたイベントを示します。

・OnKanadeTeaParty イベント
「OnKanadeTeaTime」のReference0,Reference1と、
「OnKanadeTeaParty」のReference1,Reference2が等価となります。
旧仕様のお茶会の内容はすべてバージョン１で、Reference0の内容が１になり、
拡張情報であるReference3以降はありません。

お茶会の内容に、お茶会イベント公開後に追加された「クリスマスケーキ」をバージョン１に追加しています。

・OnKanadeTeaPartyInfomationRequest イベント
旧仕様にはまったく存在しなかったイベントですが、新仕様では必須となります。
少々ややこしいところもありますが、実装をお願いします。

・OnKanadeTeaPartyDebug イベント
「OnKanadeTeaTimeDebug」と等価となる「奏でる日常の音色」に実装された
お茶会デバッグ用機能となります。
イベント内容を他のゴーストに送信したり、バージョンチェックのテストができます。


６．お茶会の内容一覧
ここからが「OnKanadeTeaParty」イベントで実際にで送信される内容となります。
まず最初にお茶会バージョン１で送信される情報からタイトル→詳細と書いています。

クライアントはお茶会バージョン１に示されるお茶会の内容すべてに対応した時、
そのクライアントはお茶会バージョン１であるという扱いになります。
（「奏でる日常の音色」に情報を送信する時のバージョン番号が「１」）
また仮にバージョン３のお茶会が存在し、それに対応する場合は、
バージョン１、２、３のすべてに対応している必要があります。

内容に更新があった場合は、このテキストを更新します。


＠バージョン１のお茶会　お茶とお菓子の組み合わせ
マカロン＋紅茶
レアチーズケーキ＋紅茶
イチゴショート＋紅茶
アップルパイ＋紅茶
クリスマスケーキ＋紅茶
さくらもち＋緑茶
わらびもち＋緑茶
フレンチトースト＋コーヒー
ドーナツ＋コーヒー
クッキー＋コーヒー


「マカロン＋紅茶」
Reference0:1
Reference1:紅茶
Reference2:マカロン

「レアチーズケーキ＋紅茶」
Reference0:1
Reference1:紅茶
Reference2:レアチーズケーキ

「イチゴショート＋紅茶」
Reference0:1
Reference1:紅茶
Reference2:イチゴショート

「アップルパイ＋紅茶」
Reference0:1
Reference1:紅茶
Reference2:アップルパイ

「クリスマスケーキ＋紅茶」
Reference0:1
Reference1:紅茶
Reference2:クリスマスケーキ
備考:お茶会公開後に追加された「クリスマスケーキ」が新仕様のバージョン１に追加されます。

「さくらもち＋緑茶」
Reference0:1
Reference1:さくらもち
Reference2:緑茶

「わらびもち＋緑茶」
Reference0:1
Reference1:わらびもち
Reference2:緑茶

「フレンチトースト＋コーヒー」
Reference0:1
Reference1:フレンチトースト
Reference2:コーヒー

「ドーナツ＋コーヒー」
Reference0:1
Reference1:ドーナツ
Reference2:コーヒー

「クッキー＋コーヒー」
Reference0:1
Reference1:クッキー
Reference2:コーヒー
備考:トーク内容を一部変更しています。


７．その他に発生するイベント

お茶会イベントで発生する、対応が必須ではないイベントを紹介します。
増えることもありますので、そのときはこのテキストを更新します。

イベント名:OnKanadeTeaPartyEnd
Reference0:お茶会を実行したバージョン

お茶会イベントで「奏でる日常の音色」がお茶会を終わるトークをした後に、すべてのゴーストに通知されます。
すべてのゴーストに通知されるため、Reference0のバージョンでお茶会に参加していたかをチェックできます。
