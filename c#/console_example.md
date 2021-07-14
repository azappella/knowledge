# Example Console Application C#

```C#
using System;
using System.Runtime.InteropServices;
using Illustrator;
using System.IO;

namespace ConsoleIllustrator
{

    class Program
    {
        static void Main(string[] args)
        {
            // new Illustrator.Application();
            Console.WriteLine("Starting now!");
            Program.StartIllustratorApplication();
        }

        static void StartIllustratorApplication()
        {
            Illustrator.Document doc = null;
            Illustrator.Application app = null;
            String inputFile = @"D:\temp\input1.eps";
            String image = @"D:\temp\file2.png";

            try
            {
                // open AI, init
                app = new Illustrator.Application();
                // open document
                // doc = app.Open(inputFile, Illustrator.AiDocumentColorSpace.aiDocumentRGBColor, null);
                doc = app.Documents.Add();
                app.ActiveDocument.Activate();
                
                if (File.Exists(image))
                {
                    // insert image
                    int cnt = doc.PlacedItems.Count;
                    PlacedItem img = doc.PlacedItems.Add();
                    img.File = image;
                    img.Embed();
                    img.Top = 0;
                    img.Left = 0;
                    img.Height = 5.4;
                    img.Width = 5.4;
                }

                TextFrame sampleText = doc.TextFrames.Add();
                // int[,] position = new int[200, 200];
                // int[] pos = { 200, 201 };
                sampleText.Top = 150;
                sampleText.Left = 150;
                sampleText.Contents = "Hello World";

                Illustrator.Document currentDoc = app.ActiveDocument;

                if (app.Documents.Count > 0)
                {
                    String output = @"D:\temp\save2";
                    doc.SaveAs(output);

                }


            }
            catch (COMException ce)
            {
                string msg = ce.StackTrace;
                string errorCode = String.Format("0x{0:X}", ce.HResult);
                Console.WriteLine("Error: " + errorCode);
            }
            finally
            {
                Console.WriteLine("Quitting now!");
                doc.Close(AiSaveOptions.aiDoNotSaveChanges);
                app.Quit();

            }
        }
    }
}
```
