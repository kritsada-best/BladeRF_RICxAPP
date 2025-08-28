# Important command-line commands

## Adding NAT to a Private Network
### path : ~/

    sudo sysctl -w net.ipv4.ip_forward=1
    sudo iptables -t nat -A POSTROUTING -s 10.45.0.0/16 ! -o ogstun -j MASQUERADE
    sudo ip ro add 10.45.0.0/16 via 10.53.1.2

## start Core Network
### path : ~/srsRAN_Project/docker 

    docker compose up 5gc

## start gNB
### path : ~/srsRAN_Project/build/apps/gnb

    sudo ./gnb -c gnb_rf_b200_tdd_n78_20mhz.yml

## start RIC
### path : ~/oran-sc-ric

    docker compose up

## start xAPP
### path : ~/oran-sc-ric

    docker compose exec python_xapp_runner ./name_your_xApp.py --metrics=DRB.UEThpDl,DRB.UEThpUl --kpm_report_style=5

