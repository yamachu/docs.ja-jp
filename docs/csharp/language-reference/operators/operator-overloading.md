---
title: 演算子のオーバーロード - C# リファレンス
description: C# 演算子をオーバーロードする方法と、どの C# 演算子がオーバーロード可能かについて説明します。
ms.date: 07/05/2019
f1_keywords:
- operator_CSharpKeyword
helpviewer_keywords:
- operator keyword [C#]
- operator overloading [C#]
ms.openlocfilehash: 77f9d37b7f3232bb1f9bad0466916336801572dd
ms.sourcegitcommit: 463f3f050cecc0b6403e67f19a61f870fb8e7b7d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68512384"
---
# <a name="operator-overloading-c-reference"></a>演算子のオーバーロード (C# リファレンス)

ユーザー定義型は定義済みの C# 演算子をオーバーロードできます。 つまり、1 つまたは両方のオペランドに該当する型は、演算のカスタム実装を提供できます。 オーバーロード可能な C# 演算子は、「[オーバーロード可能な演算子](#overloadable-operators)」のセクションで示します。

演算子の宣言には `operator` キーワードを使用します。 演算子の宣言では、次の規則を満たす必要があります。

- これには、`public` と `static` 修飾子の両方が含まれています。
- 単項演算子に指定できるパラメーター数は 1 です。 二項演算子に指定できるパラメーター数は 2 です。 どちらの場合も、少なくとも 1 つのパラメーターの型が `T` または `T?` でなければなりません。`T` は演算子の宣言が含まれる型です。

次の例は、有理数を表す簡略化された構造を定義しています。 構造体がいくつかの[算術演算子](arithmetic-operators.md)をオーバーロードします。

[!code-csharp[fraction example](~/samples/csharp/language-reference/operators/OperatorOverloading.cs)]

`int` から `Fraction` への暗黙的な変換を定義することで、前の例を拡張できます。 その場合、オーバーロードされた演算子はこれら 2 つの型の引数をサポートします。 つまり、整数を分数に足し、結果として分数を取得できるようになります。

また、`operator` キーワードを使用してカスタムの型変換を定義することもできます。 詳細については、「[User-defined conversion operators](user-defined-conversion-operators.md)」(ユーザー定義の変換演算子) を参照してください。

## <a name="overloadable-operators"></a>オーバーロード可能な演算子

次の表は、C# 演算子のオーバーロード可/不可に関する情報を示します。

| 演算子 | オーバーロード可/不可 |
| --------- | --------------- |
|[+](arithmetic-operators.md#unary-plus-and-minus-operators)、[-](arithmetic-operators.md#unary-plus-and-minus-operators)、[!](boolean-logical-operators.md#logical-negation-operator-)、[~](bitwise-and-shift-operators.md#bitwise-complement-operator-)、[++](arithmetic-operators.md#increment-operator-)、[--](arithmetic-operators.md#decrement-operator---)、[true](true-false-operators.md)、[false](true-false-operators.md)|これらの単項演算子はオーバーロードできます。|
|[+](addition-operator.md)、[-](subtraction-operator.md)、[\*](arithmetic-operators.md#multiplication-operator-)、[/](arithmetic-operators.md#division-operator-)、[%](arithmetic-operators.md#remainder-operator-)、[&](boolean-logical-operators.md#logical-and-operator-)、[&#124;](boolean-logical-operators.md#logical-or-operator-)、[^](boolean-logical-operators.md#logical-exclusive-or-operator-)、[\<\<](bitwise-and-shift-operators.md#left-shift-operator-)、[>>](bitwise-and-shift-operators.md#right-shift-operator-)、[==](equality-operators.md#equality-operator-)、[!=](equality-operators.md#inequality-operator-)、[\<](comparison-operators.md#less-than-operator-)、[>](comparison-operators.md#greater-than-operator-)、[\<=](comparison-operators.md#less-than-or-equal-operator-)、[>=](comparison-operators.md#greater-than-or-equal-operator-)|これらの 2 項演算子はオーバーロードできます。 特定の演算子はペアでオーバーロードする必要があります。詳細については、この表の後にある注を参照してください。|
|[&&](boolean-logical-operators.md#conditional-logical-and-operator-)、[&#124;&#124;](boolean-logical-operators.md#conditional-logical-or-operator-)|条件付き論理演算子は、オーバーロードできません。 ただし、オーバーロードされた [`true` および `false` 演算子](true-false-operators.md)を含む型が特定の方法で `&` または <code>&#124;</code> もオーバーロードしている場合は、その型のオペランドに対してそれぞれ `&&` または <code>&#124;&#124;</code> 演算子の評価が可能になります。 詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の[ユーザー定義型条件論理演算子](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators)に関するセクションを参照してください。|
|[&#91;&#93;](member-access-operators.md#indexer-operator-)|要素へのアクセスはオーバーロード可能な演算子とは見なされていませんが、[インデクサー](../../programming-guide/indexers/index.md)を定義することができます。|
|[(T)x](type-testing-and-conversion-operators.md#cast-operator-)|キャスト演算子はオーバーロードできませんが、新しい変換演算子を定義できます。 詳細については、「[User-defined conversion operators](user-defined-conversion-operators.md)」(ユーザー定義の変換演算子) を参照してください。|
|[+=](arithmetic-operators.md#compound-assignment), [-=](arithmetic-operators.md#compound-assignment), [\*=](arithmetic-operators.md#compound-assignment), [/=](arithmetic-operators.md#compound-assignment), [%=](arithmetic-operators.md#compound-assignment), [&=](boolean-logical-operators.md#compound-assignment), [&#124;=](boolean-logical-operators.md#compound-assignment), [^=](boolean-logical-operators.md#compound-assignment), [\<\<=](bitwise-and-shift-operators.md#compound-assignment), [>>=](bitwise-and-shift-operators.md#compound-assignment)|複合代入演算子を明示的にオーバーロードすることはできません。 ただし、二項演算子をオーバーロードするとき、対応する複合代入演算子がある場合は、それも暗黙的にオーバーロードされます。 たとえば、`+=` は、オーバーロード可能な `+` を使用して評価されます。|
|[=](assignment-operator.md)、[.](member-access-operators.md#member-access-operator-)、[?:](conditional-operator.md)、[??](null-coalescing-operator.md)、[->](pointer-related-operators.md#pointer-member-access-operator--)、[=>](lambda-operator.md)、[f(x)](member-access-operators.md#invocation-operator-)、[as](type-testing-and-conversion-operators.md#as-operator)、[checked](../keywords/checked.md)、[unchecked](../keywords/unchecked.md)、[default](../../programming-guide/statements-expressions-operators/default-value-expressions.md)、[delegate](delegate-operator.md)、[is](type-testing-and-conversion-operators.md#is-operator)、[nameof](nameof.md)、[new](new-operator.md)、[sizeof](sizeof.md)、[stackalloc](stackalloc.md)、[typeof](type-testing-and-conversion-operators.md#typeof-operator)|これらの演算子はオーバーロードできません。|

> [!NOTE]
> 比較演算子は、ペアでオーバーロードする必要があります。 つまり、ペアのどちらかの演算子をオーバーロードする場合、もう一方の演算子もオーバーロードする必要があります。 次のようなペアがこれに該当します。
>
> - `==` 演算子と `!=` 演算子
> - `<` 演算子と `>` 演算子
> - `<=` 演算子と `>=` 演算子

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [演算子のオーバーロード](~/_csharplang/spec/expressions.md#operator-overloading)
- [演算子](~/_csharplang/spec/classes.md#operators)

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [ユーザー定義の変換演算子](user-defined-conversion-operators.md)
- [C# のオーバーロードされた演算子が常に静的である理由](https://blogs.msdn.microsoft.com/ericlippert/2007/05/14/why-are-overloaded-operators-always-static-in-c/)
