# Microsoft 365 Copilot：ファイル／ドキュメント読み込みの制約に関する調査レポート（2026年9月15日時点）

## TL;DR
- **最も確実に管理できるのは「語数ガイダンス」**：Copilotの公式目安は、参照/要約は約80,000語以下、Q&A（質問応答）は約7,500語以下、Rewrite（書き換え）は約3,000語以下（Microsoft公式明記）。これを超えると**エラーではなく無言で「先頭優先・中盤無視」**の挙動になる。
- **ファイルサイズ上限は「どのCopilotか」で全く異なる**：コンシューマー版（copilot.microsoft.com）は1ファイル50MB・1会話20ファイル（公式明記）。M365 Copilot Chat（業務用）は1プロンプト512MBとされるが**公式ドキュメントには数値記載なし**（Microsoft社員のウェビナー発言のみ）。SharePoint参照はライセンス無しで7MB、ライセンス有りで200MBが上限（Copilot Studio公式）。
- **コンテキストウィンドウ（トークン数）はMicrosoftが公式には数値を公開していない**。128kトークンという数字は2024年の報道・第三者情報ベース。Microsoftは業務用Copilot Chatを固定数値ではなく「Standard/Priorityアクセス（容量ベース）」モデルへ移行させており、数値は今後も変動する。

## 確実に押さえるべき数値一覧

| 項目 | 数値 | 適用条件 | 確度ラベル | 出典URL |
|---|---|---|---|---|
| 参照/要約の推奨上限 | 約80,000語 | Word/PowerPoint、要約・参照作成 | 公式ドキュメントに明記 | support.microsoft.com/en-us/topic/keep-it-short-and-sweet-...66de2ffd |
| Q&Aの推奨上限 | 約7,500語 | ドキュメントへの質問応答 | 公式ドキュメントに明記 | 同上 |
| Rewriteの推奨上限 | 約3,000語 | 書き換え | 公式ドキュメントに明記 | 同上 |
| Word 要約の最大サイズ（Detailed） | 約1,500,000語／約300ページ（Microsoft広報は「3,000ページ＝従来の10倍」と表現） | Word、Detailed要約、M365 Copilotライセンス必須 | 公式ブログに明記 | techcommunity「More ways to summarize long Word documents」/4398979 |
| Word 要約（2024年の拡張値） | 約80,000語 | 2024年時点の「Summarize this doc」拡張値（従来比4倍） | 公式ブログに明記 | techcommunity「Summarize longer Word documents」/4227451 |
| PowerPoint 要約上限 | 約40,000語 | PowerPoint「Summarize this presentation」 | 公式ドキュメントに明記 | support.microsoft.com/powerpoint/copilot/summarize-your-presentation |
| Excel 分析対象 | 最大約200万セル | Copilot in Excel分析（テーブル） | 公式（Copilot FAQ応答） | Copilot公式FAQ「works best with Excel tables up to two million cells」 |
| コンシューマーCopilot ファイルサイズ | 1ファイル50MB | copilot.microsoft.com | 公式ドキュメントに明記 | support.microsoft.com/microsoft-copilot/file-upload-in-microsoft-copilot |
| コンシューマーCopilot ファイル数 | 1会話20ファイル | copilot.microsoft.com | 公式ドキュメントに明記 | 同上 |
| M365 Copilot Chat（業務）ファイルサイズ | 1プロンプト512MB | 業務用work chat（ライセンス有無で同一） | Microsoft社員の回答（非公式・ウェビナー） | learn.microsoft.com/answers/a/2015446 |
| M365 Copilot Chat（業務）1日あたり | 約3ファイル/24時間（未ライセンス） | 未ライセンス（無償Copilot Chat）で報告 | 第三者・ユーザー報告（公式記載なし） | learn.microsoft.com/answers/questions/2280932 |
| SharePoint参照ファイル上限（ライセンス無） | 7MB | Copilot Studio、同一テナントにM365 Copilotライセンス無し | 公式ドキュメントに明記 | learn.microsoft.com/microsoft-copilot-studio/requirements-quotas |
| SharePoint参照ファイル上限（ライセンス有） | 200MB | 同一テナントにM365 Copilotライセンス有り＋Enhanced search on | 公式ドキュメントに明記 | 同上 |
| Copilot Studio 自前アップロードファイル | 512MB／ファイル | ナレッジソースの直接アップロード | 公式ドキュメントに明記 | 同上 |
| セマンティックインデックス対象 | 512MB（PDF/PPTX/DOCX）／テナント5,000万アイテム | M365 Copilotのインデックス | 公式ドキュメントに明記 | learn.microsoft.com/microsoftsearch/semantic-index-for-copilot |
| Copilot Studio エージェント指示 | 8,000文字 | instructions（Webエディタ） | 公式明記／実挙動は約5,300字で失敗の報告あり | learn.microsoft.com/answers（generative answer nodes） |
| Copilot Studio SharePointサイト数 | 最大25 URL | generative orchestration使用時 | 公式ドキュメントに明記 | requirements-quotas |
| Copilot Notebooks グラウンディング | 最大300ファイル（Copilot Chatユーザーは50参照） | ノートブックのグラウンディング | 公式ドキュメントに明記 | support.microsoft.com Copilot Notebooks |
| Copilot Pages+Notebooks コンテナ | 最大25TB | ユーザー所有SharePoint Embeddedコンテナ | 公式ドキュメントに明記 | learn.microsoft.com/microsoft-365/loop/cpcn-storage |
| コンテキストウィンドウ | 128kトークン（≒約96,000語） | GPT-4 Turbo/GPT-4o世代 | 第三者・2024年報道（公式ドキュメント記載なし） | VentureBeat（Jordi Ribas発言）/ MVPブログ |
| プロンプト文字数上限 | 2,000〜8,000文字（ライセンス/入口で変動） | Copilot Chat Web、Edge Copilot等 | Microsoft社員回答（非公式）／第三者 | learn.microsoft.com/answers |
| Teams 会議 | 約2時間超で回答限定・遅延 | Teams会議のCopilot | 公式ドキュメントに明記 | support.microsoft.com Teams Copilot FAQ |
| Teams チャット参照範囲 | 直近30日 | チャット/チャネルのCopilot参照 | 公式ドキュメントに明記 | 同上 |
| Copilot Credits プリペイド | 25,000クレジット/月＝200ドル/パック | Copilot Studio/Copilot Chatエージェント | 公式ドキュメントに明記 | microsoft.com pricing/copilot-studio |

