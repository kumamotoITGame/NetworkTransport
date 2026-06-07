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

```
NetworkTransport/
├ package.json
├ Runtime/
  ├ NetworkTransport.asmdef
  ├ NetworkDef.cs
  ├ PacketQueue.cs
  ├ TransportTCP.cs
  └ TransportUDP.cs
```

---

# ■ package.json について（UPM定義）

`package.json` は、このフォルダをUnity Package Manager（UPM）として認識させるための設定ファイルです。

```json
{
  "name": "com.yourcompany.networktransport",
  "version": "1.0.0",
  "displayName": "Network Transport Library",
  "unity": "2023.2",
  "description": "TCP/UDP networking library for Unity projects"
}
```

● 役割

- Unityに「これはパッケージです」と伝える
- バージョン管理を行う
- 他プロジェクトから再利用可能にする

"name": "com.yourcompany.networktransport", ←ここはあなたの名前を書いてください

例）"name": "com.ohara_penpen.networktransport",<br>
例）"name": "com.capcom.networktransport",

# ■ NetworkTransport.asmdef について

```JSON
{
  "name": "NetworkTransport",
  "rootNamespace": "NetworkTransport"
}
```

● 役割

- このフォルダのコードを1つのアセンブリ（DLL）としてまとめる
- コンパイル単位を分離することでビルド時間を短縮
- 依存関係を明確化する
- 他プロジェクトへの移植性を向上させる

# ■ 別プロジェクトでの使い方

● Git URLで追加

```JSON
{
  "dependencies": {
    "com.yourcompany.networktransport": "https://github.com/yourname/NetworkTransport.git"
  }
}
```

//github.com/yourname/NetworkTransport.git" ←こはあなたのGitURL先を書く

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

