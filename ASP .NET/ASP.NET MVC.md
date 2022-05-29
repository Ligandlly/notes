# ASP.NET Core MVC

## theme

在`Views/Shared/_Layout.cshtml`中

```asp
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
```

## Controllers

```csharp
public class MoviesController : Controller
{
    // GET: /Movies/Random
    public IActionResult Random()
    {
        var movie = new Movie { Name = "SEU!" };
        return View(movie);
    }

    // GET: /movies/edit?id=1
    public IActionResult Edit(int id)
    {
        return Content($"ID = {id}");
    }
}
```

Startup.cs

```csharp
...
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
...
```

## Route

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
      name: "MovieByReleaseDate",
      pattern: "Movies/Released/{year}/{month}",
      defaults: new { controller = "Movies", action = "ByReleaseDate" },
      constraints: new { year = @"\d{4}", month = @"\d{2}" });

    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

## Attribute Route

In `MoviesControllers.cs`

```csharp
[Route("movies/released/{year}/{month:regex(\\d{{2}}):range(1,12)}")]
public IActionResult ByReleaseDate(int year, int month)
{
    return Content(year + "/" + month);
}
```

## ViewModel

View只有一个Model参数，如果要传入多个Model，就要用View Model
