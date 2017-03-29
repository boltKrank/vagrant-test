# vagrant-test

This is to get the test envirnoment up. It consists of:

1. master.demo.internal (centos-7)
2. agent1.demo.internal (centos-7)
2. agent2.demo.internal (centos-7)


Setup steps:

1. vagrant up
2. eval `ssh-agent -s`
3. ssh-add ssh_keys/id_rsa
4. bundle exec puppetizer all
