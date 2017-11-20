Collectd Plugin to Publish Facter Data
======================================

This collectd plugin uses
`facter <https://github.com/puppetlabs/facter>`__ to publish a
configured list of facts to collectd.

Configuration Parameters
------------------------

-  ``Facter`` Path to facter, defaults to ``facter``.
-  ``FacterOptions`` Extra options to facter. Defaults to empty string.
   In particular set this to ``-p`` to load custom facts.
-  ``FloatDefault`` For facts that cannot be cast to a float what value
   should be used for the metric value. The default is to not be specified
   when any facts that cannot be cast to a float are simply skipped.
-  ``Fact`` Specify a fact name that should be published. Many fact
   names can be added.
-  ``MetaData`` If ``true`` then `collectd meta
   data <https://collectd.org/wiki/index.php/Meta_Data>`__  is
   published. For example ``os.hardware`` might be published
   with ``{'value': 'x86_64'}``.



Example Configuration.
----------------------

.. code:: apache

    <Plugin "python">
      Import "collectd_facter"
      <Module "collectd_facter">
         Facter        '/opt/puppetlabs/bin/facter'
         FacterOptions '-p'
         FloatDefault  '-1'
         Fact          'os.release.major' 'system_uptime.days'
         Fact          'os.hardware'
         Fact          'my_custom_fact'
      </Module>
    </Plugin>

This example might produce data output from
`write\_http <https://collectd.org/wiki/index.php/Plugin:Write_HTTP>`__
such as.

.. code:: json

    [
      {
        "values": [
          -1
        ],
        "dstypes": [
          "gauge"
        ],
        "dsnames": [
          "value"
        ],
        "time": 1510588310.257,
        "interval": 60.0,
        "host": "bag7.cern.ch",
        "plugin": "facter",
        "plugin_instance": "os.hardware",
        "type": "gauge",
        "type_instance": "",
        "meta": {
          "value": "x86_64"
        }
      },
      {
        "values": [
          7
        ],
        "dstypes": [
          "gauge"
        ],
        "dsnames": [
          "value"
        ],
        "time": 1510588310.257,
        "interval": 60.0,
        "host": "bag7.cern.ch",
        "plugin": "facter",
        "plugin_instance": "os.release.major",
        "type": "gauge",
        "type_instance": "",
        "meta": {
          "value": "7"
        }
      },
      {
        "values": [
          2
        ],
        "dstypes": [
          "gauge"
        ],
        "dsnames": [
          "value"
        ],
        "time": 1510588310.257,
        "interval": 60.0,
        "host": "bag7.cern.ch",
        "plugin": "facter",
        "plugin_instance": "processorcount",
        "type": "gauge",
        "type_instance": "",
        "meta": {
          "value": 2
        }
      }
    ]


