---
ms.openlocfilehash: 723f1d08b9d1085d47d0d5fac71da5371badc3d0
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584541"
---
<!-- markdownlint-disable MD002 MD041 -->

在本节中，您将添加在用户日历上创建事件的功能。

1. 在 **/Pages** 目录中创建一个名为 **NewEvent** 的新文件，并添加以下代码。

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventFormSnippet":::

    此操作将向页面中添加一个表单，以便用户可以为新事件输入值。

1. 将以下代码添加到文件末尾。

    :::code language="razor" source="../demo/GraphTutorial/Pages/NewEvent.razor" id="NewEventCodeSnippet":::

    请考虑此代码执行的操作。

    - 在 `OnInitializedAsync` 该示例中，获取经过身份验证的用户的时区。
    - 在 `CreateEvent` 该示例中，将使用表单中的值初始化新的 **Event** 对象。
    - 它使用 Graph SDK 将事件添加到用户的日历中。

1. 保存全部更改并重新启动该应用程序。 在 " **日历** " 页上，选择 " **新建事件**"。 填写窗体，然后选择 " **创建**"。

    ![新事件表单的屏幕截图](images/create-event.png)
