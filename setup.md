
checkout
sysctl -w vm.max_map_count=262144
docker-compose -f create-certs.yml run --rm create_certs
docker-compose up
docker-compose exec -T es01 bin/elasticsearch-setup-passwords auto --batch -E "xpack.security.http.ssl.verification_mode=certificate"

# increase java memory:
# pass as env var in all elastic containers
- "ES_JAVA_OPTS=-Xms2g -Xmx2g"


# snapshots
- create volume (hetzner cloud)
- follow instructions to mount and format volume (volume is now mounted at /mnt/volume-elastic-snapshots)
- create folder in volume /mnt/volume-elastic-snapshots/data
- chown -R 1000.root data
- docker-mount volume:
    /mnt/volume-elastic-snapshots/data:/usr/share/elasticsearch/snapshots:rw
- add env var:
    path.repo=/usr/share/elasticsearch/snapshots


