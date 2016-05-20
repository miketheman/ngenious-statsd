# ngenious-statsd

A sample project for compiling ngins with a statsd module to emit metrics

Uses https://github.com/zebrafishlabs/nginx-statsd

## Run

Use Vagrant:
```bash
vagrant up
```

Once complete, load a couple of sites:
```
open http://localhost:8080
open http://localhost:8080/basic_status
```

If you make any config for content changes, reload nginx:
```
vagrant ssh -c 'sudo service nginx reload'
```

Watch traffic with `ngrep -d lo -W byline port 8125`

