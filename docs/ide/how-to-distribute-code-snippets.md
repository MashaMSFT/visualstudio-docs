---
title: "How to: Distribute code snippets"
ms.date: 11/04/2016
ms.prod: visual-studio-dev15
ms.technology: vs-ide-general
ms.topic: conceptual
helpviewer_keywords:
  - "code snippets, distributing"
ms.assetid: 5f717abd-e167-47ae-818c-6b0bae100ceb
author: gewarren
ms.author: gewarren
manager: douge
dev_langs:
  - "VB"
ms.workload:
  - "multiple"
---
# How to: Distribute code snippets

You can give your code snippets to your friends and have them install the snippets on their own computers by using **Code Snippets Manager**. However, if you have several snippets to distribute or would like to distribute them more widely, you include your snippet file in a Visual Studio extension. Visual Studio users can then install the extension.

You must install the Visual Studio SDK in order to create Visual Studio extensions. Find the version of the VSSDK that matches your Visual Studio installation at [Visual Studio downloads](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

## Set up the extension

In this procedure we'll use the same Hello World code snippet that's created in [Walkthrough: Create a code snippet](../ide/walkthrough-creating-a-code-snippet.md). We'll supply the *.snippet* text, so you don't have to go back and make one.

1. Create a new VSIX project named **TestSnippet**. (**File** > **New** > **Project** > **Visual C# (or Visual Basic)** > **Extensibility**.)

2. In the **TestSnippet** project, add a new XML file and call it *VBCodeSnippet.snippet*. Replace the content with the following XML:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <CodeSnippets
        xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
      <CodeSnippet Format="1.0.0">
        <Header>
          <Title>Hello World VB</Title>
          <Shortcut>HelloWorld</Shortcut>
          <Description>Inserts code</Description>
          <Author>MSIT</Author>
          <SnippetTypes>
            <SnippetType>Expansion</SnippetType>
            <SnippetType>SurroundsWith</SnippetType>
          </SnippetTypes>
        </Header>
        <Snippet>
          <Code Language="VB">
            <![CDATA[Console.WriteLine("Hello, World!")]]>
          </Code>
        </Snippet>
      </CodeSnippet>
    </CodeSnippets>
    ```

### Set up the directory structure

1. In **Solution Explorer**, select the project node and add a folder that has the name you want the snippet to have in **Code Snippets Manager**. In this case, it should be **HelloWorldVB**.

2. Move the *.snippet* file to the *HelloWorldVB* folder.

3. Select the *.snippet* file in **Solution Explorer**, and in the **Properties** window make sure **Build Action** is set to **Content**, **Copy to Output Directory** is set to **Copy always**, and **Include in VSIX** is set to **true**.

### Add the .pkgdef file

1. Add a text file to the *HelloWorldVB* folder and name it *HelloWorldVB.pkgdef*. This file is used to add certain keys to the registry. In this case, it adds a new subkey to the **HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\15.0\Languages\CodeExpansions\Basic** key.

2. Add the following lines to the file.

    ```txt
    // Visual Basic
    [$RootKey$\Languages\CodeExpansions\Basic\Paths]
    "HelloWorldVB"="$PackageFolder$"
    ```

    If you examine this key, you can see how to specify different languages.

3. Select the *.pkgdef* file in **Solution Explorer**, and in the **Properties** window make sure that:

   - **Build Action** is set to **Content**
   - **Copy to Output Directory** is set to **Copy always**
   - **Include in VSIX** is set to **true**

4. Add the *.pkgdef* file as an asset in the VSIX manifest. In the *source.extension.vsixmanifest* file, go to the **Assets** tab and click **New**.

5. In the **Add New Asset** dialog, set the **Type** to **Microsoft.VisualStudio.VsPackage**, the **Source** to **File on filesystem**, and the **Path** to **HelloWorldVB.pkgdef** (which should appear in the dropdown).

### Test the snippet

1. Now you can make sure that the code snippet works in the experimental instance of Visual Studio. The experimental instance is a second copy of Visual Studio that is separate from the one you use to write code. It allows you to work on an extension without affecting your development environment.

2. Build the project and start debugging.

   A second instance of Visual Studio appears.

3. In the experimental instance, go to **Tools** > **Code Snippets Manager** and set the **Language** to **Basic**. You should see *HelloWorldVB* as one of the folders, and you should be able to expand the folder to see the *HelloWorldVB* snippet.

4. Test the snippet. In the experimental instance, open a Visual Basic project and open one of the code files. Place your cursor somewhere in the code, right-click, and on the context menu select **Insert Snippet**.

5. You should see *HelloWorldVB* as one of the folders. Double-click it. You should see a pop-up **Insert Snippet: HelloWorldVB >** that has a dropdown **HelloWorldVB**. Click the **HelloWorldVB** dropdown. You should see the following line added to the file:

    ```vb
    Console.WriteLine("Hello, World!")
    ```

## See also

- [Code snippets](../ide/code-snippets.md)