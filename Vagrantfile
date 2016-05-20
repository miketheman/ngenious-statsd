# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "ngenious"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.provision "shell", inline: $script
end

$script = <<SCRIPT
## Set Up nginx.org repo
sudo bash -c "cat > /etc/apt/sources.list.d/nginx.list <<EOF
deb http://nginx.org/packages/ubuntu/ trusty nginx
deb-src http://nginx.org/packages/ubuntu/ trusty nginx
EOF"
# add signing key
curl -s http://nginx.org/packages/keys/nginx_signing.key | sudo apt-key add -

# Get build dependencies
sudo apt-get update
sudo apt-get install --yes build-essential git ngrep
sudo apt-get build-dep --yes nginx

# Get nginx source
cd && apt-get source nginx

# Get nginx-statsd module and apply a patch to to the source to support nginx 1.9.x and above
git clone https://github.com/zebrafishlabs/nginx-statsd.git
cd ~/nginx-statsd && curl -L https://github.com/zebrafishlabs/nginx-statsd/pull/20.patch | patch

cd ~/nginx-1.10.0/
# Tell the packaging scripts where to get the addon
sed -i -e 's|^NGX_ADDONS=$|NGX_ADDONS=~/nginx-statsd/|g' auto/options

# Build and install
dpkg-buildpackage -b && cd && sudo dpkg -i nginx_1.10.0-1~trusty_amd64.deb

sudo ln -sf /vagrant/config/default.conf /etc/nginx/conf.d/default.conf && sudo service nginx reload
SCRIPT
