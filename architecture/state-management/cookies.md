# Cookies
## Limitations

* Limited to 4096 bytes.
* 20 cookies per site.
* Users can refuse cookies.

## Adding a Cookie

    Response.Cookies["userName"].Value = "patrick";
    Response.Cookies["userName"].Expires = DateTime.Now.AddDays(1);

    HttpCookie aCookie = new HttpCookie("lastVisit");
    aCookie.Value = DateTime.Now.ToString();
    aCookie.Expires = DateTime.Now.AddDays(1);
    Response.Cookies.Add(aCookie);

## Subkeys
You can store one value in a cookie, such as user name and last visit. You can also store multiple name-value pairs in a single cookie. The name-value pairs are referred to as subkeys.

    Response.Cookies["userInfo"]["userName"] = "patrick";
    Response.Cookies["userInfo"]["lastVisit"] = DateTime.Now.ToString();
    Response.Cookies["userInfo"].Expires = DateTime.Now.AddDays(1);

    HttpCookie aCookie = new HttpCookie("userInfo");
    aCookie.Values["userName"] = "patrick";
    aCookie.Values["lastVisit"] = DateTime.Now.ToString();
    aCookie.Expires = DateTime.Now.AddDays(1);
    Response.Cookies.Add(aCookie);

## Reading a Cookie
When a browser makes a request to the server, it sends the cookies for that server along with the request.

    if(Request.Cookies["userName"] != null)
        Label1.Text = Server.HtmlEncode(Request.Cookies["userName"].Value);

    if(Request.Cookies["userName"] != null)
    {
        HttpCookie aCookie = Request.Cookies["userName"];
        Label1.Text = Server.HtmlEncode(aCookie.Value);
    }

## Changing a Cookie's Expiration Date
The browser is responsible for managing cookies, and the cookie's expiration time and date help the browser manage its store of cookies. Therefore, although you can read the name and value of a cookie, you cannot read the cookie's expiration date and time. When the browser sends cookie information to the server, the browser does not include the expiration information. (The cookie's Expires property always returns a date-time value of zero.) If you are concerned about the expiration date of a cookie, you must reset it, which is covered in the "Modifying and Deleting Cookies" section.

## Modifying and Deleting Cookies
You cannot directly modify a cookie. Instead, changing a cookie consists of creating a new cookie with new values and then sending the cookie to the browser to overwrite the old version on the client.

## Deleting Cookies
Deleting a cookie—physically removing it from the user's hard disk—is a variation on modifying it. You cannot directly remove a cookie because the cookie is on the user's computer. However, you can have the browser delete the cookie for you. The technique is to create a new cookie with the same name as the cookie to be deleted, but to set the cookie's expiration to a date earlier than today.

## Detecting Cookie Acceptance

One way to determine whether cookies are accepted is by trying to write a cookie and then trying to read it back again. If you cannot read the cookie you wrote, you assume that cookies are turned off in the browser.
