# Chia-Datalayer-S3-Sync
Sync Chia Datalayer .dat files to an S3 bucket.  This is useful to be able to serve your datalayer files over HTTP without exposing any ports to your system that runs Chia. 

# Installation and usage

This is intended to run on a Linux server or any system with a Linux style Bash shell with systemd.  Example commands assume a Debian-based distro such as Ubuntu, but the general script should run on any distro. 

1.  Copy this repo to the machine running Chia Datalayer (`git clone https://github.com/TheLastCicada/Chia-Datalayer-S3-Sync.git && cd Chia-Datalayer-S3-Sync`)
2.  Create an AWS bucket
3.  Give yourself access to write to the bucket. An [instance profile](https://repost.aws/knowledge-center/ec2-instance-access-s3-bucket) is recommended if hosting Chia on EC2
4.  Install the [official AWS CLI tool](https://aws.amazon.com/cli/)
5.  Install ionotify-tools `apt install inotify-tools`
6.  Edit [line 27 in datalayer-s3-sync](https://github.com/TheLastCicada/Chia-Datalayer-S3-Sync/blob/main/datalayer-s3-sync#L27) and replace `/home/zachary/.chia/mainnet/data_layer/db/server_files_location_testnet10` with the path to your datalayer .dat files (will be similar except the part `zachary` will be your username and the `testnet10` part will the the network you are using, such as `mainnet` or `testneta`).  Also replace `s3://mydatalayer1` with the S3 address of the bucket you created in step 1.
7.  Edit [line 28 in datalayer-s3-sync](https://github.com/TheLastCicada/Chia-Datalayer-S3-Sync/blob/main/datalayer-s3-sync#L28) and replace with the path to your datalayer .dat files (should be the same directory as in step 6).
8.  Move `datalayer-s3-sync` to `/usr/local/bin/datalayer-s3-sync` (`sudo mv ./datalayer-s3-sync /usr/local/bin/datalayer-s3-sync`)\
9.  Make `datalayer-s3-sync` executable for all users with `sudo chmod +x /usr/local/bin/datalayer-s3-sync`
10.  Edit [line 12 and 13 in datalayer-s3-sync.service](https://github.com/TheLastCicada/Chia-Datalayer-S3-Sync/blob/main/datalayer-s3-sync.service#L12-L13) and change the `User` and `Group` to be the username where Chia runs on your system. 
11.  Move `datalayer-s3-sync.service` to `/etc/systemd/system/datalayer-s3-sync.service` (`sudo mv ./datalayer-s3-sync.service /etc/systemd/system/datalayer-s3-sync.service`)
12.  Reload systemd to recognize the new file with `sudo systemctl daemon-reload`
13.  Start the sync process with `sudo systemctl start datalayer-s3-sync`
14.  Set the sync process to start at boot with `sudo systemctl enable datalayer-s3-sync`
