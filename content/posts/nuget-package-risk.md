---
slug: nuget-package-risk
title: "How This NuGet Package Almost Cost Me My Job"
description: "Discover the shocking truth about how a seemingly innocent NuGet package nearly derailed my career. Learn from my experience and avoid the pitfalls. Read now!"
date: 2023-07-15T20:24:57+01:00
draft: false
cover:
    image: img/almost-got-sacked.webp
    alt: 'How This Nuget Package Almost Cost Me My Job'
    caption: 'Almost Got Sacked | Photo by Andrea Piacquadio'
categories: [".NET Core"]
---

In the world of .NET development, NuGet packages have become an essential part of building applications. These are pre-built libraries that offer convenience and efficiency, enabling .NET developers to save time and effort by leveraging existing code. However, my personal experience with a particular NuGet package taught me a valuable lesson about the importance of thorough evaluation and careful implementation. In this blog post, I will share the story of how a seemingly harmless NuGet package nearly jeopardized my job and discuss the key takeaways and best practices to prevent similar pitfalls.


### Benefits of Z.BulkOperations
When I first encountered the Z.BulkOperations NuGet package, I was immediately drawn to its promising features and potential benefits for my project. This package specializes in efficient bulk data operations, offering a streamlined approach to handling large datasets within a database. With the package's reputation for improving performance and simplifying database operations, it seemed like the perfect solution to tackle the specific challenge I was facing in my project.

![Benefits of Z.BulkOperations](/img/benefits-of-z-bulkOperations.webp)

- Enhanced Performance:
One of the key attractions of the Z.BulkOperations package was its ability to significantly enhance performance when dealing with large volumes of data. The package leverages optimized bulk operations, enabling faster data processing and database interactions. This efficiency was particularly appealing as it promised to address performance bottlenecks that I had encountered in my project.

- Simplified Database Operations:
Another compelling feature of Z.BulkOperations was its ability to simplify complex database operations. The package provided a high-level abstraction layer that allowed for seamless bulk inserts, updates, and deletes. This abstraction meant that I could streamline my code and reduce the number of database round trips, resulting in improved efficiency and code readability.

- Positive Reputation:
The reputation of the Z.BulkOperations package within the developer community further heightened its appeal. It had garnered positive reviews and testimonials from developers who praised its performance and ease of use. The package's popularity and active community support gave me confidence that it was a reliable and well-maintained solution.

- Addressing Project Challenges:
In my project, I was tasked with processing and manipulating a large dataset within a tight timeframe. The traditional approach of performing individual database operations for each record proved to be time-consuming and resource-intensive. This is where Z.BulkOperations appeared to offer an efficient alternative, enabling me to handle the dataset more effectively and meet project deadlines.

However, as I would soon discover, the allure of the Z.BulkOperations package came with unexpected challenges and implications. Despite its attractive features, there were crucial factors that I overlooked during the initial evaluation, leading to severe consequences that nearly cost me my job. In the following sections, I will delve into these challenges and share the valuable lessons I learned from this experience.


### The Con of Z.BulkOperations

On one faithful day, I resumed work as usual, and as a routine, I decided to go through my emails, only to see a mail with a screenshot of the error message that the frontend part of my application was throwing. The error message I was greeted with was "Something went wrong, please try again". I was thrown aback because this error should not be happening.

So I tried reproducing the error on my local machine but I didn't see anything, the particular action the user was getting an error for on production was going successfully on my local machine.

I had to look at the log file to get a sense of what was happening. Below is the image of what the error message was
![Error Log](/img/error-log.webp)

I was surprised and at the same time, I felt relieved. I asked myself, how come I didn't see this?, it was obvious, I didn't thoroughly read the documentation.

When I saw this NuGet Package online, I was only interested in knowing how to implement it and solve the particular challenge I had.

### What Was The Solution?

The options before me were to:
- Pay for the NuGet package
- Make use of `AddRangeAsync`
- Make use of `SqlBulkCopy`

Since I was not willing to pay $300 for this NuGet extension or package, I decided to uninstall it, so I was left with options 2 and 3. I decided to use `SqlBulkCopy` which is the third option.

### Why Use SqlBulkCopy
The reason I didn't go for the second option was that if for example, I want to insert 100 rows into a table, I'll have to hit the database 100 times, performance-wise, this is a deal breaker. So with `SqlBulkCopy`, I'll only have to hit the database only once regardless of the number of rows I'll need to insert.

```C#
public async Task<Response<IEnumerable<long>>> CreateAsync(TimerDto request)
{
    var timers = _mapper.Map<IEnumerable<Timer>>(request.Timers).ToList();

    using (var dataTable = new DataTable())
    {
        dataTable.Columns.Add("Id", typeof(long));
        dataTable.Columns.Add("Name", typeof(string));
        dataTable.Columns.Add("Start", typeof(string));
        dataTable.Columns.Add("End", typeof(string));
    
        foreach (var timer in timers)
        {
            dataTable.Rows.Add(
                timer.Id,
                timer.Name,
                timer.Start,
                timer.End
            );
        }
    
        using (var sqlBulkCopy = new SqlBulkCopy(_context.Database.GetDbConnection().ConnectionString))
        {
            sqlBulkCopy.DestinationTableName = "[codaholic].[dbo].[Timer]";
            await sqlBulkCopy.WriteToServerAsync(dataTable);
        }
    }
    
    var timerIds = timers.Select(c => c.Id);
    return new Response<IEnumerable<long>>(timerIds);
}
```


Here is a simple explanation of what the code above does, a `DataTable` is created to hold the timer data. Columns are added to the `DataTable` to match the properties of the Timer entity, such as `Id`, `Name`, `Start`, and `End`.

The `foreach` loop iterates over the timers list and adds each timer's data as a new row in the `DataTable`. Each timer's properties are accessed and added to the corresponding columns in the `DataTable`.
SqlBulkCopy:

A `SqlBulkCopy` object is created, using the connection string from the _context object's database connection. This object facilitates the bulk insert operation into the database.

The `DestinationTableName` property is set to specify the destination table where the data should be inserted.
The `WriteToServerAsync` method is called to perform the bulk insert operation asynchronously, using the `DataTable` as the source of the data.

### Conclusion

By sharing my cautionary tale, I hope to raise awareness among developers about the potential risks of blindly adopting NuGet packages. Through careful evaluation, taking time to read the documentation, and not just getting a bunch of code from StackOverflow or ChatGPT, we can mitigate the chances of encountering similar issues and ensure a more stable software development process.

<br/>

{{< bmc-button slug="codaholic" >}}

<br/>




