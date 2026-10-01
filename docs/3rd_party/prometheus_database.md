# Using Prometheus for data collection

The prometheus database is used for many projects for data collection. It is relatively easy to set up and use.

For installation, you can follow a [guide provided by the prometheus devs](https://prometheus.io/docs/introduction/first_steps/).

!!! danger "Important"
    The part of the API (`/api/prometheus/metrics`) is **not** the database server!
    The database is always installed on an external host and never on OpenDTU-OnBattery itself!

Here are some distro-specific guides for linux:

- Debian/Ubuntu/Raspbian: [See this guide here](https://gist.github.com/eiri/1102e1f3c168684b5a8b0e7a0f5a5a14) (Although there should also be something in the APT repos)
- Archlinux: [Archlinux Wiki / Prometheus](https://wiki.archlinux.org/title/Prometheus)

### Configuring Prometheus to scrape data from the OpenDTU
```yaml
# /etc/prometheus/prometheus.yml
scrape_configs:
  - job_name: 'opendtu'
    scrape_interval: 5s # >= Inverter scrape interval
    static_configs:
    - targets: ['<ip of first opendtu>', '<ip of second opendtu>']
    metrics_path: /api/prometheus/metrics
```

#### Authentication

The `/api/prometheus/metrics` endpoint is protected by the `ReadOnlyAccess` HTTP Basic authentication scheme of the [Web API](../firmware/web_api.md).

- If [Allow readonly access to web interface without password](../firmware/configuration/security_settings.md#allow-readonly-access-to-web-interface-without-password) is **enabled**, the example above works without credentials.
- If this setting is **disabled**, requests without credentials are answered with `401 Unauthorized` and Prometheus will report the target as down. In this case, add `ReadOnlyAccess` credentials using `basic_auth`:

```yaml
# /etc/prometheus/prometheus.yml
scrape_configs:
  - job_name: 'opendtu'
    scrape_interval: 5s # >= Inverter scrape interval
    static_configs:
    - targets: ['<ip of first opendtu>', '<ip of second opendtu>']
    metrics_path: /api/prometheus/metrics
    basic_auth:
      username: 'admin'
      password: '<admin password of OpenDTU-OnBattery>'
      # alternatively, read the password from a file:
      # password_file: /etc/prometheus/opendtu_password
```

The username is always `admin`, the password is the one configured in the [Security Settings](../firmware/configuration/security_settings.md). If you scrape multiple OpenDTU-OnBattery instances with different passwords, create a separate scrape job for each of them.

!!! warning
    The credentials are transmitted via plain HTTP and are only Base64-encoded. Only scrape OpenDTU-OnBattery from within a trusted network.

### Example Grafana Dashboards
- [https://grafana.com/grafana/dashboards/19666-opendtu/](https://grafana.com/grafana/dashboards/19666-opendtu/)
- [https://github.com/tbnobody/OpenDTU/discussions/1230#discussioncomment-6969028](https://github.com/tbnobody/OpenDTU/discussions/1230#discussioncomment-6969028)
