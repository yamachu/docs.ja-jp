---
title: '方法: ファイルから XML を読み込む (C#)'
ms.date: 07/20/2015
ms.assetid: 3ed38487-8028-4209-9872-c8dce0ed4dfe
ms.openlocfilehash: cd4e45767b2f72de8d9a3de9814da6260d2413fe
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2019
ms.locfileid: "66485313"
---
# <a name="how-to-load-xml-from-a-file-c"></a>方法: ファイルから XML を読み込む (C#)
このトピックでは、<xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType> メソッドを使用して URI から XML を読み込む方法について説明します。  
  
## <a name="example"></a>例  
 次の例では、ファイルから XML ドキュメントを読み込む方法を示します。 この例では、books.xml を読み込んで、XML ツリーをコンソールに出力します。  
  
 この例では、XML ドキュメント、「[サンプル XML ファイル:書籍 (LINQ to XML)](../../../../csharp/programming-guide/concepts/linq/sample-xml-file-books-linq-to-xml.md) を使用します。  
  
```csharp  
XElement booksFromFile = XElement.Load(@"books.xml");  
Console.WriteLine(booksFromFile);  
```  
  
 このコードを実行すると、次の出力が生成されます。  
  
```xml  
<Catalog>  
  <Book id="bk101">  
    <Author>Garghentini, Davide</Author>  
    <Title>XML Developer's Guide</Title>  
    <Genre>Computer</Genre>  
    <Price>44.95</Price>  
    <PublishDate>2000-10-01</PublishDate>  
    <Description>An in-depth look at creating applications   
      with XML.</Description>  
  </Book>  
  <Book id="bk102">  
    <Author>Garcia, Debra</Author>  
    <Title>Midnight Rain</Title>  
    <Genre>Fantasy</Genre>  
    <Price>5.95</Price>  
    <PublishDate>2000-12-16</PublishDate>  
    <Description>A former architect battles corporate zombies,   
      an evil sorceress, and her own childhood to become queen   
      of the world.</Description>  
  </Book>  
</Catalog>  
```  
  