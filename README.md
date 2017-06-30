[![Build Status](http://img.shields.io/travis/cchurch/ansible-role-win-vipre.svg)](https://travis-ci.org/cchurch/ansible-role-win-vipre)
[![Galaxy](http://img.shields.io/badge/galaxy-cchurch.win--vipre-blue.svg)](https://galaxy.ansible.com/cchurch/win-vipre/)

Win-VIPRE
=========

Manage VIPRE Business Antivirus Server on Windows. Backup, restore, install,
update and uninstall VIPRE Business, Business Premium or Endpoint Security.

Role Variables
--------------

Use the following variables to specify the VIPRE product and version to be
managed. These variables affect the the install and uninstall capabilities of
this role:

- `vipre_product`: VIPRE Business product, should be one of `"standard"`,
  `"premium"` or `"endpoint"`, default is `"standard"`.
- `vipre_version`: Version of VIPRE Business to install, upgrade or uninstall.
  Valid values are `"latest"`, `"9.6"`, `"9.5"` and `"9.3"`; default is `"latest"`.

To backup the VIPRE database (internal SQLite database only, SQL Server is not
yet supported), use the the following variables:

- `vipre_backup`: Set to `true` to enable the various backup options below,
  default is `false`.
- `vipre_backup_remote`: Backup the database to a destination on the remote
  system, default is `true`.
- `vipre_backup_remote_path`: Remote path to store database backup. Default is
  `""`, which uses the full database file with `vipre_backup_suffix` appended.
- `vipre_backup_local`: Backup the database to a destination on the local
  (controlling) system, default is `false`.
- `vipre_backup_local_path`: Local path to store database backup. Default is
  `""`, which uses the current directory (`playbook_dir`) and database file name
  with `vipre_backup_suffix` appended.
- `vipre_backup_suffix`: Suffix to append to the database backup filename,
  default is `ansible_date_time.iso8601_basic_short` which appends a timestamp.
- `vipre_backup_ini`: Backup the VIPRE Site GUID and Registration Code to an INI
  file alongside the database backup, default is `true`. The INI filename will
  be the same as the databas backup filename with `.ini` appended.

To restore the VIPRE internal SQLite database from a backup, use the following
variables:

- `vipre_restore`: Set to `true` to enable the various restore options below.
- `vipre_restore_remote`: Restore the database from a backup file on the remote
  system, default is `false`. If set to `true`, `vipre_restore_local` must be
  `false`.
- `vipre_restore_remote_path`: Remote path to database file; must be provided if
  `vipre_restore_remote` is `true`.
- `vipre_restore_local`: Restore the database from a backup file on the local
  (controlling) system, default is `false`. If set to `true`,
  `vipre_restore_remote` must be `false`.
- `vipre_restore_local_path`: Local path to database file; must be provided if
  `vipre_restore_local` is `true`.
- `vipre_restore_site_guid`: Specify the value of the Site GUID to restore to
  the registry; default is `""`, which doesn't restore the Site GUID.
- `vipre_restore_reg_code`: Specify the value of the Registration Code to
  restore to the registry; default is `""`, which doesn't restore the
  Registration Code.

For installs, the following variables may be used:

- `vipre_install`: Set to `true` to install or upgrade VIPRE Business, default
  is `false`. VIPRE will be upgraded when the downloaded version is lower than
  the installed version.
- `vipre_force_install`: Set to `true` to force installation even if the
  installed version is less than or equal to the downloaded version, default is
  `false`.
- `vipre_download_url`: Specify an alternate download URL for the VIPRE
  installer, default is based on `vipre_version` and `vipre_product`.
- `vipre_product_id`: Specify the product GUID that will be installed, default
  is based on `vipre_version` and `vipre_product`.

For uninstalls, the following variables may be used:

- `vipre_uninstall`: Set to `true` to uninstall VIPRE business and enable the
  various uninstall options below, default is `false`.
- `vipre_uninstall_product`: Specify the VIPRE product to uninstall, default is
  `vipre_product}}`.
- `vipre_uninstall_version`: Specify the VIPRE version to uninstall, default is
  `vipre_version`.
- `vipre_uninstall_product_id`: Specify an alternate product GUID to uninstall,
  default is based on `vipre_uninstall_product` and `vipre_uninstall_version`.
- `vipre_uninstall_registry`: Set to `true` to remove all of the VIPRE Business
  registry keys, default is `false`.
- `vipre_uninstall_data`: Set to `true` to remove all of the VIPRE Business data
  files (usually in %ProgramData%), default is `false`.

Without setting at least one of `vipre_backup`, `vipre_restore`, `vipre_install`
or `vipre_uninstall` to `true`, the role will only check to see if VIPRE is
installed.

Example Playbook
----------------

The following example playbook installs an older version of VIPRE Business
Premium in the first invocation of the role, then does a backup and upgrade to
the latest version in the second invocation:

    - hosts: vipre
      vars:
        vipre_product: premium
      roles:
        - role: cchurch.vipre
          vipre_version: 9.5
          vipre_install: true
        - role: cchurch.vipre
          vipre_backup: true
          vipre_install: true

License
-------

BSD

Author Information
------------------

Chris Church <chris@ninemoreminutes.com>
