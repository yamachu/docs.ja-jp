---
ms.openlocfilehash: 5aa37169ae0e5c5e6b88aae5ceb5d758286c3d3a
ms.sourcegitcommit: d55e14eb63588830c0ba1ea95a24ce6c57ef8c8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67804510"
---
### <a name="new-64-bit-jit-compiler-in-the-net-framework-46"></a>.NET Framework 4.6 の新しい 64 ビット JIT コンパイラ

|   |   |
|---|---|
|説明|.NET Framework 4.6 以降では、Just-In-Time コンパイルには新しい 64 ビット JIT コンパイラが使用されます。 場合によっては、予期しない例外がスローされるか、32 ビット コンパイラまたは以前の 64 ビット JIT コンパイラを使用してアプリを実行したときとは動作が異なる可能性があります。 この変更は 32 ビット JIT コンパイラには影響しません。これまで次のような違いの存在が確認されています。<ul><li>特定の条件下では、最適化が有効なリリース ビルドの <xref:System.NullReferenceException> がボックス化解除操作でスローされる場合があります。</li><li>場合によっては、大きなメソッド本文の実働コードの実行時に <xref:System.StackOverflowException> がスローされることがあります。</li><li>特定の条件下では、メソッドに渡された構造体が、リリース ビルドの値の型ではなく、参照型として扱われます。 この問題の兆候の 1 つは、コレクション内の個々の項目が予期しない順序で表示されることです。</li><li>特定の条件下では、最適化が有効な場合、高ビットが設定された <xref:System.UInt16> 値が正しく比較されません。</li><li>特定の条件下では、特に、配列値の初期化中に、<xref:System.Reflection.Emit.OpCodes.Initblk?displayProperty=nameWithType> IL 命令でのメモリ初期化により、正しくない値でメモリが初期化される場合があります。 その結果、ハンドルされない例外または正しくない出力が発生する場合があります。</li><li>特定のまれな条件下では、コンパイラの最適化が有効な場合に、条件付きのビット テストで正しくない <xref:System.Boolean> 値が返されたり、例外がスローされたりすることがあります。</li><li>特定の条件下では、<code>if</code> ステートメントを使用して、<code>try</code> ブロックの開始前と <code>try</code> ブロックの終了時の条件をテストし、同じ条件を <code>catch</code> または <code>finally</code> ブロックで評価する場合、新しい 64 ビット JIT コンパイラが、コードの最適化の際に <code>catch</code> または <code>finally</code> ブロックから <code>if</code> 条件を削除します。 その結果、<code>catch</code> または <code>finally</code> ブロックの <code>if</code> ステートメント内のコードは無条件で実行されます。</li></ul>|
|提案される解決策|<strong>既知の問題の軽減策</strong> <br/> 上記の問題が発生する場合は、次のいずれかの方法で解決できます。<ul><li>.NET Framework 4.6.2 にアップグレードします。 .NET Framework 4.6.2 に含まれている新しい 64 ビット コンパイラは、これらの既知の問題のそれぞれに対処します。</li><li>Windows Update を実行して、Windows のバージョンが最新のものであることを確認します。 .NET Framework 4.6 および 4.6.1 へのサービス更新により、ボックス化解除操作での <xref:System.NullReferenceException> を除き、これらの問題のそれぞれに対処することができます。</li><li>古い 64 ビット JIT コンパイラでコンパイルします。 この方法の詳細については、「<strong>その他の問題の軽減策</strong>」を参照してください。</li></ul><strong>その他の問題の軽減策</strong> <br/> 古い 64 ビット コンパイラと新しい 64 ビット JIT コンパイラでコンパイルされたコードの動作、またはアプリのデバッグ バージョンとリリース バージョン (両方とも新しい 64 ビット JIT コンパイラでコンパイル) の動作に違いが見られる場合は、次のようにして、古い 64 ビット JIT コンパイラでアプリをコンパイルすることができます。<ul><li>アプリケーションごとに、アプリケーションの構成ファイルに [<](~/docs/framework/configure-apps/file-schema/runtime/uselegacyjit-element.md) 要素を追加できます。 次のように新しい 64 ビット JIT コンパイラでのコンパイルを無効にし、代わりに従来の 64 ビット JIT コンパイラを使用します。</li></ul><pre><code class="lang-xml">&lt;?xml version =&quot;1.0&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;useLegacyJit enabled=&quot;1&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre><ul><li>ユーザーごとに、<code>useLegacyJit</code> という <code>REG_DWORD</code> 値をレジストリの <code>HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework</code> キーに追加できます。 値を 1 にすると、従来の 64 ビット JIT コンパイラが有効になり、値を 0 にすると、従来の 64 ビット JIT コンパイラが無効になり、新しい 64 ビット JIT コンパイラが有効になります。</li><li>コンピューターごとに、<code>useLegacyJit</code> という名前の <code>REG_DWORD</code> 値をレジストリの <code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework</code> キーに追加できます。 値を <code>1</code> にすると、従来の 64 ビット JIT コンパイラが有効になり、値を <code>0</code> にすると、従来の 64 ビット JIT コンパイラが無効になり、新しい 64 ビット JIT コンパイラが有効になります。</li></ul>[Microsoft Connect](https://connect.microsoft.com/VisualStudio) でバグを報告し、問題を弊社に知らせることもできます。|
|スコープ|エッジ|
|Version|4.6|
|型|再ターゲット中|

