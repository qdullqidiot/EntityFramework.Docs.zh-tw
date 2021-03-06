---
title: 替代索引鍵的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996967"
---
# <a name="alternate-keys"></a>替代索引鍵

替代索引鍵做為替代的唯一識別項，除了的主索引鍵的每個實體執行個體。 替代索引鍵可用來當做目標的關聯性。 使用關聯式資料庫時這會對應至唯一索引/條件約束的替代索引鍵資料行和一個或多個 foreign key 條件約束參考資料行上的概念。

> [!TIP]  
> 如果您只想要強制執行唯一性的資料行，則您需要唯一的索引，而不是替代索引鍵，請參閱[索引](indexes.md)。 在 EF 中，替代索引鍵可提供更強大的功能比唯一索引，因為它們可以用做為外部索引鍵的目標。

替代索引鍵通常會導入您需要時，您不需要手動加以設定。 請參閱[慣例](#conventions)如需詳細資訊。

## <a name="conventions"></a>慣例

依照慣例，替代索引鍵時引進了讓您識別屬性，做為關聯性的目標不是主索引鍵。

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>資料註釋

替代索引鍵不可以使用資料註解來設定。

## <a name="fluent-api"></a>Fluent API

您可以使用 Fluent API 來設定為替代索引鍵的單一屬性。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

您也可以使用 Fluent API 來設定多個屬性 （又稱為複合的替代索引鍵） 為替代索引鍵。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
