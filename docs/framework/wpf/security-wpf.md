---
title: セキュリティ (WPF)
ms.date: 03/30/2017
helpviewer_keywords:
- XAML files [WPF], sandbox behavior
- browser-hosted application security [WPF]
- application security [WPF]
- navigation security [WPF]
- loose XAML files [WPF], sandbox behavior
- WPF security [WPF]
- WebBrowser control [WPF], security
- feature controls [WPF], security
- XBAP security [WPF]
- Internet Explorer security settings [WPF]
ms.assetid: ee1baea0-3611-4e36-9ad6-fcd5205376fb
ms.openlocfilehash: 8d01e018e570a1ab530f476368d80f4082a73bda
ms.sourcegitcommit: 24a4a8eb6d8cfe7b8549fb6d823076d7c697e0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2019
ms.locfileid: "68400794"
---
# <a name="security-wpf"></a>セキュリティ (WPF)
<a name="introduction"></a>Windows Presentation Foundation (WPF) のスタンドアロンアプリケーションとブラウザーでホストされるアプリケーションを開発する場合は、セキュリティモデルを考慮する必要があります。 [!INCLUDE[TLA2#tla_wpf](../../../includes/tla2sharptla-wpf-md.md)]スタンドアロンアプリケーションは、Windows インストーラー (.msi)、XCopy、または ClickOnce を使用してデプロイされているかどうかにかかわらず、無制限のアクセス許可 (CAS**FullTrust**アクセス許可セット) で実行されます。 部分的に信頼されたスタンドアロンの WPF アプリケーションを ClickOnce で展開することはサポートされていません。 ただし、完全に信頼されたホストアプリケーションでは、.NET Framework <xref:System.AppDomain>アドインモデルを使用して部分信頼を作成できます。 詳細については、「 [WPF アドインの概要](./app-development/wpf-add-ins-overview.md)」を参照してください。  
  
 [!INCLUDE[TLA2#tla_wpf](../../../includes/tla2sharptla-wpf-md.md)]ブラウザーでホストされるアプリケーションは[!INCLUDE[TLA#tla_iegeneric](../../../includes/tlasharptla-iegeneric-md.md)] 、または Firefox によって[!INCLUDE[TLA#tla_xbap#plural](../../../includes/tlasharptla-xbapsharpplural-md.md)]ホストさ[!INCLUDE[TLA#tla_xaml](../../../includes/tlasharptla-xaml-md.md)]れます。詳細については、「 [WPF XAML ブラウザーアプリケーションの概要](./app-development/wpf-xaml-browser-applications-overview.md)」を参照してください。  
  
 [!INCLUDE[TLA2#tla_wpf](../../../includes/tla2sharptla-wpf-md.md)]ブラウザーでホストされるアプリケーションは、既定では、既定の CAS**インターネット**ゾーンのアクセス許可セットに制限されている部分信頼セキュリティサンドボックス内で実行されます。 これにより[!INCLUDE[TLA2#tla_wpf](../../../includes/tla2sharptla-wpf-md.md)] 、一般的な Web アプリケーションが分離されるのと同じ方法で、ブラウザーでホストされるアプリケーションがクライアントコンピューターから効果的に分離されます。 XBAP は、デプロイメント URL およびクライアントのセキュリティ構成のセキュリティ ゾーンに基づいて、完全な信頼まで特権を昇格することができます。 詳細については、「 [WPF 部分信頼セキュリティ](wpf-partial-trust-security.md)」を参照してください。  
  
 このトピックでは、Windows Presentation Foundation (WPF) のスタンドアロンアプリケーションとブラウザーでホストされるアプリケーションのセキュリティモデルについて説明します。  
  
 このトピックは、次のセクションで構成されています。  
  
- [安全なナビゲーション](#SafeTopLevelNavigation)  
  
- [Web ブラウザーのセキュリティ設定](#InternetExplorerSecuritySettings)  
  
- [WebBrowser コントロールと機能コントロール](#webbrowser_control_and_feature_controls)  
  
- [部分信頼クライアント アプリケーションに対する APTCA の無効化](#APTCA)  
  
- [Loose XAML ファイルに対するサンドボックスの動作](#LooseContentSandboxing)  
  
- [セキュリティを向上する WPF アプリケーションを開発するためのリソース](#BestPractices)  
  
<a name="SafeTopLevelNavigation"></a>   
## <a name="safe-navigation"></a>安全なナビゲーション  
 では[!INCLUDE[TLA2#tla_wpf](../../../includes/tla2sharptla-wpf-md.md)] 、アプリケーションとブラウザーという2種類のナビゲーションを区別します。 [!INCLUDE[TLA2#tla_xbap#plural](../../../includes/tla2sharptla-xbapsharpplural-md.md)]  
  
 *アプリケーション ナビゲーション*は、ブラウザーによってホストされるアプリケーション内のコンテンツ項目間のナビゲーションです。 *ブラウザー ナビゲーション*は、ブラウザー自体のコンテンツとロケーション URL を変更するナビゲーションです。 アプリケーションナビゲーション (通常は XAML) とブラウザーナビゲーション (通常は HTML) の関係を次の図に示します。
  
 ![アプリケーションナビゲーションとブラウザーナビゲーションの関係。](./media/security-wpf/application-browser-navigation-relationship.png)  
  
 の[!INCLUDE[TLA2#tla_xbap](../../../includes/tla2sharptla-xbap-md.md)]移動先として安全であると見なされるコンテンツの種類は、主にアプリケーションナビゲーションまたはブラウザーナビゲーションが使用されているかどうかによって決まります。  
  
<a name="Application_Navigation_Security"></a>   
### <a name="application-navigation-security"></a>アプリケーション ナビゲーションのセキュリティ  
 アプリケーションのナビゲーションは、次の4種類のコンテンツをサポート[!INCLUDE[TLA2#tla_uri](../../../includes/tla2sharptla-uri-md.md)]するパックで識別できる場合は安全と見なされます。  
  
|コンテンツ タイプ|説明|URI の例|  
|------------------|-----------------|-----------------|  
|リソース|ビルドの種類が**リソース**のプロジェクトに追加されるファイル。|`pack://application:,,,/MyResourceFile.xaml`|  
|Content|ビルドの種類が**コンテンツ**のプロジェクトに追加されるファイル。|`pack://application:,,,/MyContentFile.xaml`|  
|起点サイト|ビルドの種類が**None**のプロジェクトに追加されるファイル。|`pack://siteoforigin:,,,/MySiteOfOriginFile.xaml`|  
|アプリケーション コード|コンパイルされたコード分離を含む XAML リソース。<br /><br /> \- または -<br /><br /> **ページ**のビルドの種類を使用してプロジェクトに追加される XAML ファイル。|`pack://application:,,,/MyResourceFile` `.xaml`|  
  
> [!NOTE]
>  アプリケーションデータファイルとパック[!INCLUDE[TLA2#tla_uri#plural](../../../includes/tla2sharptla-urisharpplural-md.md)]の詳細については、「 [WPF アプリケーションのリソースファイル、コンテンツファイル、およびデータファイル](./app-development/wpf-application-resource-content-and-data-files.md)」を参照してください。  
  
 これらのコンテンツ タイプのファイルは、ユーザーまたはプログラムを使用して移動できます。  
  
- **ユーザー ナビゲーション**。 ユーザーは、要素を<xref:System.Windows.Documents.Hyperlink>クリックして移動します。  
  
- **プログラム ナビゲーション**。 たとえば、 <xref:System.Windows.Navigation.NavigationWindow.Source%2A?displayProperty=nameWithType>プロパティを設定するなどして、ユーザーを関与させずにアプリケーションを移動します。  
  
<a name="Browser_Navigation_Security"></a>   
### <a name="browser-navigation-security"></a>ブラウザー ナビゲーションのセキュリティ  
 ブラウザー ナビゲーションは、次の条件の下でのみ安全と見なされます。  
  
- **ユーザー ナビゲーション**。 ユーザーは、入れ子になっ<xref:System.Windows.Documents.Hyperlink> <xref:System.Windows.Controls.Frame>たではなく、main <xref:System.Windows.Navigation.NavigationWindow>内の要素をクリックして移動します。  
  
- **ゾーン**。 移動先のコンテンツが、インターネットまたはローカル イントラネット上に存在する。  
  
- **プロトコル**。 使用されるプロトコルは、 **http**、 **https**、 **file**、または**mailto**のいずれかです。  
  
 が、これらの条件<xref:System.Security.SecurityException>に準拠していない方法でコンテンツに移動しようとすると、がスローされます。[!INCLUDE[TLA2#tla_xbap](../../../includes/tla2sharptla-xbap-md.md)]  
  
<a name="InternetExplorerSecuritySettings"></a>   
## <a name="web-browsing-software-security-settings"></a>Web ブラウザーのセキュリティ設定  
 コンピューターのセキュリティ設定によって、Web ブラウザーに付与されるアクセス権が決まります。 Web ブラウザーソフトウェアには、 [WinINet](https://go.microsoft.com/fwlink/?LinkId=179379)または[UrlMon](https://go.microsoft.com/fwlink/?LinkId=179383) api を使用するアプリケーションまたはコンポーネントが含まれます (Internet Explorer とプレゼンテーションの cluster.exe を含む)。  
  
 [!INCLUDE[TLA2#tla_iegeneric](../../../includes/tla2sharptla-iegeneric-md.md)]には、次のような、またはから[!INCLUDE[TLA2#tla_iegeneric](../../../includes/tla2sharptla-iegeneric-md.md)]の実行が許可される機能を構成するためのメカニズムが用意されています。  
  
- .NET Framework 依存コンポーネント  
  
- ActiveX コントロールおよびプラグイン  
  
- ダウンロード  
  
- [スクリプティング]  
  
- ユーザー認証  
  
 この方法で保護できる機能のコレクションは、**インターネット**、**イントラネット**、**信頼済みサイト**、および**制限付きサイト**ゾーンのゾーン単位で構成されます。 次の手順では、セキュリティ設定の構成方法について説明します。  
  
1. **[コントロール パネル]** を開きます。  
  
2. **[ネットワークとインターネット]** をクリックし、 **[インターネットオプション]** をクリックします。  
  
     [インターネット オプション] ダイアログ ボックスが表示されます。  
  
3. **[セキュリティ]** タブで、セキュリティ設定を構成するゾーンを選択します。  
  
4. **[レベルのカスタマイズ]** ボタンをクリックします。  
  
     **[セキュリティの設定]** ダイアログボックスが表示され、選択したゾーンのセキュリティ設定を構成できます。  
  
     ![[セキュリティの設定] ダイアログボックスを表示するスクリーンショット。](./media/security-wpf/windows-presentation-foundation-security-settings.png)  
  
> [!NOTE]
>  [インターネット オプション] ダイアログ ボックスは、Internet Explorer から開くこともできます。 **[ツール]** をクリックし、 **[インターネットオプション]** をクリックします。  
  
 [!INCLUDE[TLA#tla_ie7](../../../includes/tlasharptla-ie7-md.md)]以降では、.NET Framework に特化した次のセキュリティ設定が含まれています。  
  
- **Loose XAML**。 が[!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)]ファイル[!INCLUDE[TLA2#tla_iegeneric](../../../includes/tla2sharptla-iegeneric-md.md)]に移動できるかどうかを制御します。 ([有効]、[無効]、および [ダイアログを表示する] オプション)。  
  
- **XAML ブラウザー アプリケーション**。 がに[!INCLUDE[TLA2#tla_iegeneric](../../../includes/tla2sharptla-iegeneric-md.md)]移動して実行[!INCLUDE[TLA2#tla_xbap#plural](../../../includes/tla2sharptla-xbapsharpplural-md.md)]できるかどうかを制御します。 ([有効]、[無効]、および [ダイアログを表示する] オプション)。  
  
 既定では、これらの設定はすべて、**インターネット**、**ローカルイントラネット**、および**信頼済みサイト**ゾーンで有効になっています。また、**制限付きサイト**ゾーンでは無効になっています。  
  
<a name="Security_Settings_for_IE6_and_Below"></a>   
### <a name="security-related-wpf-registry-settings"></a>セキュリティ関連の WPF レジストリの設定  
 [インターネット オプション] から使用できるセキュリティ設定以外に、セキュリティ上重要なさまざまな WPF 機能を選択的にブロックするために次のレジストリ値を使用できます。 値は次のキーで定義されます。  
  
 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\Windows Presentation Foundation\Features`  
  
 設定可能な値を次の表に示します。  
  
|値名|値型|値のデータ|  
|----------------|----------------|----------------|  
|XBAPDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|LooseXamlDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|WebBrowserDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|MediaAudioDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|MediaImageDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|MediaVideoDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|ScriptInteropDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
  
<a name="webbrowser_control_and_feature_controls"></a>   
## <a name="webbrowser-control-and-feature-controls"></a>WebBrowser コントロールと機能コントロール  
 WPF <xref:System.Windows.Controls.WebBrowser>コントロールは、Web コンテンツをホストするために使用できます。 WPF <xref:System.Windows.Controls.WebBrowser>コントロールは、基になる WebBrowser ActiveX コントロールをラップします。 Wpf は、wpf <xref:System.Windows.Controls.WebBrowser>コントロールを使用して信頼されていない Web コンテンツをホストするときに、アプリケーションのセキュリティを確保するためのサポートを提供します。 ただし、一部のセキュリティ機能は、 <xref:System.Windows.Controls.WebBrowser>コントロールを使用してアプリケーションで直接適用する必要があります。 WebBrowser ActiveX コントロールの詳細については、「 [Webbrowser コントロールの概要とチュートリアル](https://go.microsoft.com/fwlink/?LinkId=179388)」を参照してください。  
  
> [!NOTE]
>  このセクションは、 <xref:System.Windows.Controls.Frame> <xref:System.Windows.Controls.WebBrowser>を使用して HTML コンテンツに移動するため、コントロールにも適用されます。  
  
 信頼され<xref:System.Windows.Controls.WebBrowser>ていない Web コンテンツをホストするために WPF コントロールを使用する場合、 <xref:System.AppDomain>アプリケーションは、悪意のある可能性のある HTML スクリプトコードからアプリケーションコードを分離するために、部分信頼を使用する必要があります。 これは、アプリケーションが<xref:System.Windows.Controls.WebBrowser.InvokeScript%2A>メソッド<xref:System.Windows.Controls.WebBrowser.ObjectForScripting%2A>とプロパティを使用してホストされているスクリプトと対話する場合に特に当てはまります。 詳細については、「 [WPF アドインの概要](./app-development/wpf-add-ins-overview.md)」を参照してください。  
  
 アプリケーションで WPF <xref:System.Windows.Controls.WebBrowser>コントロールを使用する場合は、セキュリティを強化し、攻撃を軽減するもう1つの方法は、Internet Explorer の機能コントロールを有効にすることです。 機能コントロールは、管理者および開発者が、WPF <xref:System.Windows.Controls.WebBrowser>コントロールをラップする WebBrowser ActiveX コントロールをホストする internet explorer とアプリケーションの機能を構成できるようにする internet explorer の追加機能です。 機能コントロールは、 [Cointernetsetfeatureenabled](https://go.microsoft.com/fwlink/?LinkId=179394)関数を使用するか、レジストリの値を変更することによって構成できます。 機能コントロールの詳細については、「[機能コントロール](https://go.microsoft.com/fwlink/?LinkId=179390)と[インターネット機能コントロール](https://go.microsoft.com/fwlink/?LinkId=179392)の概要」を参照してください。  
  
 Wpf <xref:System.Windows.Controls.WebBrowser>コントロールを使用するスタンドアロン wpf アプリケーションを開発している場合、wpf は、アプリケーションに対して次の機能コントロールを自動的に有効にします。  
  
|機能コントロール|  
|---------------------|  
|FEATURE_MIME_HANDLING|  
|FEATURE_MIME_SNIFFING|  
|FEATURE_OBJECT_CACHING|  
|FEATURE_SAFE_BINDTOOBJECT|  
|FEATURE_WINDOW_RESTRICTIONS|  
|FEATURE_ZONE_ELEVATION|  
|FEATURE_RESTRICT_FILEDOWNLOAD|  
|FEATURE_RESTRICT_ACTIVEXINSTALL|  
|FEATURE_ADDON_MANAGEMENT|  
|FEATURE_HTTP_USERNAME_PASSWORD_DISABLE|  
|FEATURE_SECURITYBAND|  
|FEATURE_UNC_SAVEDFILECHECK|  
|FEATURE_VALIDATE_NAVIGATE_URL|  
|FEATURE_DISABLE_TELNET_PROTOCOL|  
|FEATURE_WEBOC_POPUPMANAGEMENT|  
|FEATURE_DISABLE_LEGACY_COMPRESSION|  
|FEATURE_SSLUX|  
  
 これらの機能コントロールは無条件で有効になるため、完全信頼アプリケーションに悪影響が及ぶ場合があります。 この場合、特定のアプリケーションとそのアプリケーションがホストしているコンテンツにセキュリティ上のリスクがなければ、対応する機能コントロールを無効にできます。  
  
 機能コントロールは、WebBrowser ActiveX オブジェクトをインスタンス化するプロセスによって適用されます。 そのため、信頼されていないコンテンツに移動できるスタンドアロン アプリケーションを作成する場合は、その他の機能コントロールを有効にすることを検討すべきです。  
  
> [!NOTE]
>  この推奨事項は、MSHTML および SHDOCVW ホスト セキュリティの一般的な推奨事項に基づいています。 詳細については[、「MSHTML ホストのセキュリティに関する FAQ」を参照してください。第 2](https://go.microsoft.com/fwlink/?LinkId=179396)部のパート[I と MSHTML ホストのセキュリティに関する FAQ:II](https://go.microsoft.com/fwlink/?LinkId=179415)のパート ii。  
  
 実行可能ファイルでは、レジストリ値を 1 に設定して以下の機能コントロールを有効にすることを検討してください。  
  
|機能コントロール|  
|---------------------|  
|FEATURE_ACTIVEX_REPURPOSEDETECTION|  
|FEATURE_BLOCK_LMZ_IMG|  
|FEATURE_BLOCK_LMZ_OBJECT|  
|FEATURE_BLOCK_LMZ_SCRIPT|  
|FEATURE_RESTRICT_RES_TO_LMZ|  
|FEATURE_RESTRICT_ABOUT_PROTOCOL_IE7|  
|FEATURE_SHOW_APP_PROTOCOL_WARN_DIALOG|  
|FEATURE_LOCALMACHINE_LOCKDOWN|  
|FEATURE_FORCE_ADDR_AND_STATUS|  
|FEATURE_RESTRICTED_ZONE_WHEN_FILE_NOT_FOUND|  
  
 実行可能ファイルでは、レジストリ値を 0 に設定して以下の機能コントロールを無効にすることを検討してください。  
  
|機能コントロール|  
|---------------------|  
|FEATURE_ENABLE_SCRIPT_PASTE_URLACTION_IF_PROMPT|  
  
 で[!INCLUDE[TLA#tla_xbap](../../../includes/tlasharptla-xbap-md.md)] <xref:System.Windows.Controls.WebBrowser> wpfコントロールを含む部分信頼を実行すると、wpfはInternetExplorerプロセスのアドレス空間でWebBrowserActiveXコントロールをホストします。[!INCLUDE[TLA#tla_iegeneric](../../../includes/tlasharptla-iegeneric-md.md)] Webbrowser activex コントロールは[!INCLUDE[TLA2#tla_iegeneric](../../../includes/tla2sharptla-iegeneric-md.md)]プロセスでホストされるため、Internet Explorer のすべての機能コントロールが webbrowser activex コントロールに対しても有効になります。  
  
 Internet Explorer で実行されている XBAP にも、標準のスタンドアロン アプリケーションよりも高いレベルのセキュリティが適用されます。 この追加のセキュリティが必要になるのは、Internet Explorer (したがって、WebBrowser ActiveX コントロール) が[!INCLUDE[TLA#tla_winvista](../../../includes/tlasharptla-winvista-md.md)]既定[!INCLUDE[win7](../../../includes/win7-md.md)]でおよびでは保護モードで実行されるためです。 保護モードの詳細については、「 [Internet Explorer の保護モードの概要と操作](https://go.microsoft.com/fwlink/?LinkId=179393)」を参照してください。  
  
> [!NOTE]
>  インターネットゾーン<xref:System.Windows.Controls.WebBrowser> <xref:System.Security.SecurityException>内で、WPF コントロールを含む XBAP を Firefox で実行しようとすると、がスローされます。 これは、WPF セキュリティ ポリシーが原因です。  
  
<a name="APTCA"></a>   
## <a name="disabling-aptca-assemblies-for-partially-trusted-client-applications"></a>部分信頼クライアント アプリケーションに対する APTCA の無効化  
 マネージアセンブリがにインストール[!INCLUDE[TLA#tla_gac](../../../includes/tlasharptla-gac-md.md)]されると、ユーザーはそれらをインストールするための明示的なアクセス許可を提供する必要があるため、完全に信頼されます。 完全に信頼されているため、これらを使用できるのは完全信頼マネージド クライアント アプリケーションのみです。 部分的に信頼されたアプリケーションでそれらを使用できるようにする<xref:System.Security.AllowPartiallyTrustedCallersAttribute>には、(APTCA) でマークする必要があります。 この属性は、部分信頼で実行しても安全であるとテストで確認されたアセンブリだけに設定します。  
  
 ただし、APTCA アセンブリがにインストール[!INCLUDE[TLA2#tla_gac](../../../includes/tla2sharptla-gac-md.md)]された後にセキュリティの欠陥を発生させる可能性があります。 セキュリティ上の欠陥が検出されたら、アセンブリの発行者は、既存のインストールでの問題を解決し、問題発見後に発生する可能性があるインストールに備えるため、セキュリティ更新プログラムを作成できます。 更新プログラムの 1 つのオプションとして、アセンブリのアンインストールが考えられますが、その場合はこのアセンブリを使用する他の完全信頼クライアント アプリケーションを破損するおそれがあります。  
  
 [!INCLUDE[TLA2#tla_wpf](../../../includes/tla2sharptla-wpf-md.md)]aptca アセンブリをアンインストールせずに部分信頼[!INCLUDE[TLA2#tla_xbap#plural](../../../includes/tla2sharptla-xbapsharpplural-md.md)]の aptca アセンブリを無効にできる機構を提供します。  
  
 APTCA アセンブリを無効にするには、特殊なレジストリ キーを作成する必要があります。  
  
 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\policy\APTCA\<AssemblyFullName>, FileVersion=<AssemblyFileVersion>`  
  
 次のコードは一例を示しています。  
  
 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\policy\APTCA\aptcagac, Version=1.0.0.0, Culture=neutral, PublicKeyToken=215e3ac809a0fea7, FileVersion=1.0.0.0`  
  
 このキーにより、APTCA アセンブリのエントリが設定されます。 また、アセンブリを有効または無効にする値をこのキーに作成する必要があります。 値の詳細を次に示します。  
  
- 値の名前:**APTCA_FLAG**。  
  
- 値の種類:**REG_DWORD**。  
  
- 値のデータ:無効にするには**1** 。有効にする場合は**0** 。  
  
 部分信頼クライアント アプリケーションに対してアセンブリを無効にする必要がある場合は、レジストリ キーおよび値を作成する更新プログラムを作成します。  
  
> [!NOTE]
>  コア .NET Framework アセンブリは、マネージアプリケーションを実行するために必要なため、この方法で無効にしても影響を受けません。 APTCA アセンブリの無効化のサポートは、主にサードパーティ アプリケーションを対象にしたものです。  
  
<a name="LooseContentSandboxing"></a>   
## <a name="sandbox-behavior-for-loose-xaml-files"></a>Loose XAML ファイルに対するサンドボックスの動作  
 ルース[!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)]ファイルは、分離コード、イベントハンドラー、またはアプリケーション固有のアセンブリに依存しない、マークアップのみの XAML ファイルです。 圧縮[!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)]されていないファイルがブラウザーから直接移動すると、既定のインターネットゾーンのアクセス許可セットに基づいてセキュリティサンドボックスに読み込まれます。  
  
 ただし、圧縮[!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)]されていないファイルを<xref:System.Windows.Navigation.NavigationWindow> <xref:System.Windows.Controls.Frame>スタンドアロンアプリケーションで移動する場合は、セキュリティの動作が異なります。  
  
 どちらの場合も、移動[!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)]先のルースファイルはホストアプリケーションのアクセス許可を継承します。 ただし、この動作はセキュリティの観点から望ましくない可能性があります。 [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)]特に、信頼されていないか不明なエンティティによってルースファイルが生成された場合などです。 この種類のコンテンツは*外部コンテンツ*として知られ<xref:System.Windows.Controls.Frame>て<xref:System.Windows.Navigation.NavigationWindow>おり、との両方を、移動時に分離するように構成できます。 分離を実現するには 、と<xref:System.Windows.Controls.Frame> <xref:System.Windows.Navigation.NavigationWindow>の次の例に示すように、SandboxExternalContent プロパティを true に設定します。  
  
 [!code-xaml[SecurityOverviewSnippets#FrameMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/SecurityOverviewSnippets/CS/Window2.xaml#framemarkup)]  
  
 [!code-xaml[SecurityOverviewSnippets#NavigationWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/SecurityOverviewSnippets/CS/Window1.xaml#navigationwindowmarkup)]  
  
 このように設定すると、外部コンテンツは、アプリケーションをホストするプロセスとは異なるプロセスに読み込まれます。 このプロセスは既定のインターネット ゾーン アクセス許可セットに限定されており、ホスト アプリケーションとクライアント コンピューターから外部コンテンツを効果的に分離します。  
  
> [!NOTE]
>  スタンドアロンアプリケーションの[!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] <xref:System.Windows.Navigation.NavigationWindow>または<xref:System.Windows.Controls.Frame>からのルースファイルへの移動は、プレゼンテーションホストプロセスを含む WPF ブラウザーホスティングインフラストラクチャに基づいて実装されますが、セキュリティレベルは[!INCLUDE[wiprlhext](../../../includes/wiprlhext-md.md)]との Internet Explorer でコンテンツが直接読み込まれる場合 (これは[!INCLUDE[win7](../../../includes/win7-md.md)]引き続き、プレゼンテーションホストを通じて行われます) よりも少し少ないです。 これは、Web ブラウザーを使用しているスタンドアロン WPF アプリケーションに、Internet Explorer の保護モード セキュリティ機能が追加されていないためです。  
  
<a name="BestPractices"></a>   
## <a name="resources-for-developing-wpf-applications-that-promote-security"></a>セキュリティを向上する WPF アプリケーションを開発するためのリソース  
 セキュリティを強化するアプリケーションの開発[!INCLUDE[TLA2#tla_wpf](../../../includes/tla2sharptla-wpf-md.md)]に役立つその他のリソースを次に示します。  
  
|区分|リソース|  
|----------|--------------|  
|マネージド コード|[patterns & practices アプリケーション セキュリティ ガイダンス インデックス](https://go.microsoft.com/fwlink/?LinkId=117426)|  
|CAS|[コード アクセス セキュリティ](../misc/code-access-security.md)|  
|ClickOnce|[ClickOnce のセキュリティと配置](/visualstudio/deployment/clickonce-security-and-deployment)|  
|[!INCLUDE[TLA2#tla_wpf](../../../includes/tla2sharptla-wpf-md.md)]|[WPF 部分信頼セキュリティ](wpf-partial-trust-security.md)|  
  
## <a name="see-also"></a>関連項目

- [WPF 部分信頼セキュリティ](wpf-partial-trust-security.md)
- [WPF のセキュリティ方針 - プラットフォーム セキュリティ](wpf-security-strategy-platform-security.md)
- [WPF のセキュリティ方針 - セキュリティ エンジニアリング](wpf-security-strategy-security-engineering.md)
- [patterns & practices アプリケーション セキュリティ ガイダンス インデックス](https://go.microsoft.com/fwlink/?LinkId=117426)
- [コード アクセス セキュリティ](../misc/code-access-security.md)
- [ClickOnce のセキュリティと配置](/visualstudio/deployment/clickonce-security-and-deployment)
- [XAML の概要 (WPF)](./advanced/xaml-overview-wpf.md)
