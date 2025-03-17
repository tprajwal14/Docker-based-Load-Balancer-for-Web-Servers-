
🚀 Docker-based Load Balancer for Web Servers
  This project demonstrates a simple NGINX load balancer setup using Docker. It includes two backend web server containers and an NGINX proxy server acting as the load balancer.

🛠️ Prerequisites
  Docker installed
  Basic understanding of Docker networking and NGINX

🚀 Setup Steps
1. Create Two NGINX Containers (Web Servers)
  docker run -d --name container1 nginx
  docker run -d --name container2 nginx
2. Modify index.html in Each Container
For container1:
  docker exec -it container1 bash
cd /usr/share/nginx/html
echo '<!DOCTYPE html>
<html>
<head><title>Welcome to nginx!</title></head>
<body><h1>Welcome to container1</h1></body>
</html>' > index.html
exit

For container2:
  docker exec -it container2 bash
cd /usr/share/nginx/html
echo '<!DOCTYPE html>
<html>
<head><title>Welcome to nginx!</title></head>
<body><h1>Welcome to container2</h1></body>
</html>' > index.html
exit

3. Create and Run NGINX Proxy (Load Balancer)
     docker run -d --name proxyserver -p 80:80 nginx

4. Create Docker Network and Connect Containers
     docker network create mynetwork

docker network connect mynetwork container1
docker network connect mynetwork container2
docker network connect mynetwork proxyserver

5. Configure NGINX Load Balancer in Proxy Server
Access proxy server:
  docker exec -it proxyserver bash
cd /etc/nginx/conf.d

6. Edit default.conf (or create a new one):
    upstream backend {
    server container1:80;
    server container2:80;
}

server {
    listen 80;

    location / {
        proxy_pass http://backend;
    }
}

6. Test the Setup
Open your browser or use curl:
    curl http://<YOUR_PUBLIC_IP>

🎯 Outcome
✅ Two NGINX web servers behind an NGINX reverse proxy acting as a load balancer
✅ Load balanced traffic across both containers
✅ Simple yet powerful Docker networking
