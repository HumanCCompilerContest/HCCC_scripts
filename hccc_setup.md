## install
sudo apt install apache2 nginx
sudo apt install gcc libssl-dev build-essential pkg-config
sudo apt install docker docker-compose
git clone https://github.com/HumanCCompilerContest/HCCC_Infra.git

## start nginx
vim /etc/nginx/nginx.conf # remove `include /etc/nginx/conf.d/*.conf;`
vim /etc/nginx/conf.d/hccc.conf
systemctl start nginx

## setup server
# https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal
sudo apt install snapd
sudo apt remove certbot # might be needed
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx


## start server
cd HCCC_Infra/
git switch HCCC_001
sudo docker-compose up
export DATABASE_URL="postgres://non_manaka@localhost:5433/hccc_judge"
cargo build --release --bin judge_server 
cargo build --release --bin web_server
