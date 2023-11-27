seeting directory for cache path 
/var/cache/nginx-proxy-cache

proxy_cache_key: Defines the key used for caching. It combines the request scheme, method, host, and URI to form a unique cache key.

creates a mapping mobile_user
where default is DESKTOP_USER
else check user agent header and sets the appropriate value.

custom log formate is define on customuseragent and cache_st 

[$time_local]: Local time in square brackets.
$remote_addr: IP address of the client.
$upstream_cache_status: The status of the cache (hit, miss, etc.) for requests proxied to an upstream server.
$mobile_user: Categorized user as "DESKTOP_USER" or "MOBILE_USER" based on User-Agent.
$request_method: HTTP request method (GET, POST, etc.).
"$request_uri": Full original request URI.
($request_time|$upstream_response_time): Request processing time and upstream response time in parentheses.
($body_bytes_sent|$upstream_response_length): Bytes sent to the client and upstream response length in parentheses.
$status: HTTP response status code.
$host: Host header from the request.
"$http_user_agent": User-Agent header from the request.
"$http_referer": Referer header from the request.

$remote_addr: IP address of the client.
$upstream_cache_status: The status of the cache (hit, miss, etc.) for requests proxied to an upstream server.
[$time_local]: Local time in square brackets.
"$request": Full original request line.
$status: HTTP response status code.
$body_bytes_sent: Number of bytes sent to the client.
"$http_referer": Referer header from the request.
"$http_user_agent": User-Agent header from the request.

Defining upstream backend server running on port 8081 to pass al the request to this server.

initializing the variable 
$wp_user, 
$auth_user, "logout"
and $skip_cache

The real IP address of the client is determined using Cloudflare headers (CF-Connecting-IP).
real_ip_recursive on; and set_real_ip_from 0.0.0.0/0; are used to handle multiple proxy headers.

Setting configuration for proxy 



Include files
remove_header.conf
cache_general.conf
cookie_info.conf
ignore_headers.conf

proxy_hide_header Set-Cookie;: Hides the "Set-Cookie" header in the response.

setting value of cache_key_sys 
using in file cache_general.conf

setting different caching for different variable



