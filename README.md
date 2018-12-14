# vps-scrip
Auto installer
my_port () {
#CAPTURE OF VALUES
total_portas = $ (lsof -V -i tcp -P -n | grep -v "ESTABLISHED" | grep -v "COMMAND" | grep "LISTEN")
#CRIA TEMPORARY ARCHIVE
echo "$ total_portas"> ./arqtmp
#LOOP WITH THE VALUES
while read port; of
port = $ (echo $ {port} | awk '{print $ 1} | tail -1)
($ echo $ {port} | awk -F ":" '{print $ 2} | tail -1)
($ echo $ {portVAR [@]} | grep "$ port $ service") = ""]] && portVAR + = "$ port $ service \ n"
done <./arqtmp
#APAGA TEMPORARY VALUE
rm ./arqtmp
#IMPRIME RESULT
echo -e "$ portaVAR"
unset portaVAR
}

ssl_stunel () {
unset portlist
#LISTING DOORS
port_list + = ($ (echo -e "$ (my_portas | grep sshd | awk '{print $ 2}')"))
port_list + = ($ (echo -e "$ (my_portas | grep dropbear | awk '{print $ 2}')"))
if [[$ {mail_list [@]}! = ""]]; then
echo -e "\ 033 [1; 32m {txt [293]} \ 033 [1; 33m"
i = 0
for select in `echo -e $ {mail_list [@]}`; of
echo -e "[$ i] $ select"
let i ++
done
echo -ne "\ 033 [1; 32m"
while true; of
read -p "$ {txt [294]}:" portx
[[$ {port_list [$ portx]}! = ""]] && break
echo -e "$ {txt [295]}"
done
else
echo -e "\ 033 [1; 31m {txt [296]} \ 033 [0m"
sleep 3s
exit 1
fi
DPORT = "$ {port_list [$ portx]}"
unset portlist
#INSTALLING
echo -e "\ 033 [1; 33m txt [297]} \ 033 [1; 32m"
#ESCOLHENDO DOOR
while true; of
read -p "Listen Port Ssl:" SSLPORT
[[$ (my_portas | grep $ SSLPORT) = ""]] && break
echo -e "$ {SSLPORT}: $ {txt [298]}"
unset SSLPORT
done
apt-get install stunnel4 -y> / dev / null 2> & 1
echo -e "cert = /etc/stunnel/stunnel.pem \ nclient = no \ nsocket = a: SO_REUSEADDR = 1 \ nsocket = l: TCP_NODELAY = 1 \ nsocket = r: TCP_NODELAY = 1 \ n \ n [stunnel] nconnect = 127.0.0.1:${DPORT}\naccept = $ {SSLPORT} "> /etc/stunnel/stunnel.conf
openssl genrsa -out key.pem 2048
echo -e "\ 033 [1; 33m txt [299]} \ 033 [1; 32m"
openssl req -new -x509 -key key.pem -out cert.pem -days 1095
cat key.pem cert.pem >> /etc/stunnel/stunnel.pem
sed -i 's / ENABLED = 0 / ENABLED = 1 / g' / etc / default / stunnel4
service stunnel4 restart
echo -e "\ 033 [0m"
}

script_sh () {
encript=(aes-256-gcm aes-192-gcm aes-128-gcm aes-256-ctr aes-192-ctr aes-128-ctr aes-256-cfb aes-192-cfb aes-128-cfb camellia-128-cfb camellia-192-cfb camellia-256-cfb chacha20-ietf-poly1305 chacha20-ietf chacha20 rc4-md5)
s=0
echo -e " \033[1;33m════════════════════════\n${txt[300]}\n════════════════════════\n\033[1;32m"
for enc in `echo ${encript[@]}`; do
VAR_EN+="[${s}] - ${enc}\n"
s=$(($s+1))
done
echo -e "${VAR_EN}"
unset VAR_EN
echo -e " \033[1;33m════════════════════════\033[1;37m"
read -p " ${txt[301]}: " sele
encriptacao="${encript[$sele]}"
}

shadowsocks () {
[[ $(ps x|grep ssserver|grep -v grep|awk '{print $1}') != "" ]] && kill -9 $(ps x|grep ssserver|grep -v grep|awk '{print $1}') && ssserver -c /etc/shadowsocks.json -d stop
while true; do
cript_sh
[[ ${encriptacao} != "" ]] && break
echo -e "\033[1;31m ${txt[302]}\033[0m"
done
#ESCOLHENDO LISTEN
while true; do
read -p "Listen Port: " Lport
[[ $( my_port |grep "$Lport") = "" ]] && break
echo -e " ${Lport}: ${txt[298]}"
unset Lport
done
#INICIANDO
read -p" ${txt[303]} " Pass
apt-get install python-pip python-m2crypto -y
pip install shadowsocks
echo -ne '{\n"server":"' > /etc/shadowsocks.json
echo -ne "0.0.0.0" >> /etc/shadowsocks.json
echo -ne '",\n"server_port":' >> /etc/shadowsocks.json
echo -ne "${Lport},\n" >> /etc/shadowsocks.json
echo -ne '"local_port":1080,\n"password":"' >> /etc/shadowsocks.json
echo -ne "${Pass}" >> /etc/shadowsocks.json
echo -ne '",\n"timeout":600,\n"method":"aes-256-cfb"\n}' >> /etc/shadowsocks.json
echo
echo -e "\033[1;31mSTARTING\033[0m"
ssserver -c /etc/shadowsocks.json -d start
}

while true; do
echo -e "${cor[1]} ═══════════════════════════════════ ${cor[0]}"
echo -e "\033[1;36m ${txt[292]}"
echo -ne "\033[1;32m"
echo -e "${cor[1]} ═══════════════════════════════════ ${cor[0]}"
echo -e "${cor[2]} |0|?${cor[3]} ${txt[13]}"
echo -e "${cor[2]} |1|?${cor[3]} SSL"
echo -e "${cor[2]} |2|?${cor[3]} SHADOWSOCKS"
echo -e "${cor[1]} ═══════════════════════════════════ ${cor[0]}"
read -p " Opt: " optionss
[[ $optionss = "1" ]] && ssl_stunel && break
[[ $optionss = "2" ]] && shadowsocks && break
[[ $optionss = "0" ]] && break
done
source menu
