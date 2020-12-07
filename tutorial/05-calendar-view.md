---
ms.openlocfilehash: 33bdb86a4a4af997c8ca522e23ec2f883fdeda88
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584564"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a7a26-101">在本节中，您将进一步将 Microsoft Graph 并入应用程序中，以获取当前周的用户日历视图。</span><span class="sxs-lookup"><span data-stu-id="a7a26-101">In this section you will further incorporate Microsoft Graph into the application to get a view of the user's calendar for the current week.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="a7a26-102">获取日历视图</span><span class="sxs-lookup"><span data-stu-id="a7a26-102">Get a calendar view</span></span>

1. <span data-ttu-id="a7a26-103">在 " **/Pages** " 目录中创建一个名为 " **Calendar** " 的新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="a7a26-103">Create a new file in the **./Pages** directory named **Calendar.razor** and add the following code.</span></span>

    ```razor
    @page "/calendar"
    @using Microsoft.Graph
    @using TimeZoneConverter

    @inject GraphTutorial.Graph.GraphClientFactory clientFactory

    <AuthorizeView>
        <Authorized>
            <!-- Temporary JSON dump of events -->
            <code>@graphClient.HttpProvider.Serializer.SerializeObject(events)</code>
        </Authorized>
        <NotAuthorized>
            <RedirectToLogin />
        </NotAuthorized>
    </AuthorizeView>

    @code{
        [CascadingParameter]
        private Task<AuthenticationState> authenticationStateTask { get; set; }

        private GraphServiceClient graphClient;
        private IList<Event> events = new List<Event>();
        private string dateTimeFormat;
    }
    ```

1. <span data-ttu-id="a7a26-104">在节内添加以下代码 `@code{}` 。</span><span class="sxs-lookup"><span data-stu-id="a7a26-104">Add the following code inside the `@code{}` section.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="GetEventsSnippet":::

    <span data-ttu-id="a7a26-105">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="a7a26-105">Consider what this code does.</span></span>

    - <span data-ttu-id="a7a26-106">它从添加到用户的自定义声明中获取当前用户的时区、日期格式和时间格式。</span><span class="sxs-lookup"><span data-stu-id="a7a26-106">It gets the current user's time zone, date format, and time format from the custom claims added to the user.</span></span>
    - <span data-ttu-id="a7a26-107">该示例计算用户的首选时区中的当前星期的开始时间和结束时间。</span><span class="sxs-lookup"><span data-stu-id="a7a26-107">It calculates the start and end of the current week in the user's preferred time zone.</span></span>
    - <span data-ttu-id="a7a26-108">它从 Microsoft Graph 获取当前星期的日历视图。</span><span class="sxs-lookup"><span data-stu-id="a7a26-108">It gets a calendar view from Microsoft Graph for the current week.</span></span>
        - <span data-ttu-id="a7a26-109">它包括 `Prefer: outlook.timezone` 用于使 Microsoft Graph `start` `end` 在指定时区中返回和属性的标头。</span><span class="sxs-lookup"><span data-stu-id="a7a26-109">It includes the `Prefer: outlook.timezone` header to cause Microsoft Graph to return the `start` and `end` properties in the specified time zone.</span></span>
        - <span data-ttu-id="a7a26-110">它用于 `Top(50)` 请求响应中的最高为50个事件。</span><span class="sxs-lookup"><span data-stu-id="a7a26-110">It uses `Top(50)` to request up to 50 events in the response.</span></span>
        - <span data-ttu-id="a7a26-111">它用于 `Select(u => new {})` 仅请求应用程序使用的属性。</span><span class="sxs-lookup"><span data-stu-id="a7a26-111">It uses `Select(u => new {})` to request just the properties used by the app.</span></span>
        - <span data-ttu-id="a7a26-112">它用于 `OrderBy("start/dateTime")` 按开始时间对结果进行排序。</span><span class="sxs-lookup"><span data-stu-id="a7a26-112">It uses `OrderBy("start/dateTime")` to sort the results by start time.</span></span>

1. <span data-ttu-id="a7a26-113">保存全部更改并重新启动该应用程序。</span><span class="sxs-lookup"><span data-stu-id="a7a26-113">Save all of your changes and restart the app.</span></span> <span data-ttu-id="a7a26-114">选择 " **日历** " 导航项。</span><span class="sxs-lookup"><span data-stu-id="a7a26-114">Choose the **Calendar** nav item.</span></span> <span data-ttu-id="a7a26-115">应用程序显示从 Microsoft Graph 返回的事件的 JSON 表示形式。</span><span class="sxs-lookup"><span data-stu-id="a7a26-115">The app displays a JSON representation of the events returned from Microsoft Graph.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="a7a26-116">显示结果</span><span class="sxs-lookup"><span data-stu-id="a7a26-116">Display the results</span></span>

<span data-ttu-id="a7a26-117">现在，您可以将 JSON 转储替换为用户更友好的内容。</span><span class="sxs-lookup"><span data-stu-id="a7a26-117">Now you can replace the JSON dump with something more user-friendly.</span></span>

1. <span data-ttu-id="a7a26-118">将以下函数添加到 `@code{}` 节中。</span><span class="sxs-lookup"><span data-stu-id="a7a26-118">Add the following function inside the `@code{}` section.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="FormatDateSnippet":::

    <span data-ttu-id="a7a26-119">此代码采用 ISO 8601 日期字符串，并将其转换为用户的首选日期和时间格式。</span><span class="sxs-lookup"><span data-stu-id="a7a26-119">This code takes an ISO 8601 date string and converts it into the user's preferred date and time format.</span></span>

1. <span data-ttu-id="a7a26-120">将 `<code>` 元素内的元素替换 `<Authorized>` 为以下元素。</span><span class="sxs-lookup"><span data-stu-id="a7a26-120">Replace the `<code>` element inside the `<Authorized>` element with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Calendar.razor" id="CalendarViewSnippet":::

    <span data-ttu-id="a7a26-121">这将创建一个由 Microsoft Graph 返回的事件表。</span><span class="sxs-lookup"><span data-stu-id="a7a26-121">This creates a table of the events returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="a7a26-122">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="a7a26-122">Save your changes and restart the app.</span></span> <span data-ttu-id="a7a26-123">现在，" **日历** " 页面将呈现一个事件表。</span><span class="sxs-lookup"><span data-stu-id="a7a26-123">Now the **Calendar** page renders a table of events.</span></span>

    ![显示事件表的应用程序的屏幕截图](images/calendar-view.png)
