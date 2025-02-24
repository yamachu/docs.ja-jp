---
title: トランスポートの選択
ms.date: 03/30/2017
helpviewer_keywords:
- choosing transports [WCF]
ms.assetid: b169462b-f7b6-4cf4-9fca-d306909ee8bf
ms.openlocfilehash: 611e8df29b37efd880ee1d19515697d899e4fa7e
ms.sourcegitcommit: bab17fd81bab7886449217356084bf4881d6e7c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2019
ms.locfileid: "67402159"
---
# <a name="choosing-a-transport"></a>トランスポートの選択
このトピックでは、Windows Communication Foundation (WCF) に含まれている 3 つの主なトランスポートの中から選択するための条件について説明します。HTTP、TCP、および名前付きパイプします。 WCF にも含まれています、トランスポート、メッセージ キュー (MSMQ とも呼ばれます) は、このドキュメントは、メッセージ キューを対応していません。  
  
 WCF プログラミング モデルを分離エンドポイントの操作 (サービス コントラクトで表される) と 2 つのエンドポイントを接続するトランスポート機構から。 これによって、ネットワークへのサービスの公開方法を柔軟に決定できます。  
  
 WCF では、ネットワーク経由でデータを使用してエンドポイント間で転送する方法を指定する、*バインド*、一連の構成をされます*バインド要素*します。 トランスポートは、バインディングの一部であるトランスポート バインド要素で表されます。 バインディングにはセキュリティなどのオプションのプロトコル バインド要素、必須のメッセージ エンコーダーのバインド要素、および必須のトランスポートのバインド要素が含まれます。 トランスポートは、シリアル化されたメッセージを別のアプリケーションとの間で送受信します。  
  
 既存のクライアントまたはサーバーに接続する必要がある場合、特定のトランスポートを使用するという選択ができない可能性があります。 ただし、WCF サービスが、それぞれ異なるトランスポートを使用して複数のエンドポイントからアクセスできることができます。 単一のトランスポートでサービスの提供対象に対応できない場合、複数のエンドポイントにサービスを公開することを検討してください。 複数のエンドポイントに公開すると、クライアント アプリケーションはそのアプリケーションに最も適したエンドポイントを使用できます。  
  
 トランスポートを選択した後、そのトランスポートを使用するバインディングを選択する必要があります。 システム指定のバインディングを選択することができます (を参照してください[System-Provided Bindings](../../../../docs/framework/wcf/system-provided-bindings.md))、独自のカスタム バインドを構築することもできます (を参照してください[カスタム バインド](../../../../docs/framework/wcf/extending/custom-bindings.md))。 独自のバインディングを作成することもできます。 詳細については、次を参照してください。[ユーザー定義バインディング](../../../../docs/framework/wcf/extending/creating-user-defined-bindings.md)します。  
  
## <a name="advantages-of-each-transport"></a>各トランスポートの利点  
 ここでは、3 つの主なトランスポートの中から 1 つを選択する主な理由について説明します。また、トランスポートの選択に使用できる詳細な表も記載しています。  
  
