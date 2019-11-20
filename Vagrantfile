ENTRY_POINT = '192.168.42.20'
WEB_INSTANCE_COUNT = 2
DB_ADDRESS = '192.168.42.30'
DB_PORT = "5432"
DB_NAME = 'auctiondb'
DB_USER = 'auctionuser'
DB_PASSWORD = 'passw0rd'
WEB_BASE = "192.168.42."
WEB_START = 40
WEB_PORT = 3000

Vagrant.configure('2') do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.memory = 1024

  config.vm.define "db" do |cfg|
    cfg.vm.hostname = "db"
    cfg.vm.network "private_network", ip: DB_ADDRESS
    cfg.vm.provision "shell",
      path: "init_database_vm",
      privileged: true,
      env: {
	DB_ADDRESS: DB_ADDRESS,
        DB_NAME: DB_NAME,
        DB_PASSWORD: DB_PASSWORD,
        DB_USER: DB_USER,
      }
  end

  (0..(WEB_INSTANCE_COUNT - 1)).each do |index|
    config.vm.define "web#{index}" do |cfg|
      cfg.vm.hostname = "web#{index}"
      cfg.vm.network "private_network", ip: "192.168.42.#{40 + index}"
      cfg.vm.provision 'shell',
        path: 'init_web_server',
        privileged: true,
        env: {
	  WEB_PORT: WEB_PORT,
	  DB_ADDRESS: DB_ADDRESS,
          DB_PORT: DB_PORT,
          DB_USER: DB_USER,
	  DB_PASSWORD: DB_PASSWORD,
        }
      cfg.vm.post_up_message = "Application server #{cfg.vm.hostname} is available."
    end
  end

  config.vm.define 'lb' do |cfg|
    cfg.vm.hostname = 'lb'
    cfg.vm.network 'private_network', ip: ENTRY_POINT
    cfg.vm.network 'forwarded_port', guest: 80, host: 8085
    cfg.vm.provision 'shell',
       path: 'init_load_balancer',
       env: {
         WEB_BASE: WEB_BASE,
         WEB_START: WEB_START,
	 INSTANCES: WEB_INSTANCE_COUNT,
	 WEB_PORT: WEB_PORT,
       }
    cfg.vm.post_up_message = "The load balancer is available at http://#{ENTRY_POINT}"
  end
end