## 詳細

### 1. 文書の長さ・語数ガイダンス（最重要・公式）

Microsoft公式サポート記事「How reference and document lengths affect Copilot responses（旧題：Keep it short and sweet）」（最終更新2026年8月18日、ms.date 2026-07-08）が唯一の一次情報で、以下を明記している（**公式ドキュメントに明記**）。原文：*"keeping the total of all of your referenced content to around 80,000 words or less helps Copilot work effectively. Asking Copilot questions about the document works best if the document is less than about 7,500 words. Rewrite works best on a document that is less than about 3,000 words."*

- **参照/要約**：参照コンテンツ合計を**約80,000語以下**に。
- **Q&A（質問応答）**：ドキュメントが**約7,500語未満**だと最も良く動作。
- **Rewrite（書き換え）**：**約3,000語未満**が最適。

この記事には英国版など一部ローカライズで「20ページ以下／15,000語以下」という別の数値が併記されているバージョンがあり、記述に揺れがある。**同一URLでも言語版・更新時期で数値が異なる**点に注意（矛盾の併記）。米国版現行本文は80,000/7,500/3,000語の3段階で記述。

**挙動（無言の切り捨て）**：公式に明記の通り、「タスクによってはCopilotがドキュメントの先頭のみに注目し、それ以降を無視する」。またLLMは「ファイルの先頭と末尾を優先し、長いファイルの中盤への注意が薄くなる」。**エラーではなく無言で中盤が無視される**のが基本挙動。要約（全文脈が必要）は語数制限の影響を強く受けるが、特定トピックの質問応答は長文でも比較的影響を受けにくい、とも明記。

