---
ms.openlocfilehash: f98548a1332417d92b126a2cb7499fea5306b34f
ms.sourcegitcommit: 5067c508675fbedbc7eead0869308d00b63be8e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "49584546"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="951bc-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="951bc-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="951bc-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph API，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="951bc-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph API.</span></span>

1. <span data-ttu-id="951bc-103">打开 **/wwwroot/appsettings.js**。</span><span class="sxs-lookup"><span data-stu-id="951bc-103">Open **./wwwroot/appsettings.json**.</span></span> <span data-ttu-id="951bc-104">添加 `GraphScopes` 属性并更新 `Authority` 和 `ClientId` 值以匹配以下项。</span><span class="sxs-lookup"><span data-stu-id="951bc-104">Add a `GraphScopes` property and update the `Authority` and `ClientId` values to match the following.</span></span>

    :::code language="json" source="../demo/GraphTutorial/wwwroot/appsettings.example.json" highlight="3-4,7":::

    <span data-ttu-id="951bc-105">`YOUR_APP_ID_HERE`将替换为应用注册中的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="951bc-105">Replace `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="951bc-106">如果您使用的是源代码管理（如 git），现在可以从源代码管理中排除文件的 **appsettings.js** ，以避免无意中泄漏您的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="951bc-106">If you're using source control such as git, now would be a good time to exclude the **appsettings.json** file from source control to avoid inadvertently leaking your app ID.</span></span>

    <span data-ttu-id="951bc-107">查看值中包含的作用域 `GraphScopes` 。</span><span class="sxs-lookup"><span data-stu-id="951bc-107">Review the scopes included in the `GraphScopes` value.</span></span>

    - <span data-ttu-id="951bc-108">**用户。 Read** 允许应用程序获取用户的配置文件和照片。</span><span class="sxs-lookup"><span data-stu-id="951bc-108">**User.Read** allows the application to get the user's profile and photo.</span></span>
    - <span data-ttu-id="951bc-109">**MailboxSettings** 允许应用程序获取邮箱设置，其中包括用户的首选时区。</span><span class="sxs-lookup"><span data-stu-id="951bc-109">**MailboxSettings.Read** allows the application to get mailbox settings, which includes the user's preferred time zone.</span></span>
    - <span data-ttu-id="951bc-110">**ReadWrite** 允许应用程序读取和写入用户日历。</span><span class="sxs-lookup"><span data-stu-id="951bc-110">**Calendars.ReadWrite** allows the application to read and write to the user's calendar.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="951bc-111">实施登录</span><span class="sxs-lookup"><span data-stu-id="951bc-111">Implement sign-in</span></span>

<span data-ttu-id="951bc-112">此时，.NET Core 项目模板添加了用于启用登录的代码。</span><span class="sxs-lookup"><span data-stu-id="951bc-112">At this point the .NET Core project template has added the code to enable sign in.</span></span> <span data-ttu-id="951bc-113">但是，在本节中，您将添加其他代码，以通过将 Microsoft Graph 中的信息添加到用户的标识来改进体验。</span><span class="sxs-lookup"><span data-stu-id="951bc-113">However, in this section you'll add additional code to improve the experience by adding information from Microsoft Graph to the user's identity.</span></span>

1. <span data-ttu-id="951bc-114">打开 **/Pages/Authentication.razor** ，并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="951bc-114">Open **./Pages/Authentication.razor** and replace its contents with the following.</span></span>

    :::code language="razor" source="../demo/GraphTutorial/Pages/Authentication.razor" id="AuthenticationSnippet":::

    <span data-ttu-id="951bc-115">这将在登录失败时替换默认错误消息，以显示身份验证过程返回的任何错误消息。</span><span class="sxs-lookup"><span data-stu-id="951bc-115">This replaces the default error message when login fails to display any error message returned by the authentication process.</span></span>

1. <span data-ttu-id="951bc-116">在名为 **Graph** 的项目的根目录中创建一个新目录。</span><span class="sxs-lookup"><span data-stu-id="951bc-116">Create a new directory in the root of the project named **Graph**.</span></span>

1. <span data-ttu-id="951bc-117">在名为 **GraphUserAccountFactory.cs** 的 **/Graph** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="951bc-117">Create a new file in the **./Graph** directory named **GraphUserAccountFactory.cs** and add the following code.</span></span>

    ```csharp
    using System.Security.Claims;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;
    using Microsoft.Extensions.Logging;
    using Microsoft.Graph;

    namespace GraphTutorial.Graph
    {
        // Extends the AccountClaimsPrincipalFactory that builds
        // a user identity from the identity token.
        // This class adds additional claims to the user's ClaimPrincipal
        // that hold values from Microsoft Graph
        public class GraphUserAccountFactory
            : AccountClaimsPrincipalFactory<RemoteUserAccount>
        {
            private readonly IAccessTokenProviderAccessor accessor;
            private readonly ILogger<GraphUserAccountFactory> logger;

            public GraphUserAccountFactory(IAccessTokenProviderAccessor accessor,
                ILogger<GraphUserAccountFactory> logger)
            : base(accessor)
            {
                this.accessor = accessor;
                this.logger = logger;
            }

            public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
                RemoteUserAccount account,
                RemoteAuthenticationUserOptions options)
            {
                // Create the base user
                var initialUser = await base.CreateUserAsync(account, options);

                // If authenticated, we can call Microsoft Graph
                if (initialUser.Identity.IsAuthenticated)
                {
                    try
                    {
                        // Add additional info from Graph to the identity
                        await AddGraphInfoToClaims(accessor, initialUser);
                    }
                    catch (AccessTokenNotAvailableException exception)
                    {
                        logger.LogError($"Graph API access token failure: {exception.Message}");
                    }
                    catch (ServiceException exception)
                    {
                        logger.LogError($"Graph API error: {exception.Message}");
                        logger.LogError($"Response body: {exception.RawResponseBody}");
                    }
                }

                return initialUser;
            }

            private async Task AddGraphInfoToClaims(
                IAccessTokenProviderAccessor accessor,
                ClaimsPrincipal claimsPrincipal)
            {
                // TEMPORARY: Get the token and log it
                var result = await accessor.TokenProvider.RequestAccessToken();

                if (result.TryGetToken(out var token))
                {
                    logger.LogInformation($"Access token: {token.Value}");
                }
            }
        }
    }
    ```

    <span data-ttu-id="951bc-118">此类扩展 **AccountClaimsPrincipalFactory** 类并重写该 `CreateUserAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="951bc-118">This class extends the **AccountClaimsPrincipalFactory** class and overrides the `CreateUserAsync` method.</span></span> <span data-ttu-id="951bc-119">目前，此方法仅记录用于调试目的的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="951bc-119">For now, this method only logs the access token for debugging purposes.</span></span> <span data-ttu-id="951bc-120">在本练习的稍后部分，您将实现 Microsoft Graph 调用。</span><span class="sxs-lookup"><span data-stu-id="951bc-120">You'll implement the Microsoft Graph calls later in this exercise.</span></span>

