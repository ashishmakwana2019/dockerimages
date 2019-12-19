# Create data directory

    mkdir redis-data


# Docker image : 

    sudo docker run --name redis1 -p 6379:6379  -v /home/ubuntu/redis-data/:/data -d redis redis-server --appendonly yes --requirepass "redis@password"


# Test Connection : 

    redis-cli -h 127.0.0.1 -p 6379 -a redis@password ping