**日本語での読み替え**：公式は「言語によって制限は多少変わる。使用言語に応じてページ/語数を調整せよ」とのみ記載し、日本語の具体的換算は**公式情報なし**。トークン換算の一般則として英語は1トークン≒0.75語だが、日本語は1文字が複数トークンに分割されやすく、同じ情報量でもトークン消費が英語比で概ね1.5〜2倍以上になる（第三者・推測）。したがって英語基準の80,000語ガイダンスは、日本語では**安全側により短く**（体感で半分程度）見積もるのが実務的に妥当。

### 2. コンテキストウィンドウ（トークン数）

**Microsoftは業務用M365 Copilotのトークン数を公式ドキュメントで公開していない**（サブエージェント確認）。判明している情報：

- 2024年、MicrosoftのCVP Jordi Ribas氏がGPT-4 Turbo採用を発表した際、報道（VentureBeat）で「128,000トークン（従来のGPT-4は32,768）」と説明された（**第三者・2024年報道**）。原文：GPT-4 Turboは *"a massively larger 'context window'... 128,000 tokens compared to 32,768 of older GPT-4 models."*
- 2025〜2026年のGPT-4o/GPT-5移行後も、第三者分析やMVPブログでは128kトークン（≒約96,000語、ただしシステムプロンプトが一部を消費）とされる（**第三者・推測**）。
- 変遷：一部技術記事は「初期は8k/GPT-4 Turbo以前」→「128k（GPT-4 Turbo以降）」という拡大を語るが、いずれも公式数値ではない。

**重要な構造**：M365 CopilotはRAG（検索拡張生成）システムであり、SharePoint上の全データがコンテキストに載るわけではない。セマンティックインデックスでMicrosoft Graphを検索→関連スニペットのみをコンテキストウィンドウに注入する。システム指示・会話履歴・ユーザープロンプト・取得データ・応答がすべて同じトークン予算を奪い合う。「テナントの全データ＝コンテキストウィンドウ」ではない点が実務上の最重要ポイント。

### 3. ファイル添付・アップロードの制限

#### コンシューマー版 Microsoft Copilot（copilot.microsoft.com）— 公式明記
- **1ファイル50MB、1会話20ファイル**（公式サポート「File upload in Microsoft Copilot」、ms.date 2025-07-07、更新2026-08-31）。
- 対応形式：PDF, DOCX, XLSX, PPTX（文書）／PNG, JPEG, PJP, JFIF（画像）／TXT, TEXT, JSON, CSV, MD（テキスト）。
- アップロードファイルは最大18ヶ月保管、モデル学習には不使用。

#### M365 Copilot Chat（業務用、m365.cloud.microsoft）— 公式数値なし
サブエージェント確認の通り、**Microsoftは業務用work chatの具体的なファイルサイズ・ファイル数を公式ドキュメントで公開していない**。公式は「Standard access and file upload limits apply」とのみ記述。

- **1プロンプト512MB**：Microsoft社員がウェビナーで述べたとされる値（Q&A回答、2025-03-24投稿、**非公式**）。「1MB制限は撤廃、ライセンス有無に関わらず512MB」とされる。
- **ライセンス有無の差**：ファイルサイズは同一（512MB）で、差は**Priority/Standardアクセス（サービス容量の優先度）**にある。未ライセンスのCopilot Chatは容量逼迫時にスロットリング/キューイングされる（公式「Standard versus priority access to features in Microsoft 365 Copilot Chat」）。
- **1日あたり**：未ライセンスのCopilot Chat（Web）で「24時間に3ファイルまで」というエラー報告が2025年6月頃から多数（E3ユーザー等）。以前は「1日5ファイル」との報告も。いずれも**ユーザー報告ベースで公式記載なし**。Microsoft自身「公式に文書化していない」と回答。
- 対応ファイル形式（業務用）は公式「File formats supported by Microsoft 365 Copilot」がCopilot Chat, Pages, Notebooks, Createで共通の形式を列挙。ローカルパスは不可、クラウド上/アップロード済みファイルのみ。非対応形式は.docx/.pdf等への変換を推奨。ZIP等の圧縮ファイルはチャット取り込み時に展開されずエラーになる（第三者）。

### 4. SharePoint / OneDrive参照時の制限（公式・重要）

