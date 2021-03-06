.. The contents of this file are included in multiple topics.
.. This file should not be changed in a way that hinders its ability to appear in multiple documentation sets.


On |windows|, the |chef client| must have two entries added to the ``PATH`` environment variable:

* ``C:\opscode\chef\bin``
* ``C:\opscode\chef\embedded\bin``

This is typically done during the installation of the |chef client| automatically. If these values (for any reason) are not in the ``PATH`` environment variable, the |chef client| will not run properly.

.. image:: ../../images/includes_windows_environment_variable_path.png

This value can be set from a recipe. For example, from the |cookbook php| cookbook:

.. code-block:: ruby

   #  the following code sample comes from the ``package`` recipe in the ``php`` cookbook: https://github.com/chef-cookbooks/php
   
   if platform?('windows')
   
     include_recipe 'iis::mod_cgi'
     
     install_dir = File.expand_path(node['php']['conf_dir']).gsub('/', '\\')
     windows_package node['php']['windows']['msi_name'] do
       source node['php']['windows']['msi_source']
       installer_type :msi
   
       options %W[
         /quiet
         INSTALLDIR="#{install_dir}"
         ADDLOCAL=#{node['php']['packages'].join(',')}
       ].join(' ')
   end
   
   ...
   
   ENV['PATH'] += ";#{install_dir}"
   windows_path install_dir
   
   ...
   