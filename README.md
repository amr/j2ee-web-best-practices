J2EE Web Best Practices
=======================

Best-practices and common pitfalls when using J2EE for web applications

JSP
===
* JSPs by default will ask the servlet container to open a session. This results in a new session having to be maintained by the servlet container, and session tracking to be activated. Depending on your configuration, at best it will result in a `JSESSIONID` cookie being sent to the client which, aside from being useless, will also make the job of properly configuring a reverse proxy cache pretty messy. Prevent this by adding `session="false"` to your `page` directive in all of your JSPs (i.e. `<%@ page session="false" ... %>`). You may choose to have that in a separate JSP which you include in all of your JSPs (using the the `<%@ include ... %>` directive, not the `<jsp:include .. />` tag)


Character Encoding 
==================
The answer for the SO question [How to get UTF-8 working in java webapps?](http://stackoverflow.com/a/138950/43597) summarizes it pretty well.
