```
using Microsoft.AspNetCore.Http;
using System.Linq;
using System.Threading.Tasks;

namespace YourSpace.Core.Middlewares
{
    public class CustomMediaAuthentication
    {
        private readonly RequestDelegate _next;

        public CustomMediaAuthentication(RequestDelegate next)
        {
            _next = next;
        }

        public async Task InvokeAsync(HttpContext context)
        {
            
            if (!context.Request.Path.StartsWithSegments("/media"))
            {
                await _next(context);
                return;
            }

            //Authenticate user
            if (context.User.Identities.Any(IdentityExtensions => IdentityExtensions.IsAuthenticated))
            {
                await _next(context);
                return;
            }

            // Stop processing the request and return a 401 response.
            context.Response.StatusCode = 401;
            await Task.FromResult(0);
            return;
        }
    }
} 
```




### The extension class (CustomMediaAuthenticationExtension.cs)

```
using Microsoft.AspNetCore.Builder;

namespace Ninepoint.Core.Middlewares
{
    public static class IApplicationBuilderExtention
    {
        public static IApplicationBuilder UseCustomMediaAuthentication(this IApplicationBuilder app)
        {
            return app.UseMiddleware<CustomMediaAuthentication>();
        }
    }
}
```
  
### And the call in Startup.cs:

```
    //UseCustomMediaAuthentication() relies on UseAuthentication()
    //Include before app.UseUmbraco()
    app.UseAuthentication();
    app.UseCustomMediaAuthentication();
    
    ```

Ref link : https://our.umbraco.com/forum/using-umbraco-and-getting-started/108979-intercept-all-media-requests
