sudo apt-get update  
sudo apt-get install iptables-persistent git -y  
sudo git clone https://github.com/unixabg/RPI-Wireless-Hotspot.git  
cd RPI-Wireless-Hotspot  
sudo ./install  
sudo apt-get install tor  
sudo nano /etc/tor/torrc  

sudo iptables -F  
sudo iptables -t nat -F  
sudo iptables -t nat -A PREROUTING -i wlan2 -p tcp --dport 22 -j REDIRECT --to-ports 22  
sudo iptables -t nat -A PREROUTING -i wlan2 -p udp --dport 53 -j REDIRECT --to-ports 53  
sudo iptables -t nat -A PREROUTING -i wlan2 -p tcp --syn -j REDIRECT --to-ports 9040  
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"  
sudo service tor status  


sudo iptables -t nat -A PREROUTING -s 10.8.0.100 -p tcp --dport 22 -j REDIRECT --to-ports 22  
sudo iptables -t nat -A PREROUTING -s 10.8.0.100 -p udp --dport 53 -j REDIRECT --to-ports 53  
sudo iptables -t nat -A PREROUTING -s 10.8.0.100 -p tcp --syn -j REDIRECT --to-ports 9040  

iptables -t nat -A PREROUTING -s 10.8.0.0/24 -p udp -m udp --dport 53 -j REDIRECT --to-ports 53  
iptables -t nat -A PREROUTING -s 10.8.0.0/24 -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -j REDIRECT --to-ports 9040  
