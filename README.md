Description
===========

The Syslog NG cookbook installs and configures syslog-ng service. There are two recipes

* syslog-ng enables syslog-ng but does not affect your current syslog configuration
* syslog-ng::global disables your existing syslog and configures syslog-ng to handle system logging

There are also four definitions

* syslog_ng_source configures syslog-ng to listen on a udp port
* syslog_ng_source configures a filter
* syslog_ng_file configures syslog-ng to write logs it receives to a file
* syslog_ng_forwarder configures syslog-ng to forward logs it receives to another syslog server

The path to logfiles is generated by concatenating log_base, the application name, and log_name. When setting log_base and log_name you can use syslog-ng macros. For example, log_name could be "${YEAR}/${MONTH}/${DAY}/${HOUR}.log"

The cookbook has been written for and tested on CentOS with syslog-ng 2.1.4.
Syslog NG can be obtained [here: balabit.com](http://www.balabit.com/downloads/files?path=/syslog-ng/sources/2.1.4). 

Requirements
============

* Chef 0.8+
* Syslog-NG 2.x package

Usage
=====

### In a run list:
    "run_list": [
        "recipe[syslog-ng]"
    ]

### In a cookbook:
    include_recipe "syslog-ng"
    
    syslog_ng_source "source_foo" do
      index "02"
      host "127.0.0.1"
      port "514"
    end

    syslog_ng_file "application_foo" do
      index "03"
      source_name "source_foo"
      days_uncompressed "7"
      log_base "/var/applogs"
      log_name "default.log"
    end

    syslog_ng_filter "warnings" do
      index  "04"
      filter "level(warning)"
    end

    syslog_ng_forwarder "application_foo_warnings" do
      index "05"
      source_name "source_foo"
      filter_name "warnings"
      destination_host "example.com"
      destination_port "514"
      destination_protocol "udp"
    end


License and Author
==================

Author:: Artem Veremey (<artem@veremey.net>)

Copyright 2012, Artem Veremey

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

Changes
=======

### v 1.3.0

* Create filter definition and have file and forwarder optionally take a filter

### v 1.2.0

* Break source out into its own definition

### v 1.1.0

* adding a new definition for configuring forwarding
* renaming the defintion that writes files to make that clearer in the name
* in the definition for writing files, allow specifying file name
* in the definition for writing files, compress old log files
* moving system logging configuration from the default recipe to a new recipe


### v 1.0.0

* Initial public release
