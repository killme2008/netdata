<!--startmeta
custom_edit_url: "https://github.com/netdata/netdata/edit/master/src/health/notifications/alerta/README.md"
meta_yaml: "https://github.com/netdata/netdata/edit/master/src/health/notifications/alerta/metadata.yaml"
sidebar_label: "Alerta"
learn_status: "Published"
learn_rel_path: "Alerting/Notifications/Agent Dispatched Notifications"
message: "DO NOT EDIT THIS FILE DIRECTLY, IT IS GENERATED BY THE NOTIFICATION'S metadata.yaml FILE"
endmeta-->

# Alerta


<img src="https://netdata.cloud/img/alerta.png" width="150"/>


The [Alerta](https://alerta.io/) monitoring system is a tool used to consolidate and de-duplicate alerts from multiple sources for quick ‘at-a-glance’ visualization. With just one system you can monitor alerts from many other monitoring tools on a single screen.
You can send Netdata alerts to Alerta to see alerts coming from many Netdata hosts or also from a multi-host Netdata configuration.



<img src="https://img.shields.io/badge/maintained%20by-Netdata-%2300ab44" />

## Setup

### Prerequisites

#### 

- A working Alerta instance
- An Alerta API key (if authentication in Alerta is enabled)
- Access to the terminal where Netdata Agent is running



### Configuration

#### File

The configuration file name for this integration is `health_alarm_notify.conf`.


You can edit the configuration file using the `edit-config` script from the
Netdata [config directory](https://github.com/netdata/netdata/blob/master/docs/configure/nodes.md#the-netdata-config-directory).

```bash
cd /etc/netdata 2>/dev/null || cd /opt/netdata/etc/netdata
sudo ./edit-config health_alarm_notify.conf
```
#### Options

The following options can be defined for this notification

<details><summary>Config Options</summary>

| Name | Description | Default | Required |
|:----|:-----------|:-------|:--------:|
| SEND_ALERTA | Set `SEND_ALERTA` to YES |  | yes |
| ALERTA_WEBHOOK_URL | set `ALERTA_WEBHOOK_URL` to the API url you defined when you installed the Alerta server. |  | yes |
| ALERTA_API_KEY | Set `ALERTA_API_KEY` to your API key. |  | yes |
| DEFAULT_RECIPIENT_ALERTA | Set `DEFAULT_RECIPIENT_ALERTA` to the default recipient environment you want the alert notifications to be sent to. All roles will default to this variable if left unconfigured. |  | yes |
| DEFAULT_RECIPIENT_CUSTOM | Set different recipient environments per role, by editing `DEFAULT_RECIPIENT_CUSTOM` with the environment name of your choice |  | no |

##### ALERTA_API_KEY

You will need an API key to send messages from any source, if Alerta is configured to use authentication (recommended). To create a new API key:
1. Go to Configuration > API Keys.
2. Create a new API key called "netdata" with `write:alerts` permission.


##### DEFAULT_RECIPIENT_CUSTOM

The `DEFAULT_RECIPIENT_CUSTOM` can be edited in the following entries at the bottom of the same file:

```conf
role_recipients_alerta[sysadmin]="Systems"
role_recipients_alerta[domainadmin]="Domains"
role_recipients_alerta[dba]="Databases Systems"
role_recipients_alerta[webmaster]="Marketing Development"
role_recipients_alerta[proxyadmin]="Proxy"
role_recipients_alerta[sitemgr]="Sites"
```

The values you provide should be defined as environments in `/etc/alertad.conf` with `ALLOWED_ENVIRONMENTS` option.


</details>

#### Examples

##### Basic Configuration



```yaml
#------------------------------------------------------------------------------
# alerta (alerta.io) global notification options

SEND_ALERTA="YES"
ALERTA_WEBHOOK_URL="http://yourserver/alerta/api"
ALERTA_API_KEY="INSERT_YOUR_API_KEY_HERE"
DEFAULT_RECIPIENT_ALERTA="Production"

```


## Troubleshooting

### Test Notification

You can run the following command by hand, to test alerts configuration:

```bash
# become user netdata
sudo su -s /bin/bash netdata

# enable debugging info on the console
export NETDATA_ALARM_NOTIFY_DEBUG=1

# send test alarms to sysadmin
/usr/libexec/netdata/plugins.d/alarm-notify.sh test

# send test alarms to any role
/usr/libexec/netdata/plugins.d/alarm-notify.sh test "ROLE"
```

Note that this will test _all_ alert mechanisms for the selected role.

