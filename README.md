J2EE Web Best Practices
=======================

Best-practices and common pitfalls when using J2EE for web applications

JSP
===
* JSPs by default will ask the servlet container to open a session. This results in a new session having to be maintained by the servlet container, and session tracking to be activated. Depending on your configuration, at best it will result in a `JSESSIONID` cookie being sent to the client which, aside from being useless, will also make the job of properly configuring a reverse proxy cache pretty messy. Prevent this by adding `session="false"` to your `page` directive in all of your JSPs (i.e. `<%@ page session="false" ... %>`). You may choose to have that in a separate JSP which you include in all of your JSPs (using the the `<%@ include ... %>` directive, not the `<jsp:include .. />` tag). **Note**: doing so has a side effect, a pretty good one if you ask me, the JSP engine will stop resolving session attributes as JSP variables/attributes as previously, setting `request.getSession().setAttribute('foo', 'foo-value')` in the servlet would have the same effect as passing the `foo` attribute directly to the JSP's `PageContext` (such as through your MVC layer). IMHO, mixing the two was a big mistake on the servlet api designer's side, as they are two different scopes, and mixing them together is a recipe for smelly code and an open door for regressions and security. It reminds me of PHP's `register_globals`.

* JSPs by default will try to resolve page attributes from the passed page context and fall back to session attributes. This is a design pitfal and relying on this behavior is prohibited. To enforce it, set `session="false"` as explained in the previous point. 

HTTP Sessions
=============

* Configure your application to manually remove the session cookie (i.e. `JSESSIONID`) after invalidating sessions through the `session.invalidate()` API as in some servlet containers this API doesn't remove the cookie, it merely marks the session as expired/invalid. This is confirmed in Tomcat 6.0.35.

Character Encoding 
==================
The answer for the SO question [How to get UTF-8 working in java webapps?](http://stackoverflow.com/a/138950/43597) summarizes it pretty well. Other additions below:

JSPs need the `<%@ pageEncoding="UTF-8" %>` property for all JSPs. A DRY solution is to add the following to `web.xml`:

    <jsp-config>
      <jsp-property-group>
        <url-pattern>*.jsp</url-pattern>
        <page-encoding>UTF-8</page-encoding>
      </jsp-property-group>
    </jsp-config>
