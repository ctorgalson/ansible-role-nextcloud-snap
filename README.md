# ctorgalson.nextcloud_snap

An Ansible role to install and configure the Nextcloud snap. For simplicity,
the self-contained, self-updating snap is unbeatable. You'll have to be ok
with the Snap ecosystem, of course :)

## Requirements

The role has one requirement: [the Ansible `community.general` collection](https://docs.ansible.com/ansible/latest/collections/community/general/).

Install this with `ansible-galaxy install community.general`.

The role has no other special requirements, but assumes an Ubuntu LTS server
(18.04, 20.04, or 22.04 at present) server, and will install `snapd` if it's
not already present.

**The role makes no attempt to ensure the server is secure**. That's your
responsibility, and how you do it depends on the environment the server runs
in. If it's the public internet, be careful :)

## Role Variables

**Note**: some required variables deliberately left undefined so that the role
won't run without them.

| Variable name                               | Required | Default           | Description |
|---------------------------------------------|----------|-------------------|-------------|
| `nc_snap_install_channel`                   | yes      | `stable`          | The channel to use for installing the snap; options include beta, candidate, edge, and stable. |
| `nc_snap_admin_user`                        | yes      | `nextcloud_admin` | Username for Nextcloud admin user. |
| `nc_snap_admin_password`                    | yes      | `undefined`       | Password for Nextcloud admin user. |
| `nc_snap_nextcloud_data_dir`                | no       | `undefined`       | Path to optional data directory outside the snap.\* |
| `nc_snap_trusted_domains`                   | yes      | `["localhost"]`   | The set of domains for accessing Nextcloud. |
| `nc_snap_letsencrypt_email`                 | yes      | `undefined`       | The email address used for Letsencrypt certificate generation. |
| `nc_snap_letsencrypt_timeout`               | no       | `undefined`       | Timeout value for letsencrypt task. |
| `nc_snap_nextcloud_commands`                | yes      | `[]`              | A set of arbitrary Nextcloud commands to run after the role tasks are complete. |
| `nc_snap_nextcloud_connect_removable_media` | no       | `false`           | Whether or not to run `snap connect nextcloud:removable-media` with commands tasks. |
| `nc_snap_nextcloud_connect_network_observe` | no       | `false`           | Whether or not to run `snap connect nextcloud:network-observe` with commands tasks. |

\* Storage outside the app [is no longer unsupported](https://github.com/nextcloud/nextcloud-snap/issues/1670),
but the new `-d` option for `nextcloud.manual-install` doesn't actually work
with any value other than the default. The setting is present and will be
honoured if the variable is set.

If it doesn't work for you, your options may be limited to the storage space on
the disk the snap resides on, but note that it's possible to enale external
storage from within Nextcloud.

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
