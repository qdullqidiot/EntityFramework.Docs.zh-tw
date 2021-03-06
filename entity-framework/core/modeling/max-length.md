---
title: 最大長度為 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996187"
---
# <a name="maximum-length"></a>最大長度

設定最大長度，提供資料存放區，指定屬性来使用適當的資料類型的相關提示。 最大長度只適用於陣列資料類型，例如`string`和`byte[]`。

> [!NOTE]  
> Entity Framework 不會執行任何驗證的最大長度，然後將資料傳遞給提供者。 這是由提供者或資料存放區，以驗證是否適當。 比方說，當目標為 SQL Server，超出最大長度時，會導致例外狀況為基礎的資料行的資料類型將不允許超過儲存的資料。

## <a name="conventions"></a>慣例

依照慣例，再由資料庫提供者來選擇適當的資料類型的屬性。 具有長度屬性，如資料庫提供者通常會選擇可讓資料的最長長度的資料類型。 例如，將會使用 Microsoft SQL Server `nvarchar(max)` for`string`屬性 (或`nvarchar(450)`如果資料行來做為金鑰)。

## <a name="data-annotations"></a>資料註釋

若要設定屬性的最大長度，您可以使用資料註解。 在此範例中，目標 SQL Server，這會導致`nvarchar(500)`所使用的資料類型。

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

您可以使用 Fluent API 來設定屬性的最大長度。 在此範例中，目標 SQL Server，這會導致`nvarchar(500)`所使用的資料類型。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
