## apache web server is used to server the php wirdpress cintent 


virtual host is is listing on port 8081
for the directoey having source code

Options -Indexes +FollowSymLinks +MultiViews: Specifies directory options, allowing directory listing (Indexes) to be disabled and enabling symbolic links and multiple views.
AllowOverride All: Allows the use of .htaccess files to override directory configuration.
Require all granted: Grants access to all users.

based on HTTP_AUTHORIZATION

SetEnvIfNoCase ^Authorization$ "(.+)" HTTP_AUTHORIZATION=$1: Sets an environment variable HTTP_AUTHORIZATION based on the Authorization header.

HTTP_AUTHORIZATION: The name of the environment variable being set.
$1: Represents the content captured by the first capturing group in the regular expression

<FilesMatch ".+\.ph(p[3457]?|t|tml)$">: Matches files with PHP extensions. 

SetHandler "proxy:unix:/run/php/php7.4-fpm.sock|fcgi://localhost": Directs requests for PHP files to the PHP-FPM handler.

LogLevel warn: Sets the logging level to "warn".

This configuration sets up Apache to serve a website on port 8081, defines the document root, configures directory settings, handles PHP files through PHP-FPM, and sets up logging for error and access logs. It also includes security measures to deny access to version control directories and certain file types. 


log formatting 

LogFormat "%a %l %u %t -- %T/%D --  \"%r\" %>s %O \"%{Referer}i\" \"%{User-Agent}i\"" combined

%v:%p: The virtual host name and port number.
%h: The remote host (client IP address).
%l: Remote log name (from identd, not usually used).
%u: Authenticated user (if any).
%t: Time the request was received.
-- %T/%D --: Time taken to serve the request in seconds (%T) and microseconds (%D).
\"%r\": First line of the request.
%>s: Status of the last request (3-digit HTTP status code).
%O: Bytes sent, including headers.
\"%{Referer}i\": Referer header from the request.
\"%{User-Agent}i\": User-Agent header from the request.

LogFormat "%h %l %u %t \"%r\" %>s %O" common

%a: Client IP address.
%l, %u, %t, -- %T/%D --, \"%r\", %>s, %O, \"%{Referer}i\", \"%{User-Agent}i\": Same as in vhost_combined`

LogFormat "%h %l %u %t \"%r\" %>s %O" common
%h, %l, %u, %t, \"%r\", %>s, %O: Common log format components without the time taken (%T/%D) and referer/user-agent.

LogFormat "%{Referer}i -> %U" referer
%{Referer}i: Referer header from the request.
%U: The URL path requested.

LogFormat "%{User-agent}i" agent
%{User-agent}i: User-Agent header from the request.

Loaded Modules:
 core_module (static)
 so_module (static)
 watchdog_module (static)
 http_module (static)
 log_config_module (static)
 logio_module (static)
 version_module (static)
 unixd_module (static)
 access_compat_module (shared)
 actions_module (shared)
 alias_module (shared)
 auth_basic_module (shared)
 authn_core_module (shared)
 authn_file_module (shared)
 authz_core_module (shared)
 authz_host_module (shared)
 authz_user_module (shared)
 autoindex_module (shared)
 cache_module (shared)
 deflate_module (shared)
 dir_module (shared)
 env_module (shared)
 expires_module (shared)
 fcgid_module (shared)
 filter_module (shared)
 headers_module (shared)
 mime_module (shared)
 mpm_event_module (shared)
 negotiation_module (shared)
 proxy_module (shared)
 proxy_fcgi_module (shared)
 remoteip_module (shared)
 reqtimeout_module (shared)
 rewrite_module (shared)
 setenvif_module (shared)
 socache_shmcb_module (shared)
 ssl_module (shared)
 status_module (shared)