# kbc-ytdl

### 大宮北高校放送部専用 youtube downloader

#### 概要

本ツールは、大宮北高校放送部のために開発された youtube 楽曲ダウンローダーです。

#### 使い方

[指定のスプレッドシート](https://docs.google.com/spreadsheets/d/1k-l4X4NAUZPb5QQbytocC0qGIiY6cpOKIKoG3d707lQ/edit?gid=0#gid=0)上の適切な場所に URL を配置し、本ツールを実行します。

1. 上記スプレッドシートを開く
2. 「ダウンロード」シートを開く
3. 「URL」と書かれた列の 2 行目以降に、ダウンロードしたい動画の URL を貼り付ける
4. 本ツールを google colab 上で実行する

    - [Google Colab](https://colab.research.google.com/) を開く
    - 1 つ目のコードセルを実行する。
        - `!pip install -q gspread oauth2client youtube-dl` を実行して、必要なライブラリをインストールする。
        - `drive.mount('/content/drive')` を実行して、Google Drive をマウントする。
        - `credentials = service_account.Credentials.from_service_account_file('/content/drive/MyDrive/KBC大宮北高校放送部/14.youtube-downloader/youtube-downloader-420717-fdc577ac7f2c.json')` を実行して、Google Drive の認証情報を取得する。
        - `service = googleapiclient.discovery.build('sheets', 'v4', credentials=credentials)` を実行して、Google Sheets API のサービスを作成する。
        - `spreadsheet_id = '1k-l4X4NAUZPb5QQbytocC0qGIiY6cpOKIKoG3d707lQ'` を実行して、スプレッドシートの ID を指定する。
    - 2 つ目のコードセルを実行する。
        - `range_ = 'ダウンロード!A2:A'` を実行して、スプレッドシートの範囲を指定する。
        - `result = service.spreadsheets().values().get(spreadsheetId=spreadsheet_id, range=range_).execute()`および`values = result.get('values', [])` を実行して、スプレッドシートの値を取得する。
        - `failed_downloads = []` を実行して、ダウンロードに失敗した動画のリストを初期化する。
        - `for url in .....`を実行して、取得された URL 一つ一つに対してダウンロードを試行する。

5. Google Drive の`KBC大宮北高校放送部 > 14.youtube-downloader > 01.downloaded_wav`フォルダに、ダウンロードした動画が保存される

#### 注意事項

-   本ツールは、Google Colab 上で実行することを前提としています。
-   スプレッドシートの「URL」列には、ダウンロードしたい動画の URL のみを貼り付けてください。
-   本ツールでは、2 行目に以降に貼られた URL のみを対象にダウンロードを行います。
-   放送部関係者以外からのアクセスを禁止するため、[指定のスプレッドシート](https://docs.google.com/spreadsheets/d/1k-l4X4NAUZPb5QQbytocC0qGIiY6cpOKIKoG3d707lQ/edit?gid=0#gid=0)に対する権限の操作はむやみに行わないでください。
-   本ツールは、Google Cloud Platform の API を使用して、Google Drive にアクセスします。API の使用に関する詳細は、[Google Cloud Platform のドキュメント](https://cloud.google.com/docs)を参照してください。
-   本ツールでダウンロードした楽曲は、放送部の活動にのみ使用してください。商用利用や不正利用は禁止されています。
-   本ツールでは、`.wav`形式で楽曲をダウンロードします。必要に応じて、他の形式に変換してください。
