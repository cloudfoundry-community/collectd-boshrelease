# BOSH Release for collectd

A super basic release for collectd

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

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
git clone https://github.com/cloudfoundry-community/collectd-boshrelease.git
cd collectd-boshrelease
bosh upload release releases/collectd-1.yml
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```