Copilot Studio公式「Quotas and limits」およびナレッジソース関連ドキュメントに明記（**公式ドキュメントに明記**）：

- **同一テナントにM365 Copilotライセンスが無い場合**：メモリ制約により、generative answersはSharePointファイル**7MB以下**のみ処理可能。Enhanced search resultsはオフにする必要。
- **同一テナントにM365 Copilotライセンスが有る場合**：**最大200MB**。Enhanced search results＋tenant graph groundingをオンにする必要。
- **それより大きいファイル**：SharePointに保存されGraph検索では返るが、generative answersでは処理されない。代替として自前アップロードなら**512MB**まで可能。
- SharePointナレッジのコネクタペイロード上限：パブリッククラウド5MB、GCC 450KB。
- **セマンティックインデックス**：PDF/PPTX/DOCXは**512MBまで**インデックス対象（公式「Semantic indexing for Microsoft 365 Copilot」）。テナントあたり最大5,000万アイテムを追加費用なしでインデックス。Graphコネクタ経由の外部ソースは1アイテムあたり解析テキスト4MB（約60〜70万語、第三者）。
- **SharePoint埋め込みファイルのインデックス**：エージェントの埋め込みファイルは先頭750〜1,000ページ（約180万文字）までインデックス（公式「optimize sharepoint content」）。
- **SharePoint参照（URL指定）実務目安**：36,000文字（約15〜20ページ）以内が信頼性高い（Microsoft Q&AでのMicrosoft社員回答・非公式）。
- **業務でのユーザー報告**：SharePoint上ファイルでCopilotが「150MBを超えるファイルは応答制限を超える（select a file smaller than 150 MB）」というエラーを返す事例（ユーザー報告）。SharePointリストのクエリは先頭2,048行以降を落とす（第三者）。

### 5. アプリ別の制約

**Word**：
- 要約（Detailed）は**約1,500,000語／約300ページ**まで。公式ブログ原文：*"Our new Detailed summary also supports summarization of up to 3,000 pages in a document, an upgrade from the previous cap of around 300 pages (a 10X increase!)."*（Microsoft広報は「3,000ページ」と表現するが、厳密には150万語）。**M365 Copilotエンタープライズライセンスが必要**で、Businessライセンスやコンシューマー版は対象外（公式ブログ／第三者）。Windows版はVersion 2503（Build 18623.20042）以降で展開。
- 要約可能な最小は20語以上。自動要約はOneDrive/SharePoint保存が前提。
- 「アクティブなドキュメントを1回で送る」際の別のペイロード上限があり、超過時に「The document content has been truncated to meet size constraints（サイズ制約のため内容が切り詰められた）」と表示される（Microsoft Q&A、ユーザー報告）。TOC・画像・埋め込みメタデータを除去（txt経由で再保存）すると回避しやすい、との実績報告。

**Excel**：
- 分析対象は**最大約200万セル**（Copilot公式FAQ応答：*"Copilot works best with Excel tables up to two million cells."*。行数換算で約2,000,000行と紹介する第三者記事あり）。列はExcel上限16,384だが分析では256列程度までとの第三者情報。
- ファイル要件：.xlsx/.xlsb/.xlsm、OneDrive/SharePoint保存＋AutoSaveオン。テーブル形式（明確なヘッダー）が必要。非表示行/列は分析対象から漏れるため事前に再表示する。
- **重要な仕様変更**：従来の「App Skills」（高度な分析＝Python含む）は**2026年2月下旬までにExcelから削除**予定（公式サポート）。代替はAgent Mode／Copilot Chat／Analyst。
- Python高度分析はデータフレーム化して読み込む。整った行列構造が前提。

**PowerPoint**：
- 要約は**約40,000語まで**（公式サポート原文：*"Copilot in PowerPoint can currently summarize presentations up to approximately 40,000 words."*）。
- 新規作成プロンプトは2,000文字（約400語）まで（第三者）。
- スライド数を指定しても守られない（5枚指定で23枚生成）等の挙動報告（Microsoft Q&A、ユーザー報告）。特定スライド指定の要約が正しく機能しない報告も。

