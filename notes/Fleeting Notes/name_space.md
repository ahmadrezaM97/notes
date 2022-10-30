https://www.youtube.com/watch?v=8fi7uSYlOdc&t=2203s

sudo ip netns add C1
sudo uo netns add C2


sudo ip link add v-net-0 type bridge 

sudo ip link add vethC1 type veth peer vethC1-br
sudo ip link add vethC2 type veth peer vethC2-br


sudo ip link set vethC1 netns C1
sudo ip link set vethC2 netns C2

sudo ip link set vethC1-br master v-net-0
sudo ip link set vethC2-br master v-net-0

sudo ip netns exec C1 ip addr add 192.168.15.1/24 dev vethC1
sudo ip netns exec C2 ip addr add 192.168.15.2/24 dev vethC2

sudo ip netns exec C1 ip link set vetchC1 up
sudo ip netns exec C2 ip link set vetchC2 up
sudo ip link set v-net-0 upi

sudo ip link set dev vethC1-br up
sudo ip link set dev vethC2-br up 

sudo ip netns exec C2 arp

sudo sysctl net.bridge.nf-call-iptables = 0

sudo ip addr add 192.168.15.5/24 dev v-net-0

sudo up netns exec C1 ip route add default via 192.168.15.5


