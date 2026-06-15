<!-- pre-align:aligned sig=e34a5abcf80f -->

## Notification > Email > エラーコード

<a id="api-result-code"></a>

### API結果コード

| カテゴリ | 成否 | 結果コード | 結果コードメッセージ | APIレスポンスメッセージ |
|-------|-------|-------|-------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| 共通   | true  | 0     | 成功                                                               | success                                                                                                         |
| 共通 | true | 1000 | 一部のリクエストが成功しました。 | Some of your requests are successful. |
| 共通 | false | -1000 | 無効なappKeyです。 | Invalid appKey. |
| 共通   | false | -1001 | サービスが存在しません。                                                   | Service is not exist.                                                                                            |
| 共通   | false | -1002 | サービスが無効化されています。                                                 | Service is disabled.                                                                                             |
| 共通 | false | -1003 | 無効なサービスパラメータです。 | Invalid service parameter. |
| 共通 | false | -1004 | 無効なsecretKeyです。 | Invalid secretKey. |
| 共通 | false | -1005 | サービスが一時的にブロックされました。 | Service is temporarily blocked. |
| 共通   | false | -1007 | サービスの'useYn'フィールドは必須です。                                           | The 'useYn' field of the service is required.                                                                    |
| 共通 | false | -2000 | 日付形式が無効です。 | Date format is invalid. {}: {}, Only supported {}. |
| 共通 | false | -2028 | 行数制限を超えました。最大50,000行まで処理できます。 | Limit of rows is exceeded. It can handle up to 50,000 rows. |
| 共通 | false | -2046 | 'Y' または 'N' の値のみ使用できます。 | The '{}' can have only 'Y' or 'N' values. {}: {}. |
| 共通 | false | -5000 | 無効なパラメータです。 | Invalid parameter. |
| 共通 | false | -5001 | すでに有効化されているappKeyです。 | Already activated appKey. |
| 共通 | false | -5002 | 有効化されていないappKeyです。 | Not activated appKey. |
| 共通   | false | -5003 | 存在しないappKeyです。                                                | Not exist appKey.                                                                                               |
| 共通   | false | -9993 | リクエストバインディングエラーです。                                                     | Binding the requests error.                                                                                     |
| 共通   | false | -9994 | クライアント例外です。                                                      | Client exception.                                                                                               |
| 共通 | false | -9995 | 無効なリクエストです。 | Invalid request. {} |
| 共通 | false | -9996 | 'application/json' タイプのコンテンツのみサポートします。 | It supports only the content of a type 'application/json'. |
| 共通 | false | -9997 | HTTPメッセージを読み取れません。 | Can not read http message. |
| 共通   | false | -9998 | 存在しないAPIです。                                                   | Not exist API.                                                                                                  |
| 共通   | false | -9999 | 内部エラーです。                                                         | Internal error.                                                                                                 |
| ドメイン | false | -1006 | 組織内で共有されたドメインに関連付けられたサービスです。共有を解除してから進めてください。 | Service is associated with a domain shared within the organization. Please proceed after canceling the sharing. |
| ドメイン  | false | -2700 | ドメインが空です。                                                     | Domain is empty.                                                                                                |
| ドメイン  | false | -2702 | すでに存在するドメインです。                                                   | Already existed domain.                                                                                         |
| ドメイン  | false | -2704 | ドメインがルートドメインではありません。                                                | Domain is not Root Domain.                                                                                      |
| ドメイン | false | -2706 | サブドメインをルートにすることはできません。 | Subdomain must not be root. |
| ドメイン | false | -2707 | 無効なサブドメインパラメータです。 | Invalid subdomain parameter. |
| ドメイン | false | -2711 | 無効なドメインパラメータです。 | Invalid domain parameter. |
| ドメイン  | false | -2713 | DKIM認証に失敗しました。                                                  | DKIM verification failed.                                                                                       |
| ドメイン | false | -2714 | ドメインの共有に失敗しました。 | Domain sharing failed. |
| ドメイン | false | -2715 | DKIMの無効化に失敗しました。 | DKIM deactivate failed. |
| ドメイン | false | -2716 | DKIMの有効化に失敗しました。 | DKIM activate failed. |
| ドメイン | false | -2717 | ルートドメインが認証されていません。 | Root Domain is not verified. |
| ドメイン | false | -2718 | すでに共有されたドメインです。 | Already shared domain. |
| ドメイン | false | -2719 | このドメインは他の組織のユーザーと共有されていません。 | This domain is not sharing another organization user. |
| ドメイン  | false | -2720 | DMARCレコードが空です。                                               | DMARC record is empty.                                                                                          |
| ドメイン | false | -2721 | SPFレコードが重複しています。ドメインのTXTレコードを確認してください。 | SPF record is duplicated. check your domain txt record. |
| ドメイン | false | -2722 | DNSルックアップが多すぎるため、SPFレコードが見つかりません。DNSルックアップの制限は10です。 | SPF record could not be found because there were too many DNS lookups. DNS lookup limit is 10. |
| ドメイン | false | -2723 | SPFレコードのallの位置が無効です。allは最後の位置にある必要があります。ドメインのTXTレコードを確認してください。 | Invalid all position SPF record. all position must be last position. check your domain txt record. |
| ドメイン | false | -2726 | DNSルックアップに失敗しました。 | DNS LookUp failed. lookup failed message: {} |
| 送信/照会 | false | -2001 | 'receiverList' または 'receiver' は必須です。 | The 'receiverList' or 'receiver' is required. |
| 送信/照会 | false | -2002 | 'senderAddress' は必須です。 | The 'senderAddress' is required. |
| 送信/照会 | false | -2003 | 無効な添付ファイル名です。 | Invalid attach file name. |
| 送信/照会 | false | -2004 | ファイルが見つかりません。                                                    | Not found the file. attachFileId: {}                                                                            |
| 送信/照会 | false | -2006 | データが存在しません。                                                   | Not exist data.                                                                                                 |
| 送信/照会 | false | -2007 | 'requestId' が無効です。 | The 'requestId' is invalid. |
| 送信/照会 | false | -2008 | 添付ファイルのアップロードエラーです。 | Upload attach file error. |
| 送信/照会 | false | -2009 | 'requestId' または 'startSendDate' は必須です。 | The 'requestId' or 'startSendDate' is required. |
| 送信/照会 | false | -2010 | 送信者のメール形式が正しくありません。 | Sender Email Format is wrong. |
| 送信/照会 | false | -2011 | 受信者のメール形式が正しくありません。 | Receiver Email Format is wrong. |
| 送信/照会 | false | -2012 | 最大受信者数は1,000人です。 | The maximum number of recipients is 1000. |
| 送信/照会 | false | -2013 | ファイルサイズは30M以下である必要があります。 | The file size is less than 30M. |
| 送信/照会 | false | -2014 | テンプレート添付ファイルを含め、最大10個まで許可されます。 | Up to 10 attachments including the template's are allowed. |
| 送信/照会 | false | -2015 | 'title' は必須です。 | The 'title' is required. |
| 送信/照会 | false | -2016 | 'body' は必須です。 | The 'body' is required. |
| 送信/照会 | false | -2017 | 'receiveMailAddr' は必須です。 | The 'receiveMailAddr' is required. |
| 送信/照会 | false | -2018 | 'receiveType' は必須です。 | The 'receiveType' is required. |
| 送信/照会 | false | -2019 | 'receiveType' が無効です。 | The 'receiveType' is invalid. |
| 送信/照会 | false | -2020 | 'fileId' が無効です。 | The 'fileId' is invalid. |
| 送信/照会 | false | -2021 | 全体のファイルサイズが大きすぎます。 | The size of total files is too large. |
| 送信/照会 | false | -2023 | 広告メールの場合、件名の前に (広告)、(AD) などの文言を配置する必要があります。 | For advertising mail, (광고), (AD), or (広告) phrases must be placed in front of the title. |
| 送信/照会 | false | -2024 | 無効なファイル拡張子です。 | Invalid file extension. |
| 送信/照会 | false | -2029 | 'summary' 属性は200文字を超えることはできません。 | The 'summary' property can not exceed 200 characters. |
| 送信/照会 | false | -2030 | 'customHeaders' に無効な名前または本文が含まれています。 | The 'customHeaders' contains an invalid name or body. |
| 送信/照会 | false | -2031 | リクエストされた時間より過去に送信することはできません。'requestDate' を更新してください。 | You can not send in past than requested. Please update the 'requestDate'. |
| 送信/照会 | false | -2032 | 全ての受信者への送信に失敗しました。 | All of receivers are failed to send. |
| 送信/照会 | false | -2034 | 'title' が最大長を超えました。(998文字) | The 'title' exceeds maximum length. (998 chars) |
| 送信/照会 | false | -2035 | 'userId' が最大長を超えました。(50文字) | The 'userId' exceeds maximum length. (50 chars) |
| 送信/照会 | false | -2036 | 'templateId' が最大長を超えました。(50文字) | The 'templateId' exceeds maximum length. (50 chars) |
| 送信/照会 | false | -2037 | 'senderName' が最大長を超えました。(100文字) | The 'senderName' exceeds maximum length. (100 chars) |
| 送信/照会 | false | -2038 | 'senderAddress' が最大長を超えました。(100文字) | The 'senderAddress' exceeds maximum length. (100 chars) |
| 送信/照会 | false | -2039 | 'receiveName' が最大長を超えました。(100文字) | The 'receiveName' exceeds maximum length. (100 chars) |
| 送信/照会 | false | -2040 | 'receiveAddress' が最大長を超えました。(100文字) | The 'receiveAddress' exceeds maximum length. (100 chars) |
| 送信/照会 | false | -2041 | 'fileName' が最大長を超えました。(100文字) | The 'fileName' exceeds maximum length. (100 chars) |
| 送信/照会 | false | -2042 | 'createUser' が最大長を超えました。(50文字) | The 'createUser' exceeds maximum length. (50 chars) |
| 送信/照会 | false | -2043 | 現在、メールが多すぎます。別の時間に送信してください。 | There are too much mail at this time. Please send it at another time. |
| 送信/照会 | false | -2044 | 'fileName' は必須です。 | The 'fileName' is required. |
| 送信/照会 | false | -2045 | 'fileBody' は必須です。 | The 'fileBody' is required. |
| 送信/照会 | false | -2047 | 'senderGroupingKey' が最大長を超えました。(100文字) | The 'senderGroupingKey' exceeds maximum length. (100 chars) |
| 送信/照会 | false | -2048 | 最大1000まで設定できます。                                             | You can set the size up to 1000. pageSize: {}                                                                   |
| 送信/照会 | false | -2049 | 'requestId' または ('startSendDate' と 'endSendDate') は必須です。 | The 'requestId' or ('startSendDate' and 'endSendDate') are required. |
| 送信/照会 | false | -2050 | {} は他のNHN Cloudプロジェクトによって保護されています。発信ドメイン認証後に使用してください。 | The {} was protected by another nhn cloud project. Please use it after verifying the sender domain. |
| 送信/照会 | false | -2051 | メール形式が正しくありません。 {}                                               | Email format is wrong. {}                                                                                       |
| 送信/照会 | false | -2052 | 'requestDate' は60日以内である必要があります。'requestDate' を更新してください。 | The 'requestDate' must be filed within 60 days. Please update the 'requestDate'. |
| 送信/照会 | false | -2053 | 無効な予約パラメータです。 | Invalid reservation parameter. |
| 送信/照会 | false | -2054 | ファイルにヘッダがありません。ファイル形式を確認してください。 | There is no header in the file. Please check the file format. |
| 送信/照会 | false | -2055 | ファイルに receiver_address ヘッダがありません。 | There is no receiver_address header in the file. |
| 送信/照会 | false | -2056 | ('startReadDate' と 'endReadDate') は必須です。 | The ('startReadDate' and 'endReadDate') are required. |
| 送信/照会 | false | -2058 | 予約キャンセルリクエストに失敗しました。 | Reservation cancel request is failed. |
| 送信/照会 | false | -2059 | 'statsId' が最大長を超えました。(8文字) | The 'statsId' exceeds maximum length. (8 chars) |
| 送信/照会 | false | -2060 | このリクエストはステータスを更新できません。大量メールリクエストの状態を確認してください。 | This request can't update status. Check status of this mass mail request. |
| 送信/照会 | false | -2062 | 'startMailStatusUpdateDate' と 'endMailStatusUpdateDate' は必須です。 | 'startMailStatusUpdateDate' and 'endMailStatusUpdateDate' are required. |
| 送信/照会 | false | -2101 | すでに存在するtemplateIdです。                                            | Already existed templateId.                                                                                     |
| 送信/照会 | false | -2107 | テンプレートが見つかりません。                                                   | Not found the template. templateId: {}                                                                          |
| 送信/照会 | false | -2108 | 'categoryId' は必須です。 | The 'categoryId' is required. |
| 送信/照会 | false | -2109 | 無効なAPIリクエストです。replaceパラメータを確認してください。 | Invalid Api Request. Check the replace parameter. |
| 送信/照会 | false | -2111 | 添付ファイルのあるテンプレートはサポートされていません。 | A template with attachments is not supported. templateId: {} |
| 送信/照会 | false | -2114 | templateIdには予約文字(/,?,:,<,>,%,&,",')を含めることはできません。 | templateId should not contain reserved character. (/,?,:,<,>,%,&," + '"' + ",') |
| 送信/照会 | false | -2115 | テンプレートが無効化されています。テンプレートを使用するには、テンプレートの状態を有効に変更してください。 | Template is disabled. If you want to use a template, change the template status to enabled. |
| 送信/照会 | false | -2701 | ドメイン認証に失敗しました。                                                   | Domain Verification failed. ({})                                                                                |
| 送信/照会 | false | -2703 | ドメインが認証されていません。 | Domain is not verified. |
| 送信/照会 | false | -2708 | 登録されていない サブドメインです。                                                 | No registered subdomain.                                                                                             |
| 送信/照会 | false | -2709 | 同一のルートドメインではありません。(senderAddress, X-TC-ENVELOPE-FROM) | No same root domain. (senderAddress, X-TC-ENVELOPE-FROM) |
| 送信/照会 | false | -2710 | X-TC-ENVELOPE-FROMはメール形式である必要があります。 | X-TC-ENVELOPE-FROM must be email format. |
| 送信/照会 | false | -2712 | 登録されていないドメインです。 | No registered domain. |
| 送信/照会 | false | -3001 | 無効なカテゴリです。 | Invalid category. |
| 送信/照会 | false | -4000 | Excelファイルが見つかりません。                                              | Not found excel file.                                                                                           |
| 送信/照会 | false | -4001 | 無効なパラメータです。{} | Invalid parameter. {} |
| 送信/照会 | false | -4002 | エクスポートファイルのアップロードに失敗しました。                                              | Failed to upload export file.                                                                                    |
| 送信/照会 | false | -4003 | ファイルのインポートに失敗しました。 | Failed to import file. |
| 送信/照会 | false | -4004 | エクスポートファイルリスト数の照会に失敗しました。 | Failed to get export file list. count. |
| 送信/照会 | false | -4005 | エクスポートファイルリストの照会に失敗しました。 | Failed to get export file list. |
| 送信/照会 | false | -4006 | インポートファイル情報の照会に失敗しました。 | Failed to get import file info. |
| 送信/照会 | false | -4007 | エクスポートファイルのダウンロードに失敗しました。 | Failed to download export file. |
| 送信/照会 | false | -4008 | インポートファイルのダウンロードに失敗しました。 | Failed to download import file. |
| 送信/照会 | false | -4009 | ファイルのインポートに失敗しました。 | Failed to importing file. |
| 送信/照会 | false | -4010 | サービスがエクスポートファイルのリクエスト/ダウンロードを許可していません。 | The service is not allowed to request/download export file. |
| テンプレート | false | -2100 | 'templateId' は必須です。 | The 'templateId' is required. |
| テンプレート | false | -2102 | 'templateName' は必須です。 | The 'templateName' is required. |
| テンプレート | false | -2103 | 'sendMailAddress' は必須です。 | The 'sendMailAddress' is required. |
| テンプレート | false | -2105 | 'body' は必須です。 | The 'body' is required. |
| テンプレート | false | -2110 | TemplateIdは50文字以下である必要があります。 | TemplateId must be 50 characters or less. |
| テンプレート | false | -2112 | 'title' は必須です。 | The 'title' is required. |
| 統計 | false | -2200 | 無効な統計検索パラメータです。 | Invalid statistics search parameter. |
| 統計 | false | -2201 | 'from' は 'to' より前である必要があります。 | The 'from' must be before 'to'. |
| 統計 | false | -2202 | 無効な期間です。 | Invalid duration time. |
| 統計 | false | -2203 | 'from' と 'to' は必須です。 | The 'from' and 'to' are required. |
| 統計 | false | -2204 | 'searchType' は必須です。 | The 'searchType' is required. |
| 統計 | false | -2205 | 'searchType' はサポートされていません。 | The 'searchType' is not supported. |
| 受信拒否 | false | -2300 | 'id' は必須です。 | The 'id' is required. |
| 受信拒否 | false | -2301 | パラメータサイズが無効です。 | Parameter size is invalid. |
| 受信拒否 | false | -2302 | パラメータ値が無効です。 | Parameter value is invalid. |
| 受信拒否 | false | -2303 | リストサイズは1000以下である必要があります。 | The size of list must be 1000 or fewer. |
| 受信拒否 | false | -2304 | すでに登録された番号です。                                                     | It is already registered number.                                                                                |
| 受信拒否 | false | -2305 | 'blockReceiverList' は必須です。 | The 'blockReceiverList' is required. |
| 受信拒否 | false | -2306 | 無効なアップロードファイルです。 | Invalid upload file. |
| 受信拒否 | false | -2307 | ファイルの読み込みに失敗しました。 | Fail to read files. |
| 受信拒否 | false | -2308 | 無効なメールアドレスです。 | Invalid email address. |
| 受信拒否 | false | -2309 | 受信拒否の追加に失敗しました。 | BlockReceiver add failed. |
| 受信拒否 | false | -2310 | 重複した受信拒否です。 | Duplicate BlockReceiver. |