**Outlook**：
- スレッド要約（Summary by Copilot）が中核機能。公式には**スレッド長・件数の具体的上限は記載なし**。
- 未ライセンス（Copilot Chat）は「開いている単一メール/スレッド」に限定され、メールボックス横断検索は不可（Microsoft Q&A）。M365 Copilotライセンス有りだと横断的な要約・検索が可能。

**Teams**：
- 公式FAQ原文：*"Answers may be limited or be subject to longer latency in meetings over approximately 2 hours long."*（**約2時間**を超える会議で回答が限定的/遅延、公式明記）。
- チャット/チャネルの参照範囲は**直近30日**（公式FAQ：*"it will only reference messages sent in the last 30 days from the most recent message sent."*）。組織の保持ポリシーでさらに制限され得る。
- 会議トランスクリプトが利用不可になるとCopilotも利用不可になり過去の会話履歴も消える（公式）。Intelligent recapはTeams Premiumが必要。
- （参考・別製品）Copilot for Salesの会議録要約は録画70分（北米・欧州は100分）を超えるとハイライト/フォローアップが生成されない（公式）。

### 6. その他の周辺制限

**プロンプト文字数**：
- ライセンス/入口で変動。Business Basicは2,000文字、Office 365 E3は4,000文字という報告（Microsoft Q&A）。Edge Copilotは2,000/4,000→4,000/8,000へ拡大（Microsoft社員回答・非公式）。E5でCopilot Chat Webが8,000文字制限との報告も。**公式に統一的な数値なし**。

**Copilot Notebooks**：
- グラウンディングは**最大300ファイル**（Copilot Chatユーザーは50参照）。共有場所（SharePointフォルダ/サイト）追加時はアクセス可能ファイルから最も関連する300を選択（公式サポート）。
- 別のQ&Aでは「先頭100参照のみグラウンディングに使用」との記述もあり、数値に揺れ（50 vs 100 vs 300）。ライセンス種別・時期による差と思われる（両論併記）。
- 画像生成・データ可視化は不可。

**Copilot Pages／Notebooks 容量**：
- Pages+Notebooksの格納コンテナは**最大25TB**（変更不可、公式）。ユーザー所有のSharePoint Embeddedコンテナに格納。

**Copilot Studio**（M365 Copilotのエージェント作成との違い）：
- instructions（指示）は**8,000文字**（Webエディタの制限）。ただし実挙動ではmain agent＋Topicノードの合計が5,300文字前後で「prompt instructions exceeds the threshold」エラーの報告あり（Microsoft Q&A）。VSCode拡張経由なら8,000超もプッシュ可能だがWebエディタで編集不可になる。
- ナレッジソース：自前アップロードは**512MB/ファイル**、SharePointは前述の7MB/200MB。ファイル数は最大500〜1,000（情報源で差）。
- エージェントへのメッセージ：8,000 RPM/Dataverse環境（有償プラン）。generative answersは最大15スニペット使用。

**利用回数・課金（Copilot Credits）**：
- Copilot Studio/Copilot Chatエージェントは2025年9月1日よりCopilot Creditsで従量課金（旧「メッセージ」単位を置換）。
- プリペイド容量パック：**25,000クレジット/月＝200ドル/パック**。従量課金は1クレジット0.01ドル。
- M365 Copilotライセンスユーザーの社内向けエージェント利用は概ねゼロレート（無償）だが、外部/顧客向けエージェントは課金対象。管理センターの利用レポートは1ユーザーが2,000クレジット超過でアラート表示。
- コンシューマー無償Copilotのテキストチャットは固定日次上限を公式非公開（2024年に旧「1日300チャット」上限は撤廃）。画像生成は1日15ブースト。
- コンシューマー有償（旧Copilot Pro、2025年10月1日にMicrosoft 365 Premiumへ置換）はAIクレジット/機能上限で管理。

### 7. 制限回避のワークアラウンド（公式＋実績）

