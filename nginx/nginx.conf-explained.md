# Nginx Configuration File Explain 
## nginx version: nginx/1.14.2

### built with OpenSSL 1.1.1n  15 Mar 2022
TLS SNI support enabled
configure arguments: --with-cc-opt='-g -O2 -fdebug-prefix-map=/build/nginx-FlcIR2/nginx-1.14.2=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_geoip_module=dynamic --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_xslt_module=dynamic --with-stream=dynamic --with-stream_ssl_module --with-stream_ssl_preread_module --with-mail=dynamic --with-mail_ssl_module --add-dynamic-module=/build/nginx-FlcIR2/nginx-1.14.2/debian/modules/http-auth-pam --add-dynamic-module=/build/nginx-FlcIR2/nginx-1.14.2/debian/modules/http-dav-ext --add-dynamic-module=/build/nginx-FlcIR2/nginx-1.14.2/debian/modules/http-echo --add-dynamic-module=/build/nginx-FlcIR2/nginx-1.14.2/debian/modules/http-upstream-fair --add-dynamic-module=/build/nginx-FlcIR2/nginx-1.14.2/debian/modules/http-subs-

Configure Arguments:

--with-cc-opt: Specifies compiler options, such as optimization, debugging information, and security features.
--with-ld-opt: Specifies linker options, including security features.
--prefix: Sets the base directory where Nginx will be installed.
--conf-path: Specifies the path to the main Nginx configuration file.
--http-log-path: Sets the path for the HTTP access log.
--error-log-path: Sets the path for the error log.
--lock-path: Defines the path for the lock file.
--pid-path: Specifies the file where the process ID of the main process will be stored.
--modules-path: Sets the path where dynamic modules will be stored.
--http-client-body-temp-path: Defines the path for storing temporary files with the client request body.
--http-fastcgi-temp-path: Sets the path for storing temporary files for FastCGI.
--http-proxy-temp-path: Defines the path for storing temporary files for proxying.
--with-debug: Enables debugging support.
--with-pcre-jit: Enables just-in-time compilation for PCRE (Perl Compatible Regular Expressions).
Various --with-http options enable different HTTP modules like SSL, status, real IP, authentication, etc.
--with-stream and related options enable modules for stream processing.
--with-mail and related options enable modules for mail servers.
--add-dynamic-module: Adds dynamic modules to extend Nginx functionality.


Modules enabled 
with-cc
with-debug
with-http
with-ld
with-mail
with-pcre
with-stream
with-threads

ls /etc/nginx/modules-enabled/

50-mod-http-auth-pam.conf  50-mod-http-echo.conf   50-mod-http-image-filter.conf  50-mod-http-upstream-fair.conf  50-mod-mail.conf
50-mod-http-dav-ext.conf   50-mod-http-geoip.conf  50-mod-http-subs-filter.conf   50-mod-http-xslt-filter.conf    50-mod-stream.conf

soft link with the dir  /usr/share/nginx/modules-available/

mod-http-auth-pam.conf  mod-http-echo.conf   mod-http-image-filter.conf  mod-http-upstream-fair.conf  mod-mail.conf
mod-http-dav-ext.conf   mod-http-geoip.conf  mod-http-subs-filter.conf   mod-http-xslt-filter.conf    mod-stream.conf


nginx.conf explaination

creating groups XX and ZZ
            default           ZZ;
            127.0.0.1/32      XX;
            172.31.56.57/32     XX;
            172.31.54.147/32    XX;

setting $invalid_hostname variable based on $http_host
values : valid and invalid host 
valid     snifffr.com 0; www.snifffr.com 0; 127.0.0.1   0;
invalid default      1;

setting $bad_ua variable based on $http_user_agent
listed users on /etc/nginx/include/bad_user_agent.list are bad rest all good.

setting $bad_uri on the basis of $uri $bad_uri
is any of theif maching any of the key words 
cgi-bin," "cgi," "tmp," "license," "MyAdmin," "pma," "phpMoAdmin," "phpmyadmin," or "admin.

setting variable for $bad_method on the basis $request_method
 "GET", "POST", "OPTIONS", "HEAD" are valid rest all invalid 

Create a map value ban_rule on the bases on $bad_uri:$bad_ua:$invalid_hostname
on the basis of these three values value for ban_rule is set.
            default     "Unknown";
            0:0:0       "All good";
            0:0:1       "Invalid host";
            0:1:0       "Bad User-Agent";
            0:1:1       "Bad User-Agent and Invalid host";
            1:0:0       "Bad URI";
            1:0:1       "Bad URI and Invalid host";
            1:1:0       "Bad URI and User-Agent";
            1:1:1       "Bad URI, User-Agent and host";

Similarly for block_rule mapping values are set on the basis of these three values
            default     0;
            0:0:0       0;
            0:0:1       1;
            0:1:0       1;
            0:1:1       1;
            1:0:0       1;
            1:0:1       1;
            1:1:0       1;
            1:1:1       1;

on the basis of all the above maps it finally uses this it on block_it map create value 
on the basis of good_address block_rule
            defailt     0;
                XX:0    0;
                XX:1    0;
                ZZ:1    1;
