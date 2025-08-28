# Important command-line commands

## Adding NAT to a Private Network

    sudo iptables -t nat -A POSTROUTING -s 10.45.0.0/16 ! -o ogstun -j MASQUERADE
    sudo ip ro add 10.45.0.0/16 via 10.53.1.2

## start Core Network

    docker compose up 5gc

## start gNB

    sudo ./gnb -c gnb_rf_b200_tdd_n78_20mhz.yml

