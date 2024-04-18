### Dockerfile for SSH

1. Building docker image: `sudo docker build -t my_ssh_image .`
2. Running the container of image: `sudo docker run -d -p 2222:22 --name my_ssh_container my_ssh_image`
3. Find the ip of the container: `sudo docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my_ssh_container`
4. ssh to the container: `ssh root@<ip of the above>`

