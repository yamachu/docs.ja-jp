---
title: この呼び出しは待機されなかったため、現在のメソッドの実行は呼び出しの完了を待たずに続行されます。
ms.date: 07/20/2015
f1_keywords:
- bc42358
- vbc42358
helpviewer_keywords:
- BC42358
ms.assetid: 43342515-c3c8-4155-9263-c302afabcbc2
ms.openlocfilehash: c4704fb09e9519c14f29365b2cf7f536bbbc5dca
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2019
ms.locfileid: "64619561"
---
# <a name="because-this-call-is-not-awaited-the-current-method-continues-to-run-before-the-call-is-completed"></a>この呼び出しは待機されなかったため、現在のメソッドの実行は呼び出しの完了を待たずに続行されます。
この呼び出しは待機されなかったため、現在のメソッドの実行は呼び出しの完了を待たずに続行されます。 呼び出しの結果に 'Await' 演算子を適用することを検討してください。  
  
 現在のメソッドは <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> を返す非同期メソッドを呼び出し、 [Await](../../../visual-basic/language-reference/operators/await-operator.md) 演算子を結果に適用しません。 この非同期メソッドの呼び出しは、非同期タスクを開始します。 ただし、 `Await` 演算子が適用されないため、プログラムはタスクの完了を待たずに処理を続行します。 ほとんどの場合、この動作は想定されていません。 通常は、呼び出し元のメソッドの他の側面が呼び出しの結果に依存します。また、最低でも、呼び出しを含むメソッドから制御が返される前に、呼び出されたメソッドが完了することが想定されます。  
  
 同様に、呼び出された非同期メソッドで発生した例外に対する処理も重要です。 <xref:System.Threading.Tasks.Task> または  <xref:System.Threading.Tasks.Task%601> を返すメソッド内で発生した例外は、返されたタスクに格納されます。 このタスクが返されるのを待たない場合や例外を明示的にチェックしない場合、例外は失われます。 このタスクが返されるのを待機する場合は、例外が再スローされます。  
  
 ベスト プラクティスとしては、常に呼び出しを待機する必要があります。  
  
 既定では、このメッセージは警告です。 警告を表示しない方法や、警告をエラーとして扱う方法の詳細については、「 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)」を参照してください。  
  
 **エラー ID:** BC42358  
  
### <a name="to-address-this-warning"></a>この警告に対処するには  
  
- 非同期呼び出しの完了を待つ必要がなく、呼び出されたメソッドで例外が発生しないことが確実である場合に限り、警告を抑制することを検討してください。 その場合は、呼び出しのタスクの結果を変数に割り当てることで警告を抑制することができます。  
  
     次の例で、警告を発生させる方法、警告を抑制する方法、呼び出しを待機する方法を示します。  
  
    ```vb  
    Async Function CallingMethodAsync() As Task  
  
        ResultsTextBox.Text &= vbCrLf & "  Entering calling method."  
  
        ' Variable delay is used to slow down the called method so that you  
        ' can distinguish between awaiting and not awaiting in the program's output.   
        ' You can adjust the value to produce the output that this topic shows   
        ' after the code.  
        Dim delay = 5000  
  
        ' Call #1.  
        ' Call an async method. Because you don't await it, its completion isn't   
        ' coordinated with the current method, CallingMethodAsync.  
        ' The following line causes the warning.  
        CalledMethodAsync(delay)  
  
        ' Call #2.  
        ' To suppress the warning without awaiting, you can assign the   
        ' returned task to a variable. The assignment doesn't change how  
        ' the program runs. However, the recommended practice is always to  
        ' await a call to an async method.  
        ' Replace Call #1 with the following line.  
        'Task delayTask = CalledMethodAsync(delay)  
  
        ' Call #3  
        ' To contrast with an awaited call, replace the unawaited call   
        ' (Call #1 or Call #2) with the following awaited call. The best   
        ' practice is to await the call.  
  
        'Await CalledMethodAsync(delay)  
  
        ' If the call to CalledMethodAsync isn't awaited, CallingMethodAsync  
        ' continues to run and, in this example, finishes its work and returns  
        ' to its caller.  
        ResultsTextBox.Text &= vbCrLf & "  Returning from calling method."  
    End Function  
  
    Async Function CalledMethodAsync(howLong As Integer) As Task  
  
        ResultsTextBox.Text &= vbCrLf & "    Entering called method, starting and awaiting Task.Delay."  
        ' Slow the process down a little so you can distinguish between awaiting  
        ' and not awaiting. Adjust the value for howLong if necessary.  
        Await Task.Delay(howLong)  
        ResultsTextBox.Text &= vbCrLf & "    Task.Delay is finished--returning from called method."  
    End Function  
    ```  
  
     この例では、呼び出し 1 または呼び出し 2 を選択した場合、完了が待機されない非同期メソッド (`CalledMethodAsync`) は、呼び出し元 (`CallingMethodAsync`) と呼び出し元の呼び出し元 (`StartButton_Click`) の両方が完了した後に完了します。 次の出力の最後の行に、呼び出されたメソッドがいつ完了したかが示されています。 この出力には、完全な例の `CallingMethodAsync` を呼び出すイベント ハンドラーへのエントリとその終了が示されています。  
  
    ```  
    Entering the Click event handler.  
      Entering calling method.  
        Entering called method, starting and awaiting Task.Delay.  
      Returning from calling method.  
    Exiting the Click event handler.  
        Task.Delay is finished--returning from called method.  
    ```  
  
