パッケージマネージャー使用path
https://github.com/Ud128/NetworkLibrary.git?path=/Packages/com.ohara.networktransportlibrary

Unityで自作したLibraryをGitで取得する<br>
参考資料：https://zenn.dev/ganariya/articles/create-import-package-from-upm

# NetworkTransport Library

Unity向けの軽量TCP/UDP通信ライブラリです。  
スレッドベースのソケット通信とパケットキュー方式により、ゲーム処理と通信処理を分離しています。

---

# ■ 概要

このライブラリは、Unityプロジェクト間で再利用できる通信モジュールとして設計されています。

主な特徴：

- TCP / UDP 両対応
- スレッドベース通信（非同期処理）
- 送受信データのキュー管理（PacketQueue）
- サーバー・クライアント両対応設計

---

# ■ フォルダ構成

手動でPackageのフォルダーを開き com.ohara.networktransportlibrary といった名前の新規フォルダを作ってください<br>
ohara の部分は会社名などにしてください

com.ohara.networktransportlibrary フォルダの中に package.json を手動で作ってください

`package.json` は、このフォルダをUnity Package Manager（UPM）として認識させるための設定ファイルです。
package.json の中身は下記のようにしてください

```json
{
    "name": "com.ohara.networktransportlibrary",
    "displayName": "Network Transport Library",
    "version": "1.0.0",
    "unity": "2022.2",
    "description": "Unity Network library.",
    "keywords": [],
    "license": "",
    "category": "",
    "dependencies": {}
}
```
上記のように作成後、Unityを立ち上げると下記のようなフォルダ構成になっているので<br>
Unity上で フォルダを作成し Runtimeを作ってライブラリーとして必要なスクリプトを入れてください
Unity上で Assembly Definition を作成し、
名前を com.ohara.networktransportlibrary に設定してください。
下記のようなフォルダー構成にしてください

```
Packages/Network Transport Library/
├ package.json
├ Runtime/
  ├ com.ohara.networktansportlibrary.asmdef
  ├ NetworkDef.cs
  ├ PacketQueue.cs
  ├ TransportTCP.cs
  └ TransportUDP.cs
```

---

● Assembly Definition

"name": "com.ohara.networktransportlibrary"", ← ohara は会社名を書くところです

例）"name": "com.capcom.networktransport",

# ■ 別プロジェクトでの使い方

パッケージマネージャーで https://github.com/Ud128/NetworkLibrary.git?path=/Packages/com.ohara.networktransportlibrary を指定してaddする

# ■ Unity上での利用方法

● TCPクライアント例

```csharp
using NetworkTransport;

public class SampleClient : MonoBehaviour
{
    TransportTCP tcp;

    void Start()
    {
        tcp = new TransportTCP();
        tcp.Connect(IP_Address, 50765);
    }

    void Update()
    {
        byte[] buffer = new byte[1400];
        int size = tcp.Receive(ref buffer, buffer.Length);

        if (size > 0)
        {
            Debug.Log("Received: " + size);
        }
    }
}
```

● サーバー起動

```csharp
tcp.StartServer(50765, 10);
```

● データ送信

```csharp
byte[] data = System.Text.Encoding.UTF8.GetBytes("Hello");
tcp.Send(data, data.Length);
```

