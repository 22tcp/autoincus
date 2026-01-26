260126  first part of redis role  - build
        builds sentinel components from source in given target ( use --limit )

110126  using group var content with map to select deb12 or deb13 incus cloud image
        created two template files adapting to 12 resp. 13

110126  mv deploy_redis.yml to examples

100126  mkhost bootstrap_ssh is working on the ~/.ssh/config 
        fixed template: Include must be inserted at BOF of the ssh config
        else the host to hostname to ip is not assigned
