# Sample for RestableCmdlets

## Setup 
You need the ClrPlus Dynamic Powershell support. You probably just want to use the
monolithic build (which has all of the dependencies rolled up inside it)

From nuget just install the ClrPlus.Powershell package (referenced in this project already)

> Note: Make sure that port 80 is opened up in your firewall (or whatever port you're listing on)

## Usage

The only thing you have to do is to create an instance of the DynamicPowershell class by invoking `.Dynamic()` on a `Runspace` or `RunspacePool` :

#### Program.cs 

``` csharp

namespace DynamicPowershellSample {
    using System;
    using System.Management.Automation.Runspaces;
    using ClrPlus.Powershell.Core;

    class Program {
        static void Main(string[] args) {

            // always encapsulate the DynamicPowershell object in a using (or dispose it when you're done)
           using( var powershell = Runspace.DefaultRunspace.Dynamic() ) {
               // you can just use Runspace.DefaultRunspace, if there isn't one, 
               // it will manage that for you.

               // call any cmdlet or alias, just take the dash out 
               // so that get-children would be getchildren

               // pass named parameters as named parameters, you can even use unnamed parameters first, like in powershell:
               var dirResults = powershell.dir(@"c:\windows", recurse: false);

               // every cmdlet or alias will return an IEnumerable<object> 
               // that is still being added to, but you can start enumerating it right away

               foreach (var entry in dirResults) // it won't mind :D 
               {
                    Console.WriteLine(entry);
               }

               var items = powershell.dir(@"c:\program files", recurse:false);

               //you can force it to wait for the results if you want:
               items.Wait();

                foreach (var i in items) {
                    Console.WriteLine(i);
                }

            }

            Console.WriteLine("\r\n\r\n======Press enter to exit");
            Console.ReadLine();
        
        }
    }
}

```

That's pretty much it.

