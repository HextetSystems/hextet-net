hextet-net
==========

hextet.net site

nginx configuration
-------------------

```
map $http_user_agent $myindex {
	default /index.html;
	~curl /cmdline.html;
	~Wget /cmdline.html;
	~HTTPie /cmdline.html;
}

# Redirect everything to the main site.
server {
	listen 199.202.21.13:80; 
	listen [2604:4280:d000:11::13]:80;

	server_name hextet.net www.hextet.net hextet.com www.hextet.com hextet.info www.hextet.info hextet.ca www.hextet.ca;
	root /var/www/hextet-net;

	access_log /var/log/nginx/hextet_net_access.log;
	error_log /var/log/nginx/hextet_net_error.log;

	error_page 404 = /index.html;
	ssi on;

	location = / { rewrite ^ $myindex; }

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ $uri.html =404;
		# Uncomment to enable naxsi on this location
		# include /etc/nginx/naxsi.rules;
	}

}
```
