---
title: '方法: 16 進文字列と数値型の間で変換する - C# プログラミング ガイド'
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- hexadecimal strings [C#], converting to numeric type
- conversions [C#], hexidecimal strings
- strings [C#], converting hexadecimal strings
- hexadecimal strings [C#]
ms.assetid: 7115c49f-7d1d-40c3-8bd9-aae0cc1d46b6
ms.openlocfilehash: 2b896fb645113bc33b6a320948770947adc16dab
ms.sourcegitcommit: d6e27023aeaffc4b5a3cb4b88685018d6284ada4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67661140"
---
# <a name="how-to-convert-between-hexadecimal-strings-and-numeric-types-c-programming-guide"></a>方法: 16 進文字列と数値型の間で変換する (C# プログラミング ガイド)
以下の例では、次のタスクを実行する方法について説明します。  
  
- [string](../../../csharp/language-reference/keywords/string.md) の各文字の 16 進値を取得する。  
  
- 16 進文字列の各値に対応する [char](../../../csharp/language-reference/keywords/char.md) を取得する。  
  
- 16 進 `string` を [int](../../../csharp/language-reference/builtin-types/integral-numeric-types.md) に変換する。  
  
- 16 進 `string` を [float](../../../csharp/language-reference/builtin-types/floating-point-numeric-types.md) に変換する。  
  
- [バイト](../../../csharp/language-reference/builtin-types/integral-numeric-types.md)配列を 16 進 `string` に変換する。  
  
## <a name="example"></a>例  
 この例では、`string` の各文字の 16 進値を出力しています。 まず `string` を解析し、文字配列に変換します。 次いで、その数値を取得するために、各文字で <xref:System.Convert.ToInt32%28System.Char%29> を呼び出します。 最後に、その数を 16 進表現で `string` に書式設定します。  
  
 [!code-csharp[csProgGuideTypes#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#30)]  
  
## <a name="example"></a>例  
 この例は、16 進値の `string` を解析し、各 16 進値に対応する文字を出力しています。 まず、[Split(Char\[\])](xref:System.String.Split(System.Char[])) メソッドを呼び出して、各 16 進値を配列内の個別の `string` として取得します。 次いで <xref:System.Convert.ToInt32%28System.String%2CSystem.Int32%29> を呼び出し、16 進数値を [int](../../../csharp/language-reference/builtin-types/integral-numeric-types.md) の 10 進数値に変換します。ここでは、その文字コードに対応する文字を取得するための 2 つの方法を示しています。 1 つは、<xref:System.Char.ConvertFromUtf32%28System.Int32%29> を使用する方法です。これは、整数引数に対応する文字を `string` として返します。 もう 1 つは、`int` を明示的に [char](../../../csharp/language-reference/keywords/char.md) にキャストする方法です。  
  
 [!code-csharp[csProgGuideTypes#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#31)]  
  
## <a name="example"></a>例  
 この例では、<xref:System.Int32.Parse%28System.String%2CSystem.Globalization.NumberStyles%29> メソッドを呼び出し、16 進数 `string` を整数に変換する、別の方法を示します。  
  
 [!code-csharp[csProgGuideTypes#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#32)]  
  
## <a name="example"></a>例  
 次の例では、<xref:System.BitConverter?displayProperty=nameWithType> クラスおよび <xref:System.UInt32.Parse%2A?displayProperty=nameWithType> メソッドを使用して、16 進数の `string` を [float](../../../csharp/language-reference/builtin-types/floating-point-numeric-types.md) に変換する方法を示します。  
  
 [!code-csharp[csProgGuideTypes#39](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#39)]  
  
## <a name="example"></a>例  
 次の例は、<xref:System.BitConverter?displayProperty=nameWithType> クラスを使用して [byte](../../../csharp/language-reference/builtin-types/integral-numeric-types.md) 配列を 16 進文字列に変換する方法を示します。  
  
 [!code-csharp[csProgGuideTypes#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#38)]  
  
## <a name="see-also"></a>関連項目

- [Standard Numeric Format Strings](../../../standard/base-types/standard-numeric-format-strings.md)
- [型](../../../csharp/programming-guide/types/index.md)
- [方法: 文字列が数値を表しているかどうかを確認する](../../../csharp/programming-guide/strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)
