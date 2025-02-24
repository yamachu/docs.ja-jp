---
title: 算術演算子
description: 使用可能な算術演算子について説明します、F#プログラミング言語。
ms.date: 04/04/2018
ms.openlocfilehash: 74ab813a7ca5018b6bd084aea10627e4afd62015
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65641655"
---
# <a name="arithmetic-operators"></a>算術演算子

このトピックでは、F# 言語で利用できる算術演算子について説明します。

## <a name="summary-of-binary-arithmetic-operators"></a>二項算術演算子の概要

次の表は、整数および浮動小数点型のボックス化解除された使用可能な二項算術演算子をまとめたものです。

|二項演算子|メモ|
|---------------|-----|
|`+` (さらに、plus)|オフにします。 番号が一緒に追加されたときに、オーバーフローが発生し、合計の種類でサポート絶対値の最大値を超えています。|
|`-` (減算、マイナス記号)|オフにします。 アンダー フロー条件の符号なしの型が差し引かれるとき、または種類で表される浮動小数点値が小さすぎます。|
|`*` (乗算、回)|オフにします。 数値が乗算ときに、オーバーフローが発生し、製品の種類でサポート絶対値の最大値を超えています。|
|`/` (除算、除算)|原因 0 による除算、<xref:System.DivideByZeroException>整数型。 浮動小数点型は、0 による除算に提供する特殊な浮動小数点値`+Infinity`または`-Infinity`します。 アンダー フロー条件の種類によって表される浮動小数点数が小さすぎる場合にあります。|
|`%` (残りの部分、rem など)|除算の剰余を返します。 結果の符号は、最初のオペランドの符号と同じです。|
|`**` (のべき乗に累乗)|結果が、型の絶対値の最大値を超えると、オーバーフローが発生します。<br /><br />指数演算子は、浮動小数点型でのみ動作します。|

## <a name="summary-of-unary-arithmetic-operators"></a>単項算術演算子の概要

次の表では、整数および浮動小数点型で利用可能な単項算術演算子をまとめたものです。

|単項演算子|メモ|
|--------------|-----|
|`+` (正)|算術式に適用できます。 値の符号は変更されません。|
|`-` (否定、負の値)|算術式に適用できます。 値の符号を変更します。|

オーバーフローまたはアンダー フローの整数型で動作では、ラップします。 これにより、不適切な結果です。 整数のオーバーフローは、ソフトウェアがそのアカウントに書き込まれないときに、セキュリティの問題に投稿できる可能性がある重大な問題です。 アプリケーションの問題では場合の checked 演算子の使用を検討`Microsoft.FSharp.Core.Operators.Checked`します。

## <a name="summary-of-binary-comparison-operators"></a>二項比較演算子の概要

次の表では、整数および浮動小数点型で利用可能な 2 項比較演算子を示します。 これらの演算子は、型の値を返す`bool`します。

浮動小数点数のことはありません直接比較等しいかどうか、IEEE 浮動小数点表現は正確な等価演算をサポートしていないためです。 あるコードを調べることによって簡単に確認できます 2 つの数値は、別のビット表現を実際にがあります。

|演算子|メモ|
|--------|-----|
|`=` (等値を代入)|これは、代入演算子ではありません。 比較のためにのみ使用されます。 これは、一般的な演算子です。|
|`>` (より大きい)|これは、一般的な演算子です。|
|`<` (より小さい)|これは、一般的な演算子です。|
|`>=` (より大きいまたは等しい)|これは、一般的な演算子です。|
|`<=` (より小さいまたは等しい)|これは、一般的な演算子です。|
|`<>` (等しくない)|これは、一般的な演算子です。|

## <a name="overloaded-and-generic-operators"></a>演算子のオーバー ロードされたとジェネリック

定義されているすべてのこのトピックで説明されている演算子は、 **Microsoft.FSharp.Core.Operators**名前空間。 一部の演算子は、静的に解決される型パラメーターを使用して定義されます。 これは、演算子で使用する特定の種類ごとに個別の定義があることを意味します。 すべての単項および二項算術演算子とビットごとの演算子は、このカテゴリには。 比較演算子はジェネリックでありしたがっていないだけプリミティブ数値型、任意の型を処理します。 判別共用体、レコードの種類は、F# コンパイラによって生成される、独自のカスタム実装があります。 クラス型、メソッドを使用して、<xref:System.Object.Equals%2A>します。

ジェネリックの演算子はカスタマイズできます。 比較関数をカスタマイズする上書き<xref:System.Object.Equals%2A>、独自のカスタムの等値比較を提供し、実装に<xref:System.IComparable>します。 <xref:System.IComparable?displayProperty=nameWithType>インターフェイスが 1 つのメソッドには、<xref:System.IComparable.CompareTo%2A>メソッド。

## <a name="operators-and-type-inference"></a>演算子と型推論

式の演算子の使用には、その演算子の型の推定が制限されます。 また、演算子の使用では、演算子の使用は演算型を意味するために、自動ジェネリック化ができないようにします。 その他の情報がない場合、F#コンパイラが推論`int`算術式の型として。 別の型を指定することで、この動作をオーバーライドできます。 引数の型および戻り値の型のため`function1`ように、次のコードが推論されます`int`の型が、`function2`と推論されます`float`します。

[!code-fsharp[Main](../../../../samples/snippets/fsharp/lang-ref-1/snippet3501.fs)]

## <a name="see-also"></a>関連項目

- [シンボルと演算子のリファレンス](index.md)
- [演算子のオーバーロード](../operator-overloading.md)
- [ビット処理演算子](bitwise-operators.md)
- [ブール演算子](boolean-operators.md)
