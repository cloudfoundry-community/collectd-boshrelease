# Run collectd with BOSH

A super basic release for collectd.

* [Concourse CI](https://ci.starkandwayne.com/teams/main/pipelines/collectd-boshrelease)
* Pull requests will be automatically tested against a bosh-lite (see `testflight-pr` job)
* Latest upstream project versions are automatically added as [blobs via CI](https://ci.starkandwayne.com/teams/main/pipelines/collectd-boshrelease?groups=blobs)
* Discussions and CI notifications at [#collectd-boshrelease channel](https://cloudfoundry.slack.com/messages/C6RAUJGUF/) on https://slack.cloudfoundry.org

With basic config

```
properties:
  collectd:
    hostname_prefix: cf.collectd.warden.
    config: |
      LoadPlugin "df"
      LoadPlugin "disk"
      LoadPlugin "cpu"
      LoadPlugin "load"
      LoadPlugin "write_graphite"
      <Plugin "write_graphite">
        <Node "">
          EscapeCharacter "."
          Host "localhost"
          Port "2003"
        </Node>
      </Plugin>
```

You will end up with metrics that looks like

```
cf.collectd.warden.collectd_z1.0.cpu-1.cpu-steal 0.000000 1446825506
cf.collectd.warden.collectd_z1.0.cpu-2.cpu-user 0.799932 1446825506
cf.collectd.warden.collectd_z1.0.cpu-2.cpu-nice 0.000000 1446825506
cf.collectd.warden.collectd_z1.0.cpu-2.cpu-system 0.199982 1446825506
cf.collectd.warden.collectd_z1.0.cpu-2.cpu-idle 98.791028 1446825506
cf.collectd.warden.collectd_z1.0.cpu-2.cpu-wait 0.000000 1446825506
cf.collectd.warden.collectd_z1.0.cpu-2.cpu-interrupt 0.000000 1446825506
cf.collectd.warden.collectd_z1.0.cpu-2.cpu-softirq 0.000000 1446825506
cf.collectd.warden.collectd_z1.0.cpu-2.cpu-steal 0.000000 1446825506
cf.collectd.warden.collectd_z1.0.disk-sda1.disk_ops.read 0.000000 1446825506
cf.collectd.warden.collectd_z1.0.disk-sda1.disk_ops.write 0.000000 1446825506
cf.collectd.warden.collectd_z1.0.disk-sda1.disk_time.read 0.000000 1446825506
```

## Usage

To use this bosh release:

```
git clone https://github.com/cloudfoundry-community/collectd-boshrelease.git
cd collectd-boshrelease

export BOSH_ENVIRONMENT=<alias>
export BOSH_DEPLOYMENT=haproxy
bosh2 deploy manifests/collectd.yml
```

The default manifest emits cpu, disk, df and load data every 5 sec to
graphite on localhost. If you wanna see this data simply

```
bosh ssh collectd_z1/0
nc -l -p 2003
```