- **分割（Break It Down）**：長文を小分けにして個別にCopilotへ渡す（公式推奨）。
- **部分要約（Summarize in Parts）**：章ごとにコピペして別ドキュメントで要約、後で統合（公式推奨）。
- **セクション指定の質問**：「全文を読め」ではなく特定セクション/トピック/章を指定して質問（Microsoft Q&A推奨）。
- **Wordのペイロード切り詰め回避**：txt経由でTOC・画像・埋め込みメタデータを除去して再保存（ユーザー実績）。
- **M365 Copilotアプリで開く**：Wordのチャットペインの「…」→「Open in M365 Copilot App」で全文読み込みが通った実績（ユーザー報告、OneDrive保存が条件の可能性）。
- **クラウド保存＋リンク参照**：大きいファイルはOneDrive/SharePointに置きリンクで参照（Graph経由でトークン消費を抑制）。
- **ナレッジソース化**：反復利用するならCopilot Notebook（最大300参照）やCopilot Studioエージェント（512MB自前アップロード）に事前登録。
- **Excelは事前集約**：200万セル超はPower Queryで行削減・列を絞ってからCopilotへ。

## 推奨アクション（実務ガイド）

1. **まず語数で管理せよ（最優先）**：確実に全文を読ませたいなら、要約は約80,000語以内、質問応答は約7,500語以内、書き換えは約3,000語以内を守る。日本語はトークン消費が多いため、これらの数値を**半分程度に割り引いて**運用するのが安全。
2. **重要情報は先頭と末尾に置く**：LLMは中盤を軽視する（公式明記）。要点・結論・重要数値を冒頭かつ末尾に配置。
3. **要約結果は必ず検証**：特に長文では中盤の重要点（名前・日付・金額・義務・結論）が脱落しうるため、原文と突き合わせる。
4. **ファイルサイズの閾値を意識**：コンシューマー版は50MB/20ファイル（確実）。業務用は512MBとされるが公式保証なし、未ライセンスは1日3ファイルで詰まる可能性。反復業務はNotebook/Studioへ。
5. **SharePoint参照は7MB/200MBの壁を設計に織り込む**：ライセンス無しテナントは7MB超が処理されない。大きいファイルは分割保存かライセンス取得を検討。
6. **Excel App Skills廃止（2026年2月）に備える**：Python高度分析を使うワークフローはAgent Mode/Analystへ移行。
7. **挙動が変わったら仕様変更を疑う**：Copilotの制限は頻繁に変わる。「昨日通ったのに今日ダメ」はロールアウト/仕様変更の可能性が高い。

### 閾値が変わったら見直すべきサイン
- Wordで「The document content has been truncated to meet size constraints」表示 → 分割かtxt再保存へ。
- Copilot Chatで「daily upload limit reached」 → 未ライセンス枠の1日制限（推定3ファイル）。ライセンス取得かOneDrive参照へ。
- SharePointファイルで「beyond my response limit / select a file smaller than 150 MB」 → サイズ超過。分割へ。
- エージェントで「prompt instructions exceeds the threshold」 → 指示文を約5,000文字以下に圧縮。

## 注意事項（Caveats）
- **公式が数値を出していない項目が多い**：業務用Copilot Chatのファイルサイズ・ファイル数・日次上限、コンテキストウィンドウのトークン数は、いずれも**Microsoft公式ドキュメントに数値記載がない**。本レポートの512MB・3ファイル/日・128kトークンは非公式（社員発言/ユーザー報告/報道）である。Microsoftは業務用を固定数値ではなく容量ベースの「Standard/Priority」モデルへ移行させており、今後も数値は変動する。
- **同一URLでも数値が揺れる**：語数ガイダンス記事は言語版・更新時期で「80,000/7,500/3,000語」と「20ページ/15,000語」が混在。Notebookの参照上限も50/100/300と情報が割れる。ライセンス種別・時期差の可能性が高い。
- **語数/ページ換算は目安**：Microsoftの「300ページ=150万語」「80,000語≒123ページ（1ページ650語換算）」等の換算は1ページあたり語数の前提で変動する。
- **仕様は頻繁に変わる**：本レポートは2026年9月15日時点。特にExcel App Skills廃止（2026年2月）、Copilot Credits移行（2025年9月）、コンシューマー版のMicrosoft 365 Premiumへの再編（2025年10月）など、直近1年で大きな変更が続いている。導入前に必ず最新の公式ドキュメントで再確認すること。