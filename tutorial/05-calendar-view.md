---
ms.openlocfilehash: 33bdb86a4a4af997c8ca522e23ec2f883fdeda88
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584564"
---
<!-- markdownlint-disable MD002 MD041 -->

在本节中，您将进一步将 Microsoft Graph 并入应用程序中，以获取当前周的用户日历视图。

## <a name="get-a-calendar-view"></a>获取日历视图

1. 在 " **/Pages** " 目录中创建一个名为 " **Calendar** " 的新文件，并添加以下代码。

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

1. 在节内添加以下代码 `@code{}` 。

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="GetEventsSnippet":::

    请考虑此代码执行的操作。

    - 它从添加到用户的自定义声明中获取当前用户的时区、日期格式和时间格式。
    - 该示例计算用户的首选时区中的当前星期的开始时间和结束时间。
    - 它从 Microsoft Graph 获取当前星期的日历视图。
        - 它包括 `Prefer: outlook.timezone` 用于使 Microsoft Graph `start` `end` 在指定时区中返回和属性的标头。
        - 它用于 `Top(50)` 请求响应中的最高为50个事件。
        - 它用于 `Select(u => new {})` 仅请求应用程序使用的属性。
        - 它用于 `OrderBy("start/dateTime")` 按开始时间对结果进行排序。

1. 保存全部更改并重新启动该应用程序。 选择 " **日历** " 导航项。 应用程序显示从 Microsoft Graph 返回的事件的 JSON 表示形式。

## <a name="display-the-results"></a>显示结果

现在，您可以将 JSON 转储替换为用户更友好的内容。

1. 将以下函数添加到 `@code{}` 节中。

    :::code language="csharp" source="../demo/GraphTutorial/Pages/Calendar.razor" id="FormatDateSnippet":::

    此代码采用 ISO 8601 日期字符串，并将其转换为用户的首选日期和时间格式。

1. 将 `<code>` 元素内的元素替换 `<Authorized>` 为以下元素。

    :::code language="razor" source="../demo/GraphTutorial/Pages/Calendar.razor" id="CalendarViewSnippet":::

    这将创建一个由 Microsoft Graph 返回的事件表。

1. 保存更改并重新启动该应用。 现在，" **日历** " 页面将呈现一个事件表。

    ![显示事件表的应用程序的屏幕截图](images/calendar-view.png)
