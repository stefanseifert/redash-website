# **How to Upgrade Redash**

It’s recommended to upgrade your Redash instance once there are new releases, to benefit from new features and bug fixes. The upgrade process is relatively simple, and assuming you used one of the base images we provide, you can just use the [Fabric](http://www.fabfile.org/) script provided here: [https://gist.github.com/arikfr/440d1403b4aeb76ebaf8](https://gist.github.com/arikfr/440d1403b4aeb76ebaf8).

## **How to Run the Fabric Script**

1. Install Fabric: `pip install fabric requests` (needed only once)

2. Download the `fabfile.py` from the gist.

3. Run the script:`fab -H{your Redash host} -u{the ssh user for this host} -i{path to key file for passwordless login} deploy_latest_release`

  `-i` is optional and it is only needed in case you’re using private-key based authentication (and didn’t add the key file to your authentication agent or set its path in your SSH config).


## **What the Fabric Script Does**

Even if you didn’t use the image, it’s very likely you can reuse most of this script with small modifications. What this script does is:

1. Find the URL of the latest release tarball (from [GitHub releases page](http://github.com/getredash/redash/releases)).
2. Download it.
3. Create new directory for this version (for example: `/opt/redash/redash.0.5.0.b685`).
4. Unpack that (`tar -C {dir} -xvf {tarball path}`).
5. Link `/opt/redash/.env` file into this directory.
6. Apply any new migrations.
7. Link `/opt/redash/current` to new version.
8. Install any new requirements - `sudo pip install -r requirements.txt`
9. Restart web server and celery workers.