1. <span data-ttu-id="951bc-121">打开 **./Program.cs** 并 `using` 在文件顶部添加以下语句。</span><span class="sxs-lookup"><span data-stu-id="951bc-121">Open **./Program.cs** and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
    using GraphTutorial.Graph;
    ```

1. <span data-ttu-id="951bc-122">在中 `Main` ，将现有 `builder.Services.AddMsalAuthentication` 呼叫替换为以下项。</span><span class="sxs-lookup"><span data-stu-id="951bc-122">Inside `Main`, replace the existing `builder.Services.AddMsalAuthentication` call with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddMsalAuthSnippet":::

    <span data-ttu-id="951bc-123">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="951bc-123">Consider what this code does.</span></span>

    - <span data-ttu-id="951bc-124">它 `GraphScopes` 在中加载 **appsettings.js** 的值，并将每个作用域添加到 MSAL 提供程序使用的默认范围。</span><span class="sxs-lookup"><span data-stu-id="951bc-124">It loads the value of `GraphScopes` from **appsettings.json** and adds each scope to the default scopes used by the MSAL provider.</span></span>
    - <span data-ttu-id="951bc-125">它将现有帐户工厂替换为 **GraphUserAccountFactory** 类。</span><span class="sxs-lookup"><span data-stu-id="951bc-125">It replaces the existing account factory with the **GraphUserAccountFactory** class.</span></span>

1. <span data-ttu-id="951bc-126">保存更改并重新启动该应用。</span><span class="sxs-lookup"><span data-stu-id="951bc-126">Save your changes and restart the app.</span></span> <span data-ttu-id="951bc-127">使用 " **登录** " 链接登录。</span><span class="sxs-lookup"><span data-stu-id="951bc-127">Use the **Log in** link to log in.</span></span> <span data-ttu-id="951bc-128">查看并接受请求的权限。</span><span class="sxs-lookup"><span data-stu-id="951bc-128">Review and accept the requested permissions.</span></span>

1. <span data-ttu-id="951bc-129">应用程序将使用欢迎消息进行刷新。</span><span class="sxs-lookup"><span data-stu-id="951bc-129">The app refreshes with a welcome message.</span></span> <span data-ttu-id="951bc-130">访问浏览器的开发人员工具，并查看 " **控制台** " 选项卡。应用程序会记录访问令牌。</span><span class="sxs-lookup"><span data-stu-id="951bc-130">Access your browser's developer tools and review the **Console** tab. The app logs the access token.</span></span>

    ![显示访问令牌的浏览器开发人员工具窗口的屏幕截图](images/access-token.png)

## <a name="get-user-details"></a><span data-ttu-id="951bc-132">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="951bc-132">Get user details</span></span>

<span data-ttu-id="951bc-133">用户登录后，可以从 Microsoft Graph 获取其信息。</span><span class="sxs-lookup"><span data-stu-id="951bc-133">Once the user is logged in, you can get their information from Microsoft Graph.</span></span> <span data-ttu-id="951bc-134">在本节中，你将使用 Microsoft Graph 中的信息将其他声明添加到用户的 **ClaimsPrincipal** 中。</span><span class="sxs-lookup"><span data-stu-id="951bc-134">In this section you'll use information from Microsoft Graph to add additional claims to the user's **ClaimsPrincipal**.</span></span>

1. <span data-ttu-id="951bc-135">在名为 **GraphClaimsPrincipalExtensions.cs** 的 **/Graph** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="951bc-135">Create a new file in the **./Graph** directory named **GraphClaimsPrincipalExtensions.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClaimsPrincipalExtensions.cs" id="GraphClaimsExtensionsSnippet":::

    <span data-ttu-id="951bc-136">此代码实现 **ClaimsPrincipal** 类的扩展方法，使您可以使用 Microsoft Graph 对象的值获取和设置声明。</span><span class="sxs-lookup"><span data-stu-id="951bc-136">This code implements extension methods for the **ClaimsPrincipal** class that allow you to get and set claims with values from Microsoft Graph objects.</span></span>

1. <span data-ttu-id="951bc-137">在名为 **BlazorAuthProvider.cs** 的 **/Graph** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="951bc-137">Create a new file in the **./Graph** directory named **BlazorAuthProvider.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/BlazorAuthProvider.cs" id="BlazorAuthProviderSnippet":::

    <span data-ttu-id="951bc-138">此代码实现 Microsoft Graph SDK 的身份验证提供程序，该提供程序使用 AspNetCore 获取访问令牌的 **IAccessTokenProviderAccessor** 提供的 **WebAssembly。**</span><span class="sxs-lookup"><span data-stu-id="951bc-138">This code implements an authentication provider for the Microsoft Graph SDK that uses the **IAccessTokenProviderAccessor** provided by the **Microsoft.AspNetCore.Components.WebAssembly.Authentication** package to get access tokens.</span></span>

1. <span data-ttu-id="951bc-139">在名为 **GraphClientFactory.cs** 的 **/Graph** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="951bc-139">Create a new file in the **./Graph** directory named **GraphClientFactory.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphClientFactory.cs" id="GraphClientFactorySnippet":::

    <span data-ttu-id="951bc-140">此类创建一个配置了 **BlazorAuthProvider** 的 **GraphServiceClient** 。</span><span class="sxs-lookup"><span data-stu-id="951bc-140">This class creates a **GraphServiceClient** configured with the **BlazorAuthProvider**.</span></span>

1. <span data-ttu-id="951bc-141">打开 **./Program.cs** 并将新的 **HttpClient** 的 **BaseAddress** 更改为 `"https://graph.microsoft.com"` 。</span><span class="sxs-lookup"><span data-stu-id="951bc-141">Open **./Program.cs** and change the **BaseAddress** of the new **HttpClient** to `"https://graph.microsoft.com"`.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="HttpClientSnippet":::

1. <span data-ttu-id="951bc-142">在行前面添加以下代码 `await builder.Build().RunAsync();` 。</span><span class="sxs-lookup"><span data-stu-id="951bc-142">Add the following code before the `await builder.Build().RunAsync();` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="AddGraphClientFactorySnippet":::

    <span data-ttu-id="951bc-143">这会将 **GraphClientFactory** 添加为一个范围内的服务，我们可以通过依赖关系注入进行使用。</span><span class="sxs-lookup"><span data-stu-id="951bc-143">This adds the **GraphClientFactory** as a scoped service that we can make available via dependency injection.</span></span>

1. <span data-ttu-id="951bc-144">打开 **/Graph/GraphUserAccountFactory.cs** ，并将以下属性添加到类中。</span><span class="sxs-lookup"><span data-stu-id="951bc-144">Open **./Graph/GraphUserAccountFactory.cs** and add the following property to the class.</span></span>

    ```csharp
    private readonly GraphClientFactory clientFactory;
    ```

1. <span data-ttu-id="951bc-145">更新构造函数以获取 **GraphClientFactory** 参数，并将其分配给 `clientFactory` 属性。</span><span class="sxs-lookup"><span data-stu-id="951bc-145">Update the constructor to take a **GraphClientFactory** parameter and assign it to the `clientFactory` property.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="ConstructorSnippet" highlight="2,7":::

1. <span data-ttu-id="951bc-146">将现有的 `AddGraphInfoToClaims` 函数替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="951bc-146">Replace the existing `AddGraphInfoToClaims` function with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphUserAccountFactory.cs" id="AddGraphInfoToClaimsSnippet":::

    <span data-ttu-id="951bc-147">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="951bc-147">Consider what this code does.</span></span>

    - <span data-ttu-id="951bc-148">它 [获取用户的配置文件](https://docs.microsoft.com/graph/api/user-get)。</span><span class="sxs-lookup"><span data-stu-id="951bc-148">It [gets the user's profile](https://docs.microsoft.com/graph/api/user-get).</span></span>
        - <span data-ttu-id="951bc-149">它用 `Select` 来限制返回的属性。</span><span class="sxs-lookup"><span data-stu-id="951bc-149">It uses `Select` to limit which properties are returned.</span></span>
    - <span data-ttu-id="951bc-150">它将 [获取用户的照片](https://docs.microsoft.com/graph/api/profilephoto-get)。</span><span class="sxs-lookup"><span data-stu-id="951bc-150">It [gets the user's photo](https://docs.microsoft.com/graph/api/profilephoto-get).</span></span>
        - <span data-ttu-id="951bc-151">它会专门请求用户照片的48x48 像素版本。</span><span class="sxs-lookup"><span data-stu-id="951bc-151">It requests specifically the 48x48 pixel version of the user's photo.</span></span>
    - <span data-ttu-id="951bc-152">将信息添加到 **ClaimsPrincipal** 中。</span><span class="sxs-lookup"><span data-stu-id="951bc-152">It adds the information to the **ClaimsPrincipal**.</span></span>

1. <span data-ttu-id="951bc-153">打开 **/Shared/LoginDisplay.razor** 并进行以下更改。</span><span class="sxs-lookup"><span data-stu-id="951bc-153">Open **./Shared/LoginDisplay.razor** and make the following changes.</span></span>

    - <span data-ttu-id="951bc-154">替换 `/img/no-profile-photo.png` 为 `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")` 。</span><span class="sxs-lookup"><span data-stu-id="951bc-154">Replace `/img/no-profile-photo.png` with `@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")`.</span></span>
    - <span data-ttu-id="951bc-155">替换 `placeholder@contoso.com` 为 `@context.User.GetUserGraphEmail()` 。</span><span class="sxs-lookup"><span data-stu-id="951bc-155">Replace `placeholder@contoso.com` with `@context.User.GetUserGraphEmail()`.</span></span>

    ```razor
    ...
    <img src="@(context.User.GetUserGraphPhoto() ?? "/img/no-profile-photo.png")"
         class="nav-profile-photo rounded-circle align-self-center mr-2">

    ...

    <p class="dropdown-item-text text-muted mb-0">@context.User.GetUserGraphEmail()</p>
    ...
    ```

1. <span data-ttu-id="951bc-156">保存全部更改并重新启动该应用程序。</span><span class="sxs-lookup"><span data-stu-id="951bc-156">Save all of your changes and restart the app.</span></span> <span data-ttu-id="951bc-157">登录到应用程序。</span><span class="sxs-lookup"><span data-stu-id="951bc-157">Log into the app.</span></span> <span data-ttu-id="951bc-158">应用程序将更新，以在顶部菜单中显示用户的照片。</span><span class="sxs-lookup"><span data-stu-id="951bc-158">The app updates to show the user's photo in the top menu.</span></span> <span data-ttu-id="951bc-159">选择用户的照片将打开一个下拉菜单，其中包含用户的姓名、电子邮件地址和 **"注销" 按钮。**</span><span class="sxs-lookup"><span data-stu-id="951bc-159">Selecting the user's photo opens a drop-down menu with the user's name, email address, and a **Log out** button.</span></span>

    ![显示用户照片的应用程序的屏幕截图](images/user-photo.png)

    ![下拉式菜单的屏幕截图](images/user-info.png)
