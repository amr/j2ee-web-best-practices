J2EE Web Best Practices
=======================

Best-practices and common pitfalls when using J2EE for web applications

JSP
===
* JSPs by default will ask the servlet container to open a session. Prevent this by adding `session="false"` to your `page` directive. i.e. `<%@ page session="false" ... %@>`