### <a name="when-to-use-http-transport"></a>HTTP トランスポートを使用する状況  
 HTTP は、クライアントとサーバー間の要求/応答プロトコルです。 最も一般的なアプリケーションは、Web サーバーと通信する Web ブラウザー クライアントで構成されます。 クライアントは、クライアントの要求メッセージをリッスンしているサーバーに要求を送信します。 サーバーは要求を受信すると、要求のステータスを含んだ応答を返します。 成功した場合は、Web ページ、エラー メッセージ、その他の情報など、任意のデータが返されます。 HTTP プロトコルの詳細については、次を参照してください。 [Hypertext Transfer PROTOCOL](https://go.microsoft.com/fwlink/?LinkId=94858)します。  
  
 HTTP プロトコルは、接続ベースではありません。応答の送信後、状態は維持されません。 複数ページ トランザクションを処理するには、アプリケーションが、必要な状態を維持する必要があります。  
  
 Wcf では、HTTP トランスポートの WCF 以外のレガシ システムと相互運用性バインディングが最適化されています。 すべての通信相手が WCF を使用している場合は、TCP ベースまたは名前付きパイプベースのバインディングが高速です。 詳細については、次のトピックを参照してください。 <xref:System.ServiceModel.NetTcpBinding> および <xref:System.ServiceModel.NetNamedPipeBinding>  
  
### <a name="when-to-use-the-tcp-transport"></a>TCP トランスポートを使用する状況  
 TCP は、エンドツーエンドのエラー検出とエラー訂正を備えた接続ベースのストリーム指向配信サービスです。 *接続に基づく*データを交換する前に、ホスト間の通信セッションが確立されていることを意味します。 ホストは、TCP/IP ネットワーク上で論理 IP アドレスによって識別される任意のデバイスです。  
  
 TCP は使いやすく、TCP によって信頼できるデータ配信を実現できます。 特に、TCP は、送信者にパケット配信を通知し、送信者が送信した順序と同じ順序でパケットが配信されることを保証し、紛失したパケットを再転送し、データ パケットが重複していないことを確認します。 この信頼性の高い配信が 2 つの TCP/IP ノード間で適用されと同じものでないことに注意してください*WS-ReliableMessaging*エンドポイント間で適用される、中間のノードの数に関係なく、含めることができます。  
  
 WCF TCP トランスポートは、通信の両方の end が WCF を使用しているシナリオに最適です。 このバインディングは、異なるコンピューター間の通信が発生するシナリオで最も高速な WCF バインドします。 メッセージ交換では、<xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> を使用して、メッセージ転送の最適化を実現します。 TCP では双方向通信が提供されるので、クライアントがネットワーク アドレス交換 (NAT) の内側にある場合でも、TCP を使用して双方向コントラクトを実装できます。  
  
### <a name="when-to-use-the-named-pipe-transport"></a>名前付きパイプ トランスポートを使用する状況  
 名前付きパイプとは、プロセスが通信に使用できる共有メモリのセクションなど、Windows オペレーティング システム カーネルのオブジェクトです。 名前付きパイプには名前があり、1 台のコンピューター上でのプロセス間の一方向通信または双方向通信に使用できます。  
  
 通信は、1 台のコンピューター上の異なる WCF アプリケーション間で必要な別のコンピューターからの通信をできないようにすると、名前付きパイプ トランスポートを使用します。 追加の制限として、Windows リモート デスクトップから実行しているプロセスを同じ Windows リモート デスクトップのセッションに制限することができます。ただし、そのプロセスにシステム特権がある場合を除きます。  
  
> [!WARNING]
>  IIS でホストされている複数のサイトで弱いワイルドカードの URL 予約を名前付きパイプ トランスポートを使用する場合、次のエラーが表示される場合があります。サイト '2' をリッスンしているときにアクティブ化サービス、プロトコル 'net.pipe' の ' NetPipeActivator' でエラーが発生しました、したがって、プロトコルは、サイトの一時的に無効です。 詳細については、例外メッセージを参照してください。 URL:WeakWildcard:net.pipe:/\<マシン名 >/状態。ConflictingRegistration 例外:プロセス名:SMSvcHost プロセス ID:1076\  
  
## <a name="decision-points-for-choosing-a-transport"></a>トランスポート選択の判断材料  
 トランスポートの選択に使用する一般的な判断材料を次の表に示します。 アプリケーションに適用される追加の属性とトランスポートについて考慮する必要があります。 アプリケーションに重要な属性を特定し、各属性と相性の良いトランスポートを特定し、最後にその属性セットで最も適切に機能するトランスポートを選択します。  
  
|属性|説明|望ましいトランスポート|  
|---------------|-----------------|------------------------|  
|診断|診断によって、トランスポートの接続問題を自動的に検出できます。 すべてのトランスポートは、接続に関するエラー情報を返信する機能をサポートしています。 ただし、WCF では、ネットワークの問題を調査するための診断ツールは含まれません。|なし|  
|ホスト|すべての WCF エンドポイントは、アプリケーション内でホストされている必要があります。 IIS 6.0 およびそれ以前のバージョンは、HTTP トランスポートを使用するだけのホスティング アプリケーションをサポートします。 [!INCLUDE[wv](../../../../includes/wv-md.md)]サポートが TCP を含めた、すべての WCF トランスポートをホストするための追加し、名前付きパイプします。 詳細については、次を参照してください。[インターネット インフォメーション サービスをホストしている](../../../../docs/framework/wcf/feature-details/hosting-in-internet-information-services.md)と[Windows プロセス アクティブ化サービスでのホスティング](../../../../docs/framework/wcf/feature-details/hosting-in-windows-process-activation-service.md)します。|HTTP|  
|検査|検査は、転送中に、メッセージからの情報を抽出し、処理する機能です。 HTTP プロトコルでは、ルーティングと制御情報がデータから分離されるので、メッセージを調査して分析するツールの作成が容易になります。 検査しやすいトランスポートでは、ネットワーク機器に求められる処理能力も低い場合があります。 使用するセキュリティのレベルは、メッセージを調査できるかどうかに影響を与えます。|HTTP|  
|待ち時間|待ち時間は、メッセージの交換を完了するために必要な最小限の時間です。 すべてのネットワーク操作には、選択したトランスポートに応じて、多少の待ち時間が発生します。 HTTP など、ネイティブ メッセージ交換パターンが要求/応答であるトランスポートで双方向または一方向通信を使用すると、メッセージの強制的な関連付けが原因で追加の待ち時間が発生する可能性があります。 このような場合、TCP など、ネイティブ メッセージ交換パターンが双方向であるトランスポートを使用することを検討してください。|TCP、名前付き<br /><br /> パイプ|  
|範囲|トランスポートの範囲は、トランスポートが他のシステムと接続できる能力を反映しています。 名前付きパイプ トランスポートの範囲は非常に狭く、同じコンピューター上で実行しているサービスにしか接続できません。 TCP トランスポートと HTTP トランスポートの範囲は広く、一部の NAT とファイアウォール構成を通ることができます。 詳細については、次を参照してください。 [Nat とファイアウォールの使用](../../../../docs/framework/wcf/feature-details/working-with-nats-and-firewalls.md)します。|HTTP、TCP|  
|セキュリティ|セキュリティは、機密性、整合性、または認証を提供することによって、転送中にメッセージを保護する機能です。 機密性では、メッセージが調査されないように保護し、整合性では、メッセージが変更されないように保護します。認証では、メッセージの送信者または受信者が本物であることを保証します。<br /><br /> WCF には、転送セキュリティ、メッセージ レベルとトランスポート レベルの両方がサポートしています。 トランスポートがバッファー転送モードをサポートする場合、メッセージ セキュリティはそのトランスポートで構成されます。 トランスポート セキュリティのサポートは、選択したトランスポートによって異なります。 HTTP、TCP、および名前付きパイプの各トランスポートは、トランスポート セキュリティに対して同等の適切なサポートを提供します。|すべて|  
|スループット|スループットは、指定の期間内に送信し、処理できるデータの量を示します。 待ち時間のように、選択したトランスポートが、サービス操作のスループットに影響する場合があります。 トランスポートのスループットを最大にするには、コンテンツを送信するときのオーバーヘッドおよびメッセージ交換が完了するまでの待ち時間を最小限に抑えることが必要です。 TCP トランスポートと名前付きパイプ トランスポートは、メッセージ本文に対するオーバーヘッドが小さく、メッセージ応答の待ち時間を短縮するネイティブ双方向パターンをサポートします。|TCP、名前付きパイプ|  
|ツール|ツールは、開発、診断、ホスティング、およびその他のアクティビティに関する、サードパーティ アプリケーションによるプロトコルへのサポートを表します。 HTTP プロトコルで動作するツールとソフトウェアの開発には、大規模な投資が行われています。|HTTP|  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.BasicHttpBinding>
- <xref:System.ServiceModel.WSHttpBinding>
- <xref:System.ServiceModel.WSDualHttpBinding>
- <xref:System.ServiceModel.WSFederationHttpBinding>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.NetTcpBinding>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement>
- <xref:System.ServiceModel.NetNamedPipeBinding>
- <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>
- [バインディング](../../../../docs/framework/wcf/feature-details/bindings.md)
- [システム標準のバインディング](../../../../docs/framework/wcf/system-provided-bindings.md)
- [ユーザー定義バインディングの作成](../../../../docs/framework/wcf/extending/creating-user-defined-bindings.md)
