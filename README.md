# ctorgalson.nextcloud_snap

An Ansible role to install and configure the Nextcloud snap.

## Requirements

The role has no special requirements, but assumes an Ubuntu LTS (currently
20.04) server. It will install `snapd` if it's not already present.

**The role makes no attempt to ensure the server is secure**. That's your
responsibility, and how you do it depends on the environment the server runs
in. If it's the public internet, be careful :)

## Role Variables

**Note**: some required variables deliberately left undefined so that the role
won't run without them.

| Variable name | Required | Default | Description |
|---------------|---------------|----------|-------------|
| `nc_snap_admin_user`          | yes      | `nextcloud_admin` | Username for Nextcloud admin user. |
| `nc_snap_admin_password`      | yes      | `undefined`       | Password for Nextcloud admin user. |
| `nc_snap_nextcloud_dir`       | no       | `undefined`       | Path to optional nextcloud directory outside the snap. |
| `nc_snap_trusted_domains`     | yes      | `["localhost"]`   | The set of domains for accessing Nextcloud. |
| `nc_snap_letsencrypt_email`   | yes      | `undefined`       | The email address used for Letsencrypt certificate generation. |
| `nc_snap_nextcloud_commands`  | yes      | `[]`              | A set of arbitrary Nextcloud commands to run after the role tasks are complete. |

## Example Playbook

    - hosts: servers
      vars:
        # Login with this user.
        nc_snap_admin_user: "nextcloud_admin"
        # Login with this password.
        nc_snap_admin_password: "uQ21v6z22!6iho7h3n^4OxHnehXRbNDo"
        # Use this directory (perhaps a mounted external volume) for storage.
        nc_snap_nextcloud_data_dir: "/var/nextcloud/data"
        # Use only these domains to access the Nextcloud installation.
        nc_snap_trusted_domains:
          - "localhost"
          - "example.com"
        # Use this email address for Letsencrypt certificate generation.
        nc_snap_letsencrypt_email: "example@example.com"
        # Run these arbitrary nextcloud commands after the playbook runs.
        nc_snap_nextcloud_commands:
          - "status"
      roles:
         - ctorgalson.nextcloud_snap

## License

GPL-3.0-only

## Author Information

Christopher Torgalson
