# How to build
docker build \
--no-cache=true \
-t local/varnish5.2:0.1 \
--build-arg ENV_TYPE=production \
--build-arg VARNISH_STORAGE=malloc,100MB \
--build-arg VARNISH_TTL=120 \
--build-arg VARNISH_VCL_CONF=/etc/varnish/default.vcl \
--build-arg VARNISH_LISTEN_ADDRESS=127.0.0.1 \
--build-arg VARNISH_LISTEN_PORT=6081 \
--build-arg VARNISH_ADMIN_LISTEN_ADDRESS=127.0.0.1 \
--build-arg VARNISH_ADMIN_LISTEN_PORT=6082 \
--build-arg VARNISH_DAEMON_OPTIONS="-p thread_pool_min=5 -p thread_pool_max=2000 -p thread_pool_add_delay=4 -p thread_pools=4 -p pcre_match_limit_recursion=500 -p pcre_match_limit=2500 -p thread_pool_stack=72k" \
--build-arg VARNISH_CONTAINER_LOG_TYPE=varnishncsa \
--build-arg VARNISHNCSA_LOG_OPTIONS="%t - %{%Y-%m-%d}t - %{%H:%M:%S}t - %{X-Forwarded-For}i - %h - %r - %s - %b - %T - %D - %{Varnish:time_firstbyte}x - %{Varnish:handling}x - %{Referer}i - %{User-agent}i" \
. 

# How to run
docker run -d -p 8081:6081 \
--name varnish \
-e 'VARNISHNCSA_LOG_OPTIONS="-D -P /run/varnishncsa.pid -F %t - %{%Y-%m-%d}t - %{%H:%M:%S}t - %{X-Forwarded-For}i - %h - %r - %s - %b - %T - %D - %{Varnish:time_firstbyte}x - %{Varnish:handling}x - %{Referer}i - %{User-agent}i"' \
local/varnish5.2:0.1

# Useful links
https://copr.fedorainfracloud.org/coprs/ingvar/varnish52/packages/

# Available varibles
RELOAD_VCL=1
VARNISH_VCL_CONF=/etc/varnish/default.vcl
VARNISH_LISTEN_PORT=6081
VARNISH_ADMIN_LISTEN_ADDRESS=127.0.0.1
VARNISH_ADMIN_LISTEN_PORT=6082
VARNISH_SECRET_FILE=/etc/varnish/secret
VARNISH_TTL=120
VARNISH_USER=varnish
VARNISH_GROUP=varnish
NFILES=131072
MEMLOCK=82000
NPROCS="unlimited"

# Configuration for EC2 m4.xlarge
VARNISH_STORAGE="malloc,9.5GB"
DAEMON_OPTS="-p thread_pool_min=5 \ 
-p thread_pool_max=2000 \
-p thread_pool_add_delay=4 \
-p thread_pools=4 \
-p pcre_match_limit_recursion=500 \
-p pcre_match_limit=2500 \
-p thread_pool_stack=72k \
-p feature=+esi_ignore_https"
