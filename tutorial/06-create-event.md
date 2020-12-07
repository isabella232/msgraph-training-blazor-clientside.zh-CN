---
ms.openlocfilehash: 723f1d08b9d1085d47d0d5fac71da5371badc3d0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584541"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5b6dc-101">在本节中，您将添加在用户日历上创建事件的功能。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="5b6dc-102">在 **/Pages** 目录中创建一个名为 **NewEvent** 的新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-102">Create a new file in the **./Pages** directory named **NewEvent.razor** and add the following code.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventFormSnippet":::

    <span data-ttu-id="5b6dc-103">此操作将向页面中添加一个表单，以便用户可以为新事件输入值。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-103">This adds a form to the page so the user can enter values for the new event.</span></span>

1. <span data-ttu-id="5b6dc-104">将以下代码添加到文件末尾。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-104">Add the following code to the end of the file.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventCodeSnippet":::

    <span data-ttu-id="5b6dc-105">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-105">Consider what this code does.</span></span>

    - <span data-ttu-id="5b6dc-106">在 `OnInitializedAsync` 该示例中，获取经过身份验证的用户的时区。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-106">In `OnInitializedAsync` it gets the authenticated user's time zone.</span></span>
    - <span data-ttu-id="5b6dc-107">在 `CreateEvent` 该示例中，将使用表单中的值初始化新的 **Event** 对象。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-107">In `CreateEvent` it initializes a new **Event** object using the values from the form.</span></span>
    - <span data-ttu-id="5b6dc-108">它使用 Graph SDK 将事件添加到用户的日历中。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-108">It uses the Graph SDK to add the event to the user's calendar.</span></span>

1. <span data-ttu-id="5b6dc-109">保存全部更改并重新启动该应用程序。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-109">Save all of your changes and restart the app.</span></span> <span data-ttu-id="5b6dc-110">在 " **日历** " 页上，选择 " **新建事件**"。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-110">On the **Calendar** page, select **New event**.</span></span> <span data-ttu-id="5b6dc-111">填写窗体，然后选择 " **创建**"。</span><span class="sxs-lookup"><span data-stu-id="5b6dc-111">Fill in the form and choose **Create**.</span></span>

    ![新事件表单的屏幕截图](images/create-event.png)
