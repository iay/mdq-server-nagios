# mdq-server-nagios

Nagios plugin to provide monitoring of [mdq-server](https://github.com/iay/mdq-server) instances.

## Prerequisites

The script uses a number of Perl modules. Most of them should already be installed on a system
running Nagios, but `Monitoring::Plugin` may need to be acquired separately. If you are running on
a RHEL-derived system with EPEL available, you can use `yum install perl-Monitoring-Plugin`.

## Testing and Installing

To find out if the script will run on your system, you can invoke it against some of the
provided data files:

    $ ./check_mda_health -u test/up.json
    MDQ_HEALTH OK - status is UP
    $ ./check_mdq_health -u file:test/down.json
    MDQ_HEALTH CRITICAL - status is DOWN
    $ ./check_mdq_health -u file:test/degraded.json
    MDQ_HEALTH WARNING - status is DEGRADED

If everything seems fine, copy `check_mda_health` into the directory where your other Nagios
plugins live. This is normally defined as `$USER1$` in `/etc/nagios/private/resource.cfg`
as something like `/usr/lib64/nagios/plugins`.

You can then define a command based on this plugin in one of your configuration files as follows:

    define command {
        command_name    check_mdq_health
        command_line    $USER1$/check_mdq_health --url $ARG1$ $ARG2$
    }

A service check then looks like this:

    define service {
        use                   my-service-template
        host_name             mdq
        service_description   health
        check_command         check_mdq_health!http://mdq.example.org/health
    }

## Copyright and License

The entire package is Copyright (C) 2015–2023, Ian A. Young.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
