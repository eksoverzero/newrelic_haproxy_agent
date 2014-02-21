## New Relic Haproxy monitoring Plugin

Repurposed from https://github.com/railsware/newrelic_platform_plugins/tree/master/newrelic_haproxy_agent

The New Relic Haproxy Plugin enables monitoring of HAProxy – a TCP/HTTP load balancer – and reports the following data for a specified proxy:

* Error Rate (per-min)
* Proxy Status
* Request Rate (per-min)
* Active Servers
* Sessions Active
* Sessions Queued

### Requirements

In order to use this plugin, you must have an active New Relic account.

Plugin should work on any generic Unix environment with the following
software components installed:

  - Ruby (>= 1.8.7)
  - bundler for Ruby: https://github.com/carlhuda/bundler

### Instructions for running the Haproxy plugin agent

1. run `bundle install` to install required gems
2. Copy `config/newrelic_plugin.yml.example` to `config/newrelic_plugin.yml`
3. Edit `config/newrelic_plugin.yml` and replace "YOUR_LICENSE_KEY_HERE" with your New Relic license key
4. Edit the `config/newrelic_plugin.yml` file and add the Haproxy status URL
5. Running the plugin

In order to check your configuration, you can launch the plugin
in foreground mode, with all output going to stdout:

  ./newrelic_haproxy_agent

Carefully check plugin's output for any possible error messages.
In case of success, collected data should appear in the New Relic
user interface shortly after starting.

Plugin can also be started as a daemon using the following command:

  ./newrelic_haproxy_agent.daemon start

In this case you can check its status by running

  ./newrelic_haproxy_agent.daemon status

and stop it with

  ./newrelic_haproxy_agent.daemon stop

### Using Monit to keep the Agent running

On Ubuntu: `sudo apt-get install monit`

Example config file:

```
# /etc/monit/conf.d/newrelic_haproxy_agent.conf
check process newrelic_haproxy_agent
  with pidfile /home/ubuntu/newrelic_memacached_agent/newrelic_haproxy_agent.pid
  start program = "/bin/su - ubuntu -c '/home/ubuntu/newrelic_haproxy_agent/newrelic_haproxy_agent.daemon start'" with timeout 90 seconds
  stop program = "/bin/su - ubuntu -c '/home/ubuntu/newrelic_haproxy_agent/newrelic_haproxy_agent.daemon stop'" with timeout 90 seconds
  if totalmem is greater than 250 MB for 2 cycles then restart
  group newrelic_agent
```
