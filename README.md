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

手動でPackageのフォルダーを開き com.ohara.networktansportlibrary といった名前の新規フォルダを作ってください<br>
ohara の部分は会社名などにしてください

om.ohara.networktansportlibrary フォルダの中に package.json を主導で作ってください

`package.json` は、このフォルダをUnity Package Manager（UPM）として認識させるための設定ファイルです。
package.json の中身は下記のようにしてください

```json
{
    "name": "com.ohara.networktansportlibrary",
    "displayName": "Network Tansport Library",
    "version": "1.0.0",
    "unity": "2022.2",
    "description": "Unity Network library.",
    "keywords": [],
    "license": "",
    "category": "",
    "dependencies": {}
}
```
上記のように作成後、Unityを立ち上げると下記のようなフォルダー攻勢になっているので<br>
Unity上で Create＞Folder でフォルダを作成し Runtimeを作ってライブラリーとして必要なスクリプトを入れてください
Unity上で AssemblyDefinition を作り データ名はcom.ohara.networktansportlibraryとしInspectorでNameのところを Network Tansport Library としてください
下記のようなフォルダー構成にしてください

```
Packages/Network Tansport Library/
├ package.json
├ Runtime/
  ├ com.ohara.networktansportlibrary.asmdef
  ├ NetworkDef.cs
  ├ PacketQueue.cs
  ├ TransportTCP.cs
  └ TransportUDP.cs
```

---

● AssemblyDefinition の役割

- Unityに「これはパッケージです」と伝える
- バージョン管理を行う
- 他プロジェクトから再利用可能にする

"name": "com.ohara.networktansportlibrary"", ← ohara は会社名を書くところです

例）"name": "com.capcom.networktransport",

# ■ 別プロジェクトでの使い方

パッケージマネージャーで https://github.com/Ud128/NetworkLibrary.git?path=/Packages/com.ohara.networktansportlibrary を指定してaddする

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

