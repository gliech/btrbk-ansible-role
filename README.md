# [Btrbk Ansible Role][1]

[![test & release][2]][3]


This Ansible role installs btrbk from the OS package manager and allows to
configure it with the provided or your own configuration file and a systemd
timer.

## Requirements

Optionally requires SSH if backing up to or from remote targets.

## Role Variables

<table>
<tr><th>Name</th><th>Required</th><th>Type / Choices</th><th>Description</th></tr>
<tr><td><code>btrbk_config_file</code></td>
<td>no</td>
<td>string</td>
<td>

Name of the `btrbk.conf` file to use. You can either use a basic configuration
provided with this module or use the name of a file that you provide yourself.
The file is read as a Jinja template, so if you provide it yourself you can
incorporate additional logic, and the file should be placed in a location where
Ansible will look for templates.

**Default:** `"basic_btrbk.conf.j2"`
</td></tr>


<tr><td><code>btrbk_volume</code></td>
<td>no</td>
<td>string</td>
<td>

If you are using a `btrbk.conf` that operates on a single btrfs volume, you can
use this variable to provide the mount path of the volume. If both this and the
`btrbk_snapshot_dir` variables are provided, the snapshot directory will be
created automatically.

**Example:** `"/mnt/storage"`
</td></tr>


<tr><td><code>btrbk_snapshot_dir</code></td>
<td>no</td>
<td>string</td>
<td>

If you are using a `btrbk.conf` that uses only one volume or the same name for
all instances of the snapshot\_dir, you can use this variable to provide the path
to the snapshot directory. If both this and the `btrbk_volume` variables are
provided, the snapshot directory will be created automatically.

**Default:** `"snapshots"`
</td></tr>


<tr><td><code>btrbk_timer_schedule</code></td>
<td>no</td>
<td>string</td>
<td>

Systemd onCalendar expression that describes how often btrbk should run. Set
to an empty string if you do not want to use the systemd timer.

**Default:** `"hourly"`
</td></tr>
</table>

If you use the `basic_btrbk.conf.j2` provided in this role, the following
variables are used in addition to the ones mentioned above

<table>
<tr><th>Name</th><th>Required</th><th>Type / Choices</th><th>Description</th></tr>
</td></tr>


<tr><td><code>btrbk_volume</code></td>
<td>yes</td>
<td>string</td>
<td>

Mount path of the volume.

**Example:** `"/mnt/storage"`
</td></tr>


<tr><td><code>btrbk_snapshot_dir</code></td>
<td>yes</td>
<td>string</td>
<td>

Path to the snapshot directory in the `btrbk_volume`.

**Default:** `"snapshots"`
</td></tr>


<tr><td><code>btrbk_subvolumes</code></td>
<td>yes</td>
<td>list(string)</td>
<td>

List of subvolume paths in `btrbk_volume` that should be backed up.

**Example:** `["root", "home"]`
</td></tr>
</table>


## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  tasks:
    - ansible.builtin.import_role:
        name: gliech.btrbk
      vars:
        btrbk_volume: /storage/system_root
        btrbk_subvolumes:
          - root
          - home
```

## License

This project is licensed under the terms of the [GNU General Public License v3.0](LICENSE)

[1]: https://galaxy.ansible.com/ui/standalone/roles/gliech/btrbk/
[2]: https://github.com/gliech/btrbk-ansible-role/actions/workflows/release.yml/badge.svg
[3]: https://github.com/gliech/btrbk-ansible-role/actions/workflows/release.yml
[4]: https://github.com/gliech/semantic-release-config-github-ansible-role
