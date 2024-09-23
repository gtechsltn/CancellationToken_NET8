# Mastering CancellationToken in .NET 8: A Must-Have Skill for Modern Developers

Mastering CancellationToken in .NET 8: A Must-Have Skill for Modern Developers

https://medium.com/@michaelmaurice410/mastering-cancellationtoken-in-net-8-a-must-have-skill-for-modern-developers-f3594f151054

+ Graceful Cancellation: CancellationToken enables you to cancel long-running operations in a controlled manner, preventing resource wastage.
+ Responsiveness: Incorporating cancellation in your tasks makes your applications more responsive and user-friendly.
+ Error Handling: Always handle OperationCanceledException to ensure your application can manage cancellations without crashing.

```
dotnet new console -n CancellationTokenExample
cd CancellationTokenExample
```

### Program.cs
```
using System;
using System.Threading;
using System.Threading.Tasks;
class Program
{
    static async Task Main(string[] args)
    {
        using var cancellationTokenSource = new CancellationTokenSource();
        Console.WriteLine("Press 'c' to cancel the operation.\n");
        var task = LongRunningOperationAsync(cancellationTokenSource.Token);
        // Listen for user input to cancel the operation
        Task.Run(() =>
        {
            if (Console.ReadKey().KeyChar == 'c')
            {
                cancellationTokenSource.Cancel();
                Console.WriteLine("\nCancellation requested.");
            }
        });
        try
        {
            await task;
            Console.WriteLine("Operation completed successfully.");
        }
        catch (OperationCanceledException)
        {
            Console.WriteLine("Operation was canceled.");
        }
    }
    static async Task LongRunningOperationAsync(CancellationToken cancellationToken)
    {
        Console.WriteLine("Operation started. Press 'c' to cancel.\n");
        for (int i = 0; i < 10; i++)
        {
            // Simulate work by delaying
            await Task.Delay(1000);
            // Check if cancellation was requested
            if (cancellationToken.IsCancellationRequested)
            {
                Console.WriteLine("Cancellation detected, stopping the operation...");
                cancellationToken.ThrowIfCancellationRequested();
            }
            Console.WriteLine($"Operation step {i + 1}/10 completed.");
        }
    }
}
```