## <a name="example"></a>例  
 次の Windows Presentation Foundation (WPF) アプリケーションには、前の例のメソッドが含まれています。 このアプリケーションを設定するには、次の手順を実行します。  
  
1. WPF アプリケーションを作成し、 `AsyncWarning`という名前を付けます。  
  
2. Visual Studio コード エディターで、 **[MainWindow.xaml]** タブをクリックします。  
  
     タブが表示されない場合は、 **ソリューション エクスプローラー**で MainWindow.xaml のショートカット メニューを開き、 **[コードの表示]** を選択します。  
  
3. MainWindow.xaml の **[XAML]** ビューで、コードを次のコードに置き換えます。  
  
    ```vb  
    <Window x:Class="MainWindow"  
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"  
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  
            Title="MainWindow" Height="350" Width="525">  
        <Grid>  
            <Button x:Name="StartButton" Content="Start" HorizontalAlignment="Left" Margin="214,28,0,0" VerticalAlignment="Top" Width="75" HorizontalContentAlignment="Center" FontWeight="Bold" FontFamily="Aharoni" Click="StartButton_Click" />  
            <TextBox x:Name="ResultsTextBox" Margin="0,80,0,0" TextWrapping="Wrap" FontFamily="Lucida Console"/>  
        </Grid>  
    </Window>  
    ```  
  
     ボタンとテキスト ボックスを含むシンプルなウィンドウが、MainWindow.xaml の **[デザイン]** ビューに表示されます。  
  
     XAML デザイナーの詳細については、「[XAML デザイナーを使用した UI の作成](/visualstudio/designers/creating-a-ui-by-using-xaml-designer-in-visual-studio)」を参照してください。 独自の単純な UI を構築する方法については、「[チュートリアル: Async と Await を使用した Web へのアクセス」の WPF アプリケーションの作成に関するセクションと単純な WPF MainWindow のデザインに関するセクションを参照してください。Async と Await を使用した Web へのアクセス](../../../visual-basic/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)」をご覧ください。  
  
4. MainWindow.xaml.vb のコードを次のコードに置き換えます。  
  
    ```vb  
    Class MainWindow   
  
        Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)  
  
            ResultsTextBox.Text &= vbCrLf & "Entering the Click event handler."  
            Await CallingMethodAsync()  
            ResultsTextBox.Text &= vbCrLf & "Exiting the Click event handler."  
        End Sub  
  
        Async Function CallingMethodAsync() As Task  
  
            ResultsTextBox.Text &= vbCrLf & "  Entering calling method."  
  
            ' Variable delay is used to slow down the called method so that you  
            ' can distinguish between awaiting and not awaiting in the program's output.   
            ' You can adjust the value to produce the output that this topic shows   
            ' after the code.  
            Dim delay = 5000  
  
            ' Call #1.  
            ' Call an async method. Because you don't await it, its completion isn't   
            ' coordinated with the current method, CallingMethodAsync.  
            ' The following line causes the warning.  
            CalledMethodAsync(delay)  
  
            ' Call #2.  
            ' To suppress the warning without awaiting, you can assign the   
            ' returned task to a variable. The assignment doesn't change how  
            ' the program runs. However, the recommended practice is always to  
            ' await a call to an async method.  
  
            ' Replace Call #1 with the following line.  
            'Task delayTask = CalledMethodAsync(delay)  
  
            ' Call #3  
            ' To contrast with an awaited call, replace the unawaited call   
            ' (Call #1 or Call #2) with the following awaited call. The best   
            ' practice is to await the call.  
  
            'Await CalledMethodAsync(delay)  
  
            ' If the call to CalledMethodAsync isn't awaited, CallingMethodAsync  
            ' continues to run and, in this example, finishes its work and returns  
            ' to its caller.  
            ResultsTextBox.Text &= vbCrLf & "  Returning from calling method."  
        End Function  
  
        Async Function CalledMethodAsync(howLong As Integer) As Task  
  
            ResultsTextBox.Text &= vbCrLf & "    Entering called method, starting and awaiting Task.Delay."  
            ' Slow the process down a little so you can distinguish between awaiting  
            ' and not awaiting. Adjust the value for howLong if necessary.  
            Await Task.Delay(howLong)  
            ResultsTextBox.Text &= vbCrLf & "    Task.Delay is finished--returning from called method."  
        End Function  
  
    End Class  
  
    ' Output  
  
    ' Entering the Click event handler.  
    '   Entering calling method.  
    '     Entering called method, starting and awaiting Task.Delay.  
    '   Returning from calling method.  
    ' Exiting the Click event handler.  
    '     Task.Delay is finished--returning from called method.  
  
    ' Output  
  
    ' Entering the Click event handler.  
    '   Entering calling method.  
    '     Entering called method, starting and awaiting Task.Delay.  
    '     Task.Delay is finished--returning from called method.  
    '   Returning from calling method.  
    ' Exiting the Click event handler.  
    ```  
  
5. F5 キーを押してプログラムを実行し、 **[Start]** を複数回クリックします。  
  
     想定される出力がコードの最後に表示されます。  
  
## <a name="see-also"></a>関連項目

- [Await 演算子](../../../visual-basic/language-reference/operators/await-operator.md)
- [Async および Await を使用した非同期プログラミング](../../../visual-basic/programming-guide/concepts/async/index.md)
