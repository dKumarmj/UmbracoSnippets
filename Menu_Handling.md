 ```
 public class component : IComponent

 public void Initialize()
        {
           Umbraco.Web.Trees.TreeControllerBase.MenuRendering += TreeControllerBase_MenuRendering;
        }

void TreeControllerBase_MenuRendering(Umbraco.Web.Trees.TreeControllerBase sender, Umbraco.Web.Trees.MenuRenderingEventArgs e)
        {
            // this example will add a custom menu item for all admin users

            // for all content tree nodes
            if (sender.TreeAlias == "content"
                && sender.Security.CurrentUser.Groups.Any(x => x.Alias.InvariantEquals("admin")))
            {
                // creates a menu action that will open /umbraco/currentSection/itemAlias.html
                var i = new Umbraco.Web.Models.Trees.MenuItem("itemAlias", "Item name");

                // optional, if you want to load a legacy page, otherwise it will follow convention
                i.AdditionalData.Add("actionUrl", "my/long/url/to/webformshorror.aspx");

                // optional, if you don't want to follow the naming conventions, but do want to use a angular view
                // you can also use a direct path "../App_Plugins/my/long/url/to/view.html"
                i.AdditionalData.Add("actionView", "my/long/url/to/view.html");

                // sets the icon to icon-wine-glass
                i.Icon = "wine-glass";

                // insert at index 5
                e.Menu.Items.Insert(5, i);
            }
        }
        
         public void Terminate()
        {
            // unsubscribe on shutdown
         Umbraco.Web.Trees.TreeControllerBase.MenuRendering -= TreeControllerBase_MenuRendering;
         
        }
        }
```
