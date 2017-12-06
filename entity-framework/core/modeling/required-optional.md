---
title: "必要/選用的屬性-EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="aab4e-102">必要和選擇性的屬性</span><span class="sxs-lookup"><span data-stu-id="aab4e-102">Required and Optional Properties</span></span>

<span data-ttu-id="aab4e-103">屬性會被視為選擇性是否有效，才能包含`null`。</span><span class="sxs-lookup"><span data-stu-id="aab4e-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="aab4e-104">如果`null`不是有效的值，以指派給屬性，則它會被視為必要的屬性。</span><span class="sxs-lookup"><span data-stu-id="aab4e-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="aab4e-105">慣例</span><span class="sxs-lookup"><span data-stu-id="aab4e-105">Conventions</span></span>

<span data-ttu-id="aab4e-106">依照慣例，其 CLR 類型可以包含 null 的屬性會設定為選擇性 (`string`， `int?`，`byte[]`等。)。</span><span class="sxs-lookup"><span data-stu-id="aab4e-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="aab4e-107">將設定其 CLR 類型不能包含 null 的屬性視需要 (`int`， `decimal`，`bool`等。)。</span><span class="sxs-lookup"><span data-stu-id="aab4e-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="aab4e-108">CLR 類型不能包含 null 的屬性無法設定為選擇性。</span><span class="sxs-lookup"><span data-stu-id="aab4e-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="aab4e-109">此屬性將一律視為 Entity Framework 所需。</span><span class="sxs-lookup"><span data-stu-id="aab4e-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="aab4e-110">資料註釋</span><span class="sxs-lookup"><span data-stu-id="aab4e-110">Data Annotations</span></span>

<span data-ttu-id="aab4e-111">您可以使用資料註解表示是必要屬性。</span><span class="sxs-lookup"><span data-stu-id="aab4e-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="aab4e-112">關於 fluent 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="aab4e-112">Fluent API</span></span>

<span data-ttu-id="aab4e-113">您可以使用 fluent 應用程式開發的應用程式開發介面，表示是必要屬性。</span><span class="sxs-lookup"><span data-stu-id="aab4e-113">You can use the Fluent API to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```