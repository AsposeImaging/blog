---
title: Introduction to Aspose.CAD, Part 2
layout: post
tags: csharp, cad, convert, export
---


If you're willing to use full version of <a href="https://products.aspose.com/cad/">Aspose.CAD</a> API, you'll need to apply your license in your software that uses Aspose.CAD library. In this article I'll show how to do this. It's simple!

There are two types of licenses, one is a traditional license file that is paid upfront, and another is a pay-per-use service key license, called Metered license. Let's dive a little deeper.

##License file
This one is very simple to implement. Just include these lines in your project somewhere:
````csharp
            License license = new Aspose.CAD.License();
			// Pass path to license file
            license.SetLicense("Aspose.CAD.lic");
```
After execution of these lines your Aspose.CAD libary will work in licensed mode, no file complexity limit, no watermark. As such, you'd probably want to put these lines somewhere within your software's startup code, before any other use of Aspose.CAD. <a href="https://apireference.aspose.com/net/cad/aspose.cad/license/methods/setlicense/index">SetLicense</a> also accepts <a href="https://msdn.microsoft.com/en-us/library/system.io.filestream(v=vs.110).aspx">FileStream</a>, so you can perform IO in code that's separate from license setup.

##Metered license
This is a license that measures amount of use of the Aspose.CAD libary, and syncs with Aspose licensing service to perform billing on per-use basis. It also lets you measure the registered amount of use. Everything is done via a static <a href="https://apireference.aspose.com/net/cad/aspose.cad/metered">Metered</a> class Take a look:
````csharp
            // Set public and private keys in Metered class
            Aspose.CAD.Metered.SetMeteredKey("*****", "*****");

            // Get metered data amount before calling API
            decimal amountbefore = Aspose.CAD.Metered.GetConsumptionQuantity();

            // Display information
            Console.WriteLine("Amount Consumed Before: " + amountbefore.ToString());
            
            
            // Do processing
            //Aspose.CAD.FileFormats.Cad.CadImage image = (Aspose.CAD.FileFormats.Cad.CadImage)Aspose.CAD.Image.load("BlockRefDgn.dwg");

            
            // Get metered data amount After calling API
            decimal amountafter = Aspose.CAD.Metered.GetConsumptionQuantity();

            // Display information
            Console.WriteLine("Amount Consumed After: " + amountafter.ToString());
```
As you see, it's equally simple to use.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
