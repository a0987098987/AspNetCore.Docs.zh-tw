## <a name="usermanager-and-signinmanager"></a><span data-ttu-id="a4022-101">使用者管理員與登入管理員</span><span class="sxs-lookup"><span data-stu-id="a4022-101">UserManager and SignInManager</span></span>

<span data-ttu-id="a4022-102">在伺服器應用需要時設定使用者識別符聲明類型:</span><span class="sxs-lookup"><span data-stu-id="a4022-102">Set the user identifier claim type when a Server app requires:</span></span>

* <span data-ttu-id="a4022-103"><xref:Microsoft.AspNetCore.Identity.UserManager%601>或在<xref:Microsoft.AspNetCore.Identity.SignInManager%601>API 終結點中。</span><span class="sxs-lookup"><span data-stu-id="a4022-103"><xref:Microsoft.AspNetCore.Identity.UserManager%601> or <xref:Microsoft.AspNetCore.Identity.SignInManager%601> in an API endpoint.</span></span>
* <span data-ttu-id="a4022-104"><xref:Microsoft.AspNetCore.Identity.IdentityUser>詳細資訊,例如使用者名、電子郵寄地址或鎖定結束時間。</span><span class="sxs-lookup"><span data-stu-id="a4022-104"><xref:Microsoft.AspNetCore.Identity.IdentityUser> details, such as the user's name, email address, or lockout end time.</span></span>

<span data-ttu-id="a4022-105">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="a4022-105">In `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<IdentityOptions>(options => 
    options.ClaimsIdentity.UserIdClaimType = ClaimTypes.NameIdentifier);
```

<span data-ttu-id="a4022-106">呼叫`WeatherForecastController`<xref:Microsoft.AspNetCore.Identity.IdentityUser%601.UserName>`Get`方法時,以下紀錄 :</span><span class="sxs-lookup"><span data-stu-id="a4022-106">The following `WeatherForecastController` logs the <xref:Microsoft.AspNetCore.Identity.IdentityUser%601.UserName> when the `Get` method is called:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.Logging;
using BlazorAppIdentityServer.Server.Models;
using BlazorAppIdentityServer.Shared;

namespace BlazorAppIdentityServer.Server.Controllers
{
    [Authorize]
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        private readonly UserManager<ApplicationUser> _userManager;

        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", 
            "Balmy", "Hot", "Sweltering", "Scorching"
        };

        private readonly ILogger<WeatherForecastController> logger;

        public WeatherForecastController(ILogger<WeatherForecastController> logger, 
            UserManager<ApplicationUser> userManager)
        {
            this.logger = logger;
            _userManager = userManager;
        }

        [HttpGet]
        public async Task<IEnumerable<WeatherForecast>> Get()
        {
            var rng = new Random();

            var user = await _userManager.GetUserAsync(User);

            if (user != null)
            {
                logger.LogInformation($"User.Identity.Name: {user.UserName}");
            }

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = DateTime.Now.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            })
            .ToArray();
        }
    }
}
```
