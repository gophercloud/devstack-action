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
