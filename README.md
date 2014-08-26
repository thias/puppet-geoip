# puppet-geoip

## Overview

Install any of the GeoIP database files.

* `geoip::conf` : Class to manage `GeoIP.conf` with your MaxMind account info.
* `geoip::file` : Definition to install any GeoIP database file.

There is no magic here, you need to have the `Geo*.dat` files available on
your puppetmaster somewhere, and reference them.

You can use `geoip::conf` on your puppetmaster and create a cron job which
will update databases regularly.

## Examples

Set defaults in `site.pp` :

```puppet
Geoip::File {
  path        => '/usr/local/share',
  source_path => 'puppet:///modules/mymodule/GeoIP',
}
```

For a node where nginx will have its geoip module enabled :

```puppet
geoip::file { 'GeoIP.dat':
  before => Service['nginx'],
}
```

For a node which will be downloading updated files from MaxMind directly
using the `geoipupdate` script :

```puppet
class { 'geoip::conf':
  userid     => '12345',
  licensekey => 'abcdefg12345',
  productids => 'GeoIP2-Country 106',
  group      => 'wheel',
  mode       => '0640',
}
```

