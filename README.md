[![.github/workflows/with-defaults.yml](https://github.com/gophercloud/devstack-action/actions/workflows/with-defaults.yml/badge.svg)](https://github.com/gophercloud/devstack-action/actions/workflows/with-defaults.yml)

# devstack-actions
Github actions which will install OpenStack with devstack.

## Azure APT mirror workaround

By default, the action removes `azure.archive.ubuntu.com` entries from
`/etc/apt/apt-mirrors.txt` before its first APT operation to avoid stalled
package downloads. Other mirrors and their priorities are preserved. If the
mirror-list file does not exist, the workaround is skipped. APT sources that
reference the Azure mirror directly are not changed.

Set `enable_workaround_azure_mirror: 'false'` in the step's `with:` block to
disable the workaround.

## CirrOS prefetch

By default, the action downloads the CirrOS `uec.tar.gz` and `disk.img` images
from GitHub Releases into `devstack/files/` before running devstack. Devstack
skips its own download when the file is already there. This avoids timeouts
from `download.cirros-cloud.net`, which the Ironic devstack plugin hardcodes.
The version comes from `CIRROS_VERSION` in `conf_overrides`, or from devstack's
`stackrc`. A failed download only logs a warning, and devstack then downloads
the image itself.

Set `prefetch_cirros: 'false'` in the step's `with:` block to disable it.

## Usage

Example of Github Action step:

With defaults:
```
    steps:
      - name: Deploy devstack
        uses: gophercloud/devstack-action@v0.6
```

With overrides:
```
    steps:
      - name: Deploy devstack
        uses: gophercloud/devstack-action@v0.6
        with:
          branch: stable/xena
          conf_overrides:
            CINDER_ISCSI_HELPER=tgtadm
          enabled_services: 's-account,s-container,s-object,s-proxy'
```